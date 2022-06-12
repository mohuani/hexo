---
title: "is()和hasClass()"
date: 2019-09-03 19:36:38
draft: true
tags: [前端]
categories:
- [前端]
---

#### is()和hasClass()
- jq中的is()有判断元素是否含有某个class的属性，但是hasClass()没有效果。下面是我找的对应的官方的文档，注意仔细阅读文档里面的小demo，并且注意里面的细微区别。

- is()
```bash
https://api.jquery.com/is/

Description: Check the current matched set of elements against a selector, element, or jQuery object and return true if at least one of these elements matches the given arguments.
```
- hasClass()
```bash
https://api.jquery.com/hasClass/#hasClass-className

Description: Determine whether any of the matched elements are assigned the given class.
```