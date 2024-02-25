---
title: Integrate NeoDB page in Hugo
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

This article explains how to create a movie viewing page in Hugo using the NeoDB API and Shortcode.
<!--more-->

{{<center-quote>}}
NeoDB is dedicated to providing a free and open interconnected space for residents of the federated universe to collect and review books, movies, music, and games.

– NeoDB
{{</center-quote>}}

## What is NeoDB

{{<admonition info "NeoDB" true>}}
NeoDB is a decentralized, open-source community platform that supports a variety of collections including books, movies, series, podcasts, games, and performances.

Compared to DouBan in China, NeoDB:
- Has no censorship system and no ads.
- Offers a more comprehensive collection of foreign books and niche content.
- Data can be easily [imported from DouBan](https://about.neodb.social/doc/howto/) and also exported.

{{</admonition >}}

- Here's my NeoDB homepage：
{{<link "https://neodb.social/users/will101" "NeoDB" "My NeoDB homepage" true>}}

## Fetching Data from NeoDB via API

NeoDB offers an open API to access users' book and movie records.
{{<admonition info "Usage of API" true>}}

- URL: `https://neodb.social/api/me/shelf`
- Bearer Token required.
- Specify Path Variable `type` as one of `wishlist`, `progress`, `complete`
- (Optional) Specify Query Param `category` as one of `book`, `movie`, `tv`, `podcast`, `music`, `game`, `performance`
- (Optional) Specify Query Param `page` for pagination

For example: `https://neodb.social/api/me/shelf/complete?category=book&page=1`
{{</admonition >}}

To get the Access Token：[NeoDB Developer#How to authorize](https://neodb.social/developer/), or generate a Test Access Token for testing.

For convenience and to encapsulate TOKEN information, the API retrieval can be deployed as a Cloudflare Worker.
- Create a new Worker in Cloudflare Workers
- [Set the environment variable](https://developers.cloudflare.com/workers/configuration/environment-variables/) NEODB_TOKEN with your Access Token
- worker.js script:

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


## Integrating Data into Hugo with Hugo Shortcode

Create `neodb.html` in `/layouts/shortcodes`, replacing with your Cloudflare Worker URL:

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

Then use the shortcode in your page:

```markdown
(Remove `\` below)

## Books Want to Read

\{\{< neodb book wishlist>\}\}

## Books Have read

\{\{< neodb book complete>\}\}
```

## Sample Effect

![NeoDB Sample](https://r2.hcplantern.top/2024/01/14/1705232044.png)
