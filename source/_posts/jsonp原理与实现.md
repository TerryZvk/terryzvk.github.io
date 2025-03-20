---
title: jsonp原理与实现
date: 2022-02-23 23:56:41
tags: ['JSONP']
---

jsonp是一种跨域通信的手段，它的原理其实很简单：

首先是利用script标签的src属性来实现跨域。

通过将前端方法作为参数传递到服务器端，然后由服务器端注入参数之后再返回，实现服务器端向客户端通信。

由于使用script标签的src属性，因此只支持get方法

<!-- more -->

```
function jsonp(req){
    var script = document.createElement('script');
    var url = req.url + '?callback=' + req.callback.name;
    script.src = url;
    document.getElementsByTagName('head')[0].appendChild(script); 
}

function hello(res){
    alert('hello ' + res.data);
}


jsonp({
    url : '',
    callback : hello 
});
```

服务器端代码:

```
var http = require('http');
var urllib = require('url');

var port = 8080;
var data = {'data':'world'};

http.createServer(function(req,res){
    var params = urllib.parse(req.url,true);
    if(params.query.callback){
        console.log(params.query.callback);
        //jsonp
        var str = params.query.callback + '(' + JSON.stringify(data) + ')';
        res.end(str);
    } else {
        res.end();
    }
    
}).listen(port,function(){
    console.log('jsonp server is on');
});
```


