---
title: 记录一个 MoviePilot NeoDB 插件
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

我开发了一个 MoviePilot NeoDB 插件
<!--more-->

## 前言

[MoviePilot](https://github.com/jxxghp/MoviePilot) 是一个非常好用的 NAS 影音平台自动化工具，其前身为 [NasTool](https://github.com/NAStool/nas-tools)。MP 通过插件的方式支持了很多额外功能，包括豆瓣想看自动订阅。但是我最近[弃用豆瓣](/posts/neodb-in-hugo/)了，所以我在原有的[豆瓣插件](https://github.com/jxxghp/MoviePilot-Plugins/blob/main/plugins/doubansync/__init__.py)上开发了一个 NeoDB 插件，可以做到在 NeoDB 上点想看，插件运行自动添加订阅并执行下载。

{{<image src="https://r2.hcplantern.top/2024/03/04/1709554249.png" caption="MoviePilot 界面">}}

## 给 MoviePilot 开发插件

MP 插件本质上是一个 Python 脚本，它可以通过调用 MP 的 API 来实现自动化功能。[插件仓库在这里](https://github.com/jxxghp/MoviePilot-Plugins)。插件的开发非常简单，只需要继承 `_PluginBase` 类，然后实现主要的几个方法即可。其实大部分方法都和豆瓣同步的插件一致，基本只需要修改`sync`这一个方法即可。

插件实现的思路是，首先获取插件配置页面的 NeoDB 用户 Token。Token 可以是多个账户的。随后通过 NeoDB 的 API 获取用户的想看记录，然后通过 MP 的 API 添加订阅。

部分代码如下：

```python
    def sync(self):
        """
        通过用户RSS同步豆瓣想看数据
        """
        if not self._tokens:
            return
        # 遍历所有用户
        for token in self._tokens.split(","):
            if not token:
                continue
            # 请求头含 Token
            headers = {"Authorization": f"Bearer {token}"}
            # 获取用户名
            username = self.__get_username(token)
            # 同步每个 NeoDB 用户的数据
            logger.info(f"开始同步 NeoDB 用户 {username} 的想看数据 ...")
            results = []
            try:
                movie_response = requests.get(self._movie_url, headers=headers)
                movie_response.raise_for_status()
                tv_response = requests.get(self._tv_url, headers=headers)
                tv_response.raise_for_status()

                try:
                    results = movie_response.json().get("data", []) + tv_response.json().get("data", [])
                except ValueError:
                    logger.error("用户数据解析失败")
                    continue
            except Exception as e:
                logger.error(f"获取数据失败：{str(e)}")
                continue
            if not results:
                logger.info(f"用户 {username} 没有想看数据")
                continue
            # 遍历该用户的所有想看条目
            for result in results:
                try:
                    # Take the url as the unique identifier. For example: /movie/2fEdnxYWozPayayizQmk5M
                    item_id = result['item']['url']
                    title = result['item']['title']
                    category = result['item']['category']
                    api_url = result['item']['api_url']
                    # 判断是否在天数范围内
                    if not self.__is_in_date_range(result.get("created_time"), title):
                        continue
                    # 检查是否处理过
                    if not item_id or item_id in [h.get("neodb_id") for h in history]:
                        logger.info(f'标题：{title}，NeoDB ID：{item_id} 已处理过')
                        continue
                    # 获取条目详细信息 item_info
                    try:
                        item_info = requests.get(f"https://neodb.social{api_url}").json()
                    except Exception as e:
                        logger.error(f"获取条目信息失败：{str(e)}")
                        continue
            # 随后通过 MP API 识别媒体信息、添加订阅、下载、存储插件历史记录等
```

## [NeoDB API](https://neodb.social/developer/)

简要记录一下用到的 NeoDB API

### User

`/api/me`: 获取用户基本信息

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

`/api/me/shelf/{type}?category={category}`: 用户的记录，`type=wishlist` 就是想看列表

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

这个 API 可以获取到详细的电影或者剧集信息。这个 API 相比上一个多了影视条目日期。

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

NeoDB 的开放性和丰富的 API 让开发插件变得非常容易~~豆瓣出来挨打~~，希望它能够成为一个更好的影视记录平台。

如果你也是 MP 和 NeoDB 用户，欢迎在这里或者[插件仓库](https://github.com/jxxghp/MoviePilot-Plugins)提出更多问题和建议。