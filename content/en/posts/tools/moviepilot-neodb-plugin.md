---
title: A MoviePilot NeoDB Plugin
date: 2024-03-04
tags:
  - neodb
  - moviepilot
categories:
  - tools
keywords:
  - NeoDB
  - MoviePilot
---

I developed a MoviePilot NeoDB plugin
<!--more-->

## Preface

[MoviePilot](https://github.com/jxxghp/MoviePilot) is a very handy NAS media platform automation tool, formerly known as [NasTool](https://github.com/NAStool/nas-tools). MP supports many additional features through plugins, including automatic subscription for DouBan's watchlist. However, I recently [abandoned DouBan](/posts/neodb-in-hugo/), so I developed a NeoDB plugin based on the original [DouBan plugin](https://github.com/jxxghp/MoviePilot-Plugins/blob/main/plugins/doubansync/__init__.py), which allows for adding subscriptions and executing downloads automatically when marking something as "want to watch" on NeoDB.

{{<image src="https://r2.hcplantern.top/2024/03/04/1709554249.png" caption="MoviePilot Interface">}}

## Developing Plugins for MoviePilot

A MP plugin is essentially a Python script that can automate tasks through calling MP's API. [The plugin repository is here](https://github.com/jxxghp/MoviePilot-Plugins). Developing a plugin is very simple; you only need to inherit the `_PluginBase` class and then implement a few main methods. In fact, most methods are consistent with those of the DouBan sync plugin, and you basically only need to modify the `sync` method.

The idea behind the plugin implementation is to first obtain the NeoDB user Token from the plugin configuration page. There can be multiple accounts' Tokens. Then, it retrieves the user's watchlist records through NeoDB's API and adds subscriptions through MP's API.

Partial code as follows:
```python
    def sync(self):
        """
        Sync wish-to-watch data from users' RSS
        """
        if not self._tokens:
            return
        # Iterate through all users
        for token in self._tokens.split(","):
            if not token:
                continue
            # Header includes Token
            headers = {"Authorization": f"Bearer {token}"}
            # Get username
            username = self.__get_username(token)
            # Sync data for each NeoDB user
            logger.info(f"Starting to sync wish-to-watch data for NeoDB user {username} ...")
            results = []
            try:
                movie_response = requests.get(self._movie_url, headers=headers)
                movie_response.raise_for_status()
                tv_response = requests.get(self._tv_url, headers=headers)
                tv_response.raise_for_status()

                try:
                    results = movie_response.json().get("data", []) + tv_response.json().get("data", [])
                except ValueError:
                    logger.error("Failed to parse user data")
                    continue
            except Exception as e:
                logger.error(f"Failed to retrieve data: {str(e)}")
                continue
            if not results:
                logger.info(f"No wish-to-watch data for user {username}")
                continue
            # Iterate through all wish-to-watch entries for that user
            for result in results:
                try:
                    # Take the url as the unique identifier. For example: /movie/2fEdnxYWozPayayizQmk5M
                    item_id = result['item']['url']
                    title = result['item']['title']
                    category = result['item']['category']
                    api_url = result['item']['api_url']
                    # Check if it's within the date range
                    if not self.__is_in_date_range(result.get("created_time"), title):
                        continue
                    # Check if it's already processed
                    if not item_id or item_id in [h.get("neodb_id") for h in history]:
                        logger.info(f'Title: {title}, NeoDB ID: {item_id} has been processed')
                        continue
                    # Get detailed information of the item (item_info)
                    try:
                        item_info = requests.get(f"https://neodb.social{api_url}").json()
                    except Exception as e:
                        logger.error(f"Failed to get item details: {str(e)}")
                        continue
            # Then, identify media information, add subscriptions, download, and store plugin history records through MP API
```

## [NeoDB API](https://neodb.social/developer/)

A brief record of the NeoDB API used

### User

`/api/me`: Get basic user information获取用户基本信息

```json
{
    "url": "https://neodb.social/users/{userid}/",
    "external_acct": "{mastodon_account_id}",
    "display_name": "{name}",
    "avatar": "{avatar_url}",
    "username": "{username}"
}
```

### User Shelf

`/api/me/shelf/{type}?category={category}`: User record，`type=wishlist` is wishlist

```json
{
    "data": [
        {
            "shelf_type": "wishlist",
            "visibility": 0,
            "item": {
                "id": "https://neodb.social/tv/season/0VrO1Nw9ykr3Zunv0eDR8U",
                "type": "TVSeason",
                "uuid": "0VrO1Nw9ykr3Zunv0eDR8U",
                "url": "/tv/season/0VrO1Nw9ykr3Zunv0eDR8U",
                "api_url": "/api/tv/season/0VrO1Nw9ykr3Zunv0eDR8U",
                "category": "tv",
                "parent_uuid": "6dlSjBV09DyK9LnZgRfdgs",
                "display_title": "环形物语",
                "external_resources": [
                    {
                        "url": "https://www.themoviedb.org/tv/93784/season/1"
                    },
                    {
                        "url": "https://movie.douban.com/subject/30277286/"
                    }
                ],
                "title": "环形物语",
                "brief": "亚马逊预定了一部8集的科幻剧集《环形故事》(Tales From The Loop)，基于瑞典艺术家Simon Stålenhag创作的科幻画作，Nathaniel Halpern(《大群》)编写剧本，马克·罗曼尼克(《别让我走》《黑胶时代》)执导，马特·里夫斯的制片公司6th & Idaho、瑞典的Indio、Fox 21联合制作。\n画作探索了一群生活在“The Loop”之上的人民的生活，The Loop是一个为了探索和解开宇宙奥秘的机器，它使以前只有科幻小说里才有的东西成为可能。在这个奇幻、神秘的小镇上，讲述着辛酸的人类故事，讲述着赤裸裸的普遍情感体验，同时又利用了流派叙事的计谋。",
                "cover_image_url": "https://neodb.social/m/movie/2021/09/1531b8acab-434d-440d-a2c1-a93583df3d97.jpg",
                "rating": 8.1,
                "rating_count": 185
            },
            "created_time": "2024-02-29T08:07:18.674Z",
            "comment_text": null,
            "rating_grade": null,
            "tags": []
        },
        ...... # more items
    ],
    "pages": 1,
    "count": 10
}
```

### Movie / TV item details

`/api/movie/{item_uuid}` or `/api/tv/season/{tv_season_uuid}`

```json
{
    "id": "https://neodb.social/tv/season/0ln9v5Y0Syxrb9Ulch6Nji",
    "type": "TVSeason",
    "uuid": "0ln9v5Y0Syxrb9Ulch6Nji",
    "url": "/tv/season/0ln9v5Y0Syxrb9Ulch6Nji",
    "api_url": "/api/tv/season/0ln9v5Y0Syxrb9Ulch6Nji",
    "category": "tv",
    "parent_uuid": "2AkFx1oyBBRNOAjxOl92Mq",
    "display_title": "小学风云 第一季",
    "external_resources": [
        {
            "url": "https://movie.douban.com/subject/35650735/"
        }
    ],
    "title": "小学风云 第一季",
    "brief": "一群敬业、热情的老师和一位略显失聪的校长聚集在费城的一所公立学校，决心帮助学生在生活中取得成功。",
    "cover_image_url": "https://neodb.social/m/item/doubanmovie/2023/04/04/540d923a-ed71-4ffc-a64e-968ef1387135.jpg",
    "rating": 8.0,
    "rating_count": 7,
    "season_number": 1,
    "orig_title": "Abbott Elementary",
    "other_title": [
        "雅培小学"
    ],
    "director": [
        "兰德尔·艾因霍恩",
        "马特·佐恩"
    ],
    "playwright": [
        "昆塔·布伦森",
        "乔丹·坦普尔"
    ],
    "actor": [
        "昆塔·布伦森",
        "博德希·戴尔",
        "泰勒·詹姆斯·威廉姆",
        "丽萨·安·沃尔特",
        "雪莉·李·拉尔夫",
        "克里斯·佩尔费蒂",
        "贾内尔·詹姆斯 ",
        "威廉·斯坦福德·戴维斯",
        "约什·拉策",
        "莱拉·盖夫曼",
        "西安·布罗德纳克斯",
        "萨德·迪瓦恩",
        "伦敦·卡温顿",
        "戴维斯·梅森",
        "乔治·沙佩尔森"
    ],
    "genre": [
        "喜剧"
    ],
    "language": [
        "英语"
    ],
    "area": [
        "美国"
    ],
    "year": 2021,
    "site": "",
    "episode_count": 13,
    "episode_uuids": []
}
```

## 结语

NeoDB's openness and rich API make plugin development very easy ~~DouBan is shit~~, hoping it can become a better platform for movie and TV show tracking.

If you are also a MP and NeoDB user, feel free to ask more questions or make suggestions here or in the [plugin repository](https://github.com/jxxghp/MoviePilot-Plugins).
