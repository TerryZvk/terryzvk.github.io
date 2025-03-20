---
title: 'js 单引号替换成双引号,双引号替换成单引号'
date: 2021-03-25 10:10:31
tags: ['JS']
---

js方法传参<code>data</code>中有双引号时，会报错。比如以下代码：
```
'<a href="#" style="text-decoration: none;color: #7b7de5;margin-left: 10px" onclick="handleAcClick('+data+')">回执信息</a>'
```

可以把参数中的双引号转换为单引号：
```
JSON.stringify(data).replace(/\"/g,"'")
```
最后使用参数的时候再把单引号转换为双引号就行了
```
JSON.parse(data.replace(/'/g, '"'))
```
