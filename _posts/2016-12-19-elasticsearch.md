---
layout:post
title:elasticsearch 在不是 not_analyzed 的前提下如何全匹配的效果
---

使用[wildcard](https://www.elastic.co/guide/en/elasticsearch/reference/2.3/query-dsl-wildcard-query.html)查询

wildcard可以使用通配符:?用来匹配任意字符，*用来匹配零个或者多个字符.
但是假如不使用通配符时就相当于全匹配.

匹配符匹配:

```
{    "query": {        "wildcard": {            "postcode": "W?F*HW"         }    }}
```

全匹配:

```
{    "query": {        "wildcard": {            "postcode": "WFHW"         }    }}
```

或者多字段匹配(全字段):

```
{    "query": {        "wildcard": {            "_all": "WFHW"         }    }}
```

