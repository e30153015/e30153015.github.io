---
title: JS-function及this
date: 2019-10-01 19:05:49
tags: Javascript
categories: 六角學院-JS進階
---
# JS-function及this

```js
function callName(a){
  console.log(a); //ming
  var a;
  console.log(a); //ming
  a = 'yao';
  console.log(a); //yao
}

callName('ming');
```
<!-- more -->
物件即使透過參數做傳遞,也會保有傳參特性

建議不要調整物件屬性參數 ,否則可能會使原有屬性受到調整
```js
function callObject(obj){
  obj.name="Jay";
};
var family = {
  name: 'ming'
};

callObject(family);
console.log(family.name); //Jay
```

## arguments
function內 arguments為類陣列,無論傳入多少參數都能接收

ex
```js
function test(){
    console.log(arguments);
}
test(1,2,3,4,5); // [1,2,3,4,5]
```

## 閉包
```js
function storeMoney(){
  var money = 1000;
  return function(price){
    money = money + price;
    return money;
  }
}

//此時MingMoney有屬於自己的函式及變數 >money 1000
var MingMoney = storeMoney();
//此時JayMoney有屬於自己的函式及變數 >money 1000
var JayMoney = storeMoney();  
console.log(MingMoney(1000)); // 2000
console.log(MingMoney(100)); // 2100

console.log(JayMoney(300)); // 1300
console.log(JayMoney(300)); // 1600
```
### 函式工廠、私有方法
```js
function storeMoney(initValue){
  var money = initValue || 1000  //若無初始值則為1000
  return {
    increase: function(price){
      money += price;
    },
    decrease: function(price){
      money -= price;
    },
    value: function(){
      return money;
    }
  }
}
var MingMoney = storeMoney(100);
MingMoney.increase(300);
MingMoney.decrease(189);
console.log(MingMoney.value()); // 211
var JayMoney = storeMoney(1700);
JayMoney.increase(300);
JayMoney.decrease(189);
console.log(JayMoney.value()); //1811
```

### 限制作用域
```js
//無限制作用域 , i會直接加至3後回傳
function arrFunction(){
  var arr = [];
  for (var i = 0;i<3;i++){
      arr.push(function(){
        console.log(i)
      })
  }
  return arr;
}

var fn = arrFunction();
fn[0](); //
fn[1]();
fn[2]();
```

```js
// 立即函式限制作用域
function arrFunction(){
  var arr = [];
  for (var i = 0;i<3;i++){
    (function(j){
      arr.push(function(){
        console.log(j)
      })
    })(i);
  }
  return arr;
}

var fn = arrFunction();
fn[0](); // 0
fn[1](); // 1
fn[2](); // 2
```

```js
//ES6
function arrFunction(){
  var arr = [];
  for (let i = 0;i<3;i++){
      arr.push(function(){
        console.log(i)
      })
  }
  return arr;
}

var fn = arrFunction();
fn[0](); // 0
fn[1](); // 1 
fn[2](); // 2
```

## this指向
### this基本觀念
*  每個執行環境都有屬於自己的this關鍵字
*  this與函式如何宣告`沒有關連性`,僅與呼叫方法有關
*  `嚴格模式下,簡易呼叫會有很大的改變

### 影響函式this的調用方式
* 作為物件方法(最常運用this的方法)
* 簡易呼叫(絕大多數的呼叫方式(絕大多數的呼叫方式)
* bind, apply, call 方法
* new
* DOM 事件處理器
* 箭頭函式 (ES6)

** 物件的方法調用時 , 僅需關注`是在哪一個物件`下呼叫
#### this練習
ex1
```js
var myName = "global";

function callName(){
    console.log(this.myName);
}

var family = {
  myName: 'MingHome',
  callName: callName,
  Ming:{
    myName:'YaoMing',
    callName: callName
  }
}

family.callName(); //MingHome
family.Ming.callName(); //YaoMing

var newCallName = family.callName;
newCallName(); //global
```
** 簡易呼叫 (Simply Call)

盡可能不要使用Simple Call的`this`

ex2
```js
// IIFE
var myName = 'global';
(function(){
  console.log(this.myName); // global
  function callSomeone(){
    console.log(this.myName); // global
  }
  callSomeone();
})();
```

ex3
```js
var myName = "global";

