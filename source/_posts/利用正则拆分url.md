---
title: 利用正则拆分url
date: 2019-05-08 11:11:46
tags: 正则表达式
---

```javascript
function parseUrl(url) {
  var result = {};
  var keys = [
    "href",
    "origin",
    "protocol",
    "host",
    "hostname",
    "port",
    "pathname",
    "search",
    "hash"
  ];
  var i, len;
  var regexp = /(([^:]+:)\/\/(([^:\/\?#]+)(:\d+)?))(\/[^?#]*)?(\?[^#]*)?(#.*)?/;
  var match = regexp.exec(url);
  if (match) {
    for (i = keys.length - 1; i >= 0; --i) {
      result[keys[i]] = match[i] ? match[i] : "";
    }
  }
}
```
