---
title: 将 NeoDB 记录整合到 Hugo 中
date: 2024-01-14
tags:
  - neodb
  - hugo
  - 书影音
categories:
  - skills
keywords:
  - NeoDB
  - Hugo
  - 观影
---

本文介绍通过 NeoDB API 和 Shortcode 在 Hugo 中创建观影页面的方法。
<!--more-->

{{<center-quote>}}
NeoDB 致力于为联邦宇宙居民提供一个自由开放互联的书籍、电影、音乐和游戏收藏评论空间。

– NeoDB
{{</center-quote>}}

## 什么是 NeoDB

{{<admonition info "NeoDB" true>}}
NeoDB 是一个去中心化的开源的社区平台，支持书籍、电影、剧集、播客、游戏、演出等多种类型的收藏。

相比于国内的豆瓣，NeoDB
- 不存在审查制度，也没有广告。
- 外文书和冷门内容收录更全面。
- 数据可方便地[从豆瓣导入](https://about.neodb.social/doc/howto/)，也可导出。

{{</admonition >}}

- 这里是我的 NeoDB 主页：
{{<link "https://neodb.social/users/will101" "NeoDB" "我的 NeoDB 主页" true>}}

## 通过 API 获取 NeoDB 数据

NeoDB 提供了开放的 API，可以通过 API 获取用户的书影音记录。
{{<admonition info "该 API 的用法" true>}}

- URL 为 `https://neodb.social/api/me/shelf`
- 需要 Bearer Token
- 指定 Path Variable `type` 为 `wishlist`, `progress`, `complete` 中的一个
- (可选) 指定 Query Param `category` 为 `book`, `movie`, `tv`, `podcast`, `music`, `game`, `performance` 中的一个
- (可选) 指定 Query Param `page` 为页码

例如 `https://neodb.social/api/me/shelf/complete?category=book&page=1`
{{</admonition >}}

获取 Access Token 的方法：[NeoDB Developer#How to authorize](https://neodb.social/developer/), 也可以生成 Test Access Token 用于测试。

为了方便使用、封装 TOKEN 信息，我们可以将获取 API 独立部署成一个 Cloudflare Worker。

1. 在 Cloudflare Workers 中创建一个新的 Worker
2. [设置环境变量](https://developers.cloudflare.com/workers/configuration/environment-variables/) `NEODB_TOKEN`，值为刚刚获取的 Access Token
3. `worker.js` 内容如下：

```js
const myBearer = NEODB_TOKEN; // Assuming 'NEODB_TOKEN' is set in your Cloudflare Worker's environment variables

addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  try {
    console.log(myBearer)
    const url = new URL(request.url);
    const category = url.pathname.substring(1);

    // Optionally, handle query parameters (e.g., page number)
    const page = url.searchParams.get('page') || '1';
    // Available values : wishlist, progress, complete
    const type = url.searchParams.get('type') || 'complete';

    let dbApiUrl = `https://neodb.social/api/me/shelf/${type}?category=${category}&page=${page}`;
    const response = await fetch(dbApiUrl, {
      method: 'get',
      headers: {
        'Accept': 'application/json',
        'Authorization': `Bearer ${myBearer}`
      }
    });

    // Check if the response from the API is OK (status code 200-299)
    if (!response.ok) {
      throw new Error(`API returned status ${response.status}`);
    }

    // Optionally, modify or just forward the API's response
    const data = await response.json();
    return new Response(JSON.stringify(data), {
      headers: { 'Content-Type': 'application/json' },
      status: response.status
    });

  } catch (error) {
    // Handle any errors that occurred during the fetch
    return new Response(error.message, { status: 500 });
  }
}
```


## 通过 Hugo Shortcode 将数据整合到 Hugo 中

在 `/layouts/shortcodes` 中创建 `neodb.html`，注意替换你的 Cloudflare Worker URL：

```html
<!--Available categories: book, movie, tv, podcast, music, game, performance-->
{{ $category := .Get 0 }}
<!--Available types: wishlist, progress, complete-->
{{ $type := .Get 1 }}

{{ if eq $type "" }}
{{ $type = "complete" }}
{{ end }}

{{ $url := printf "https://your-worker-url/%s?type=%s" $category $type }}

{{ $json := getJSON $url}}

<div class="item-gallery">
    {{ range $value := first 10 $json.data }}
    {{ $item := $value.item }}
    <div class="item-card">
        <a class="item-card-upper" href="{{ $item.id }}" target="_blank" rel="noreferrer">
            <img class="item-cover" src="{{ .item.cover_image_url }}" alt="{{ .item.display_title }}">
        </a>
        {{ if .item.rating }}
        <div class="rate">
            <span><b>{{ .item.rating }}</b>🌟</span>
            <br>
            <span class="rating-count"> {{.item.rating_count}}人评分</span>
        </div>
        {{ else}}
        <div class="rate">
            <span>暂无🌟</span>
            <br>
            <span class="rating-count"> {{.item.rating_count}}人评分</span>
        </div>
        {{ end }}
        <h3 class="item-title">{{ .item.display_title }}</h3>
    </div>
    {{ end }}
</div>

<style>
    .item-gallery {
        display: flex;
        padding: 0 1rem;
        overflow-x: scroll;
        align-items: baseline;
    }

    .item-card {
        display: flex;
        flex-direction: column;
        flex: 0 0 17%;
        margin: 0 0.5rem 1rem;
        border-radius: 5px;
        transition: transform 0.2s;
        width: 8rem;
    }

    .item-card:hover {
        transform: translateY(-5px);
    }

    .rate {
        text-align: center;
    }

    .rating-count {
        font-size: 0.8rem;
        color: grey;
    }

    .item-cover {
        width: 100%;
        min-height: 3rem;
        border: 2px solid transparent;
    }

    .item-title {
        font-size: 1rem;
        text-align: center;
        margin: 0;
    }

</style>
```

然后，在页面中使用 shortcode 引用即可：

```markdown
(去除下列 shortcodes 中的 `\`)

## 想读的书

\{\{< neodb book wishlist>\}\}

## 读过的书

\{\{< neodb book complete>\}\}
```

## 效果示例

![NeoDB 示例](https://r2.hcplantern.top/2024/01/14/1705232044.png)