var family = {
  myName: 'MingHome',
  callName:function(){
    var vm = this;
    console.log(this.myName);  // MingHome
    //setTimeout 算是全域下的Simply Call, 所以this會指向全域   
    setTimeout(function(){
      console.log(this.myName); // global
      console.log(vm.myName); // MingHome
    },1000)
  }
}

family.callName();
```
ex4

`call`與`bind`的區別為 `call`會直接呼叫
``` js
function fn(para1,para2){
  console.log(this,para1,para2)
};
var family = {
  myName:'小明家'
};

fn(1,2); // window, 1, 2
fn.call(family,1,2); //family, 1, 2
fn.apply(family,[3,4]); //family, 3, 4

var fn2 = fn.bind(family,'Ming','Jay');
fn2(1);//family, Ming, Jay

var fn3 = fn.bind(family,'Ming');
fn3(1);//family, Ming, 1
```

### 嚴格模式
* 加入 'use strict'即可運作
* 並不會影響不支援嚴格模式的瀏覽器
* 可依據執行環境設定 'use strict'
* 透過拋出錯誤的方式消除一些安靜的錯誤(防止小錯誤)
* 禁止使用一些有可能被未來版本ECMAScript定義的語法

ex

`簡易呼叫(Simply Call)`的 `this` 盡可能不要調用, 它的本質其實是 `undefined`;
```js
function callPara(para1, para2){
  console.log(this,typeof this,para1,para2)
}
callPara.call(1,'Ming','Jay');          //Number{1}物件型別,object,"Ming","Jay"
callPara.call(undefined,'Ming','Jay');  //window,object,"Ming","Jay"
callPara('Ming','Jay');                 //window,object,"Ming","Jay"
function callStrict(para1, para2){
  'use strict';
  console.log(this,typeof this,para1,para2)
}
callStrict.call(1,'Ming','Jay');          // 1 ,number,"Ming","Jay"
callStrict.call(undefined,'Ming','Jay');  //undefined,"undefined","Ming","Jay"
callStrict('Ming','Jay');                 //undefined,"undefined","Ming","Jay"

// 簡易呼叫(Simply Call)的 this 盡可能不要調用, 它的本質其實是 undefined;
```

### this:DOM
ex 1
```html
<button onclick="console.log(this)"></button>
<!-- this會直接將整個標籤呈現上來 -->
<button onclick="console.dir(this)"></button>
<!-- 會是個單純的物件,能知道這個物件有哪些屬性可用 -->  
```

ex 2

為點擊的li增加背景顏色
```html
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
  </ul>
<script>  
  var fn = function(){
    console.log('test')
    console.dir(this)
    this.style.backgroundColor = 'orange';
  }
  //將所有li加上event
  var els = document.querySelectorAll('li');
  for(var i = 0;i<els.length;i++){
    els[i].addEventListener('click',fn)
  }
</script>
```

### this練習
ex1
```js
var name = "全域";
var area = {
  name: '區域',
  callName: function(){
    console.log(this.name);
  }
};
(function(){
  var a = area.callName;
  area.callName();  //區域
  a();  //全域
})()

var a = area.callName;
area.callName();  //區域
a();   //全域
```

ex 2
```js
var name = '全域';
(function(){
  var name = '區域';
  console.log(this.name); //全域
  setTimeout(function(){
    console.log(this.name); //全域
  })
})();
```

ex 3
```js
var arr = ['1','2','3'].map(parseInt);
console.log(arr);  // [1, NaN, NaN]
//.map為callback function拆解為
arr = ['1','2','3'].map(function(item,i){
  return parseInt(item,i); //i為進制
})
console.log(arr)
```

ex 4

`陳述式`與`表達式`
```js
function a(){
  console.log('hexSchool a');
}
function b(){
  return 'hexSchool b';
}
var c = function(){
  console.log('hexSchool c');
}

var d;

// a-陳述式, b-陳述式, c-表達式, d-陳述式
```