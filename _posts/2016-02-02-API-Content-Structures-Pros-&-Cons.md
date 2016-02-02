---
layout: post
title: API Content Structures Pros & Cons
date: 2016-02-02 22:38:00.000000000 +01:00
tags: API
---

From today on I will write down notes of what I've learned from the book
"Build APIS You Won't Hate".

Today the topic is about the response content structure.

## JSON-API

The famous [JSON-API](http://jsonapi.org/format) suggests to use `plural key` for
both single resources and resource collections.

{% highlight json %}
{
  "users": [
    {
      "id": "1",
      "name": "bearzk"
    }
  ]
}

{
  "users": [
    {
      "id": "1",
      "name": "bearzk"
    },
    {
      "id": "2",
      "name": "tom"
    }
  ]
}
{% endhighlight %}

### Pros

- consistent response, always very same structure

### Cons

- could be confusing to human when first time see it

## Twitter Style

Twitter uses another strategy instead, it will give you single thing or collection
of things when you asked differently.

{% highlight json %}
{
  "id": "1",
  "name": "bearzk"
}

[
  {
    "id": "1",
    "name": "bearzk"
  },
  {
    "id": "2",
    "name": "tom"
  }
]
{% endhighlight %}

### Pros

- minimalistic response

### Cons

- no possibility for pagination or other meta info

## Facebook Style

Ask for one, get one.

{% highlight json %}
{
  "id": "1",
  "name": "bearzk"
}
{% endhighlight %}

Ask for more, get more, namespaced.

{% highlight json %}
{
  "data": [
    {
      "id": "1",
      "name": "bearzk"
    },
    {
      "id": "2",
      "name": "tom"
    }
  ]
}
{% endhighlight %}

### Pros

- Space for pagination and other meta info
- Simplistic response with extra namespace

### Cons

- Single item has no other way than embedding to have meta info

## BAYWH Style

always with name space, also when other object wrapped within.

{% highlight json %}
{
  "data": {
      "id": "1",
      "name": "bearzk"
  }
}

{
  "data": [
    {
      "id": "1",
      "name": "bearzk"
    },
    {
      "id": "2",
      "name": "tom"
    }
  ]
}

{
  "data": {
    "id": "1",
    "name": "bearzk",
    "comments": {
      "data": [
        {
          "id": "1234",
          "text": "Awesome API!"
        },
        {
          "id": "4321",
          "text": "Yeah!"
        },
        page: "1"
      ]
    }
  }
}
{% endhighlight %}

### Pros

- generic root scope
- easily wrapping other meta info into `data` scope

### Cons

- any? :)
