---
layout: post
title: "JavaScript Debug"
description: ""
category: javascript
tags: [javascript]
---
{% include JB/setup %}

## Debug Focus

<https://developers.google.cn/web/tools/chrome-devtools/accessibility/focus>

Add `document.activeElement` to Live Expression.

Or

```js
var hostTextarea = document.querySelector("#Input189 textarea");

hostTextarea.onclick = function(e) { console.log("%s: %s", this.tagName, e.type); };

hostTextarea.onfocus = function(e) { console.log("%s: %s", this.tagName, e.type); };
```
