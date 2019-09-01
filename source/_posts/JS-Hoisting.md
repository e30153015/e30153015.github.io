---
title: JS-Hoisting
date: 2019-08-29 10:44:45
tags: Javascript
categories: 六角學院-JS進階
---
# Hoisting

將JS拆成兩個階段
* 創造階段
* 執行階段

`函式陳述式`在創造階段時就會優先載入
<!-- more -->
ex1
``` js
var ming = '小明';
// 拆解
// 創造階段
var ming;
// 執行階段
ming = '小明'
```
ex2
``` js
callName();
function callName(){
  console.log('呼叫小明')   // 小明
}
// 函式陳述式 在創造階段時就會優先載入
// 拆解
// 創造階段
function callName(){ 
   console.log('呼叫小明')
 }
```
ex3
``` js
var callName = function(){
  console.log('呼叫小明2')
}
function callName(){
  console.log('呼叫小明1')
}
callName(); // 呼叫小明2

// 拆解
// 創造階段
function callName(){
  console.log('呼叫小明2')
}
var callName;
// 執行階段
callName = function(){
  console.log('呼叫小明2')
}
callName();
```
ex4
``` js
callName();   // undefined
function callName(){
  console.log(Ming); 
}
var Ming = '小明';

// 拆解
// 創造階段
function callName(){
  console.log(Ming);  
}
var Ming;
// 執行階段
callName();  // function先執行 , 固Ming為undefined
Ming = '小明';
```

ex6
``` js
function callName(){
  console.log('小明');
}
callName();  // 第一次執行  杰倫
function callName(){
  console.log('杰倫');
}
callName();  // 第二次執行  杰倫

// 拆解
// 創造階段
function callName(){
  console.log('小明');
}
function callName(){
  console.log('杰倫');
}
// 執行階段
callName();  
callName();  
```

```js
// 練習題
whosName();
function whosName(){
  if (name) {
    name = '杰倫'
  }
}
var name = '小明';
console.log(name);   // 小明

// 拆解
// 創造階段
function whosName(){
  ...
}
var name;
// 執行階段
whosName();
 name = '小明';
 // console.log('小明')
```

testetetsete