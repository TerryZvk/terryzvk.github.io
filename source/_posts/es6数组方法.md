---
title: es6数组方法
date: 2022-02-17 16:47:53
tags:
---

es5数组方法用的比较多，比较熟了，es6比较少，这里总结一下。
<!-- more -->
### Array.from()
用于将类数组和可遍历的对象转换为真正的数组
类数组：就是类似数组的对象，本质上要有length属性，说到底，任何拥有length属性的对象，都可以通过Array.from()方法转换为数组。
可遍历的对象：ES6新增的数据结构set和map

```
let myArray = {'0':'a','1':'b','2':'c'};
let arr = Array.from(myArray);
console.log(arr);   //[]由于没有length属性打印结果为一个空数组

let myArray = {'0':'a','1':'b','2':'c',length:3};
let arr = Array.from(myArray);
console.log(arr);  //["a", "b", "c"]

console.log(Array.from({length:2}));  //[undefined, undefined]
```

### fill()
填充一个数组，一般用于数组的初始化

```
var arr = ['a','b','c'];
console.log(arr.fill(1)); // [1, 1, 1]
```

### keys()
keys()遍历出所有的索引值

```
for (let index of ['a', 'b'].keys()) {
    console.log(index);
}  //0  1

```
### valueOf()
valueOf()遍历所有的值
```
for (let index of ['a', 'b'].valueOf()) {
    console.log(index);
}  //a  b
```
### entries()
entries()遍历值和索引
```
for (let index of ['a', 'b'].entries()) {
    console.log(index);
}  //[0, "a"] [1, "b"]
```
