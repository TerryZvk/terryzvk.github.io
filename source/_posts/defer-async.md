---
title: defer async
date: 2022-02-19 18:20:14
tags:
---

我们都知道，普通的<code>script</code>标签加载 <code>js</code>会阻塞页面解析，因此优化措施可以将script放到<code></body></code>标签前面。但还有一种更好的做法是加<code>defer </code>和 <code>async</code>属性。这两个属性使得<code>script</code>都不会阻塞<code>DOM</code>的渲染
<!-- more -->
### defer

如果script标签设置了该属性，则浏览器会异步的下载该文件并且不会影响到后续DOM的渲染；
如果有多个设置了defer的script标签存在，则会按照顺序执行所有的script；
defer脚本会在文档渲染完毕后，DOMContentLoaded事件调用前执行。

应用场景: 如果你的脚本代码依赖于页面中的DOM元素（文档是否解析完毕），或者被其他脚本文件依赖。
例：
1.评论框
2.代码语法高亮
3.polyfill.js

### async

async脚本会在加载完毕后执行。
sync的执行，并不会按着script在页面中的顺序来执行，而是谁先加载完谁执行。
async脚本的加载不计入DOMContentLoaded事件统计

应用场景: 如果你的脚本并不关心页面中的DOM元素（文档是否解析完毕），并且也不会产生其他脚本需要的数据。

例：
1.百度统计