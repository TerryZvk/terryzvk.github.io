---
title: 箭头函数this
date: 2022-02-17 16:00:54
tags: ['JS', '箭头函数']
---

箭头函数的this在定义时就确定，就是箭头函数所在作用域的this, 且不会被call, apply, bind改变。
<!-- more -->
```
let foo = () => {
  console.log(this.id)
}

let id = 21

foo.call({ id: 42 }) //  21
```

如果箭头函数改成function，则会被改变，因为function的this只有调用时能确定

```
let foo = function(){
  console.log(this.id)
}

let id = 21

foo.call({ id: 42 }) // 42
```

