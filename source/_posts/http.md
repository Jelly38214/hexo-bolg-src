---
title: http
date: 2020-06-19 11:14:04
categories:
  - protocol
tags:
  - http, https
cover: "/img/designpattern/factory-method.png"
---

# Response

Not all fields in response header can be got by frontend. Only the fields in `CORS-safelisted` response headers.

##### [Default CORS-safelisted](https://developer.mozilla.org/zh-CN/docs/Glossary/Simple_response_header)

- Cache-Control
- Content-Language
- Content-Type
- Expires
- Last-Modified
- Pragma

Expose more response header for Frontend by using `Access-Control-Expose-Headers`, look at example below.

```nginx
  Access-Control-Expose-Headers: Content-Disposition, Content-Length
```
