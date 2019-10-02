---
title: JS物件
date: 2019-09-18 20:51:58
tags: Javascript
categories: 六角學院-JS進階
---
# JS物件


變數無法被刪除 , 屬性才可以

ex
```js
var a = 1;
b = 2; // 等同於 window.b = 2

delete a; //false
delete b; //true
console.log(a); //1 變數無法刪除
console.log(b); //not defined
```
<!-- more -->
## 傳值與傳址
Javascript 只有兩種類型型別  純值及物件

純值無法新增屬性
當一個值無法新增屬性 代表它為物件
ex
```js
var str = '小明';
str.name = '大明';
console.log(str);  // 小明

function callName(){
    console.log('呼叫')
}
console.log(typeof callName); //function
```
函式也是物件型別 , 是屬於物件型別下的一個子型別

JavaScript中參數的傳值與傳址心得
純值單純只傳遞值 , 修改後無關聯性

傳值
```js
var person = '小明';
var person2 = person;
person2 = '杰倫';
console.log(person,person2) // 小明 , 杰倫
```

傳址

ex1
```js
var person = {
  name: '小明'
}
var person2 = person;
person2.name = '杰倫';
person2.name2 = '小王';
console.log(person,person2); // 皆為 {name:'杰倫',name2:'小王'}
console.log(person === person2);  // true
```

ex2

當賦予新物件後 , 參考位址改變 , 變更內容時不影響前者
```js
var person = {
  name: '小明'
}
var person2 = person;
person2 = {
  name: '小王'
}
console.log(person,person2); //{name:'小明'} , {name:'小王''}
console.log(person === person2); // false  原因為不同址
```

ex3

物件與物件的比對是根據參考位址,而非物件值
```js
var obj = {};
var obj2 = {};
console.log(obj === obj2); // false
```

ex4
```js
var a = { x: 1}; //0x01
var b = a;
a.x = { x: 2}; //0x02
a.y = a = { y: 1}; //0x03
console.log(a);//{y:1};
console.log(b);//{x:2,y:1}
```
第四行 a.y = a = 這裡是同時執行,順序不影響

a = { y: 1} 創出新的參考位址 { y: 1} , 固 a 變更至新參考位址 0x03

a.y = { y: 1} 此時位址還在0x02  固0x02為{x:2,y:1}  , b位址還是在0x02
![](/images/byreference.jpg)

## 物件的淺層複製及深層複製

### 淺層複製
淺層複製只能複製第一層的欄位及value

第一層value若是物件型別,一樣會指向同個參考位址

ex 1
```js
var family = {
  name: '小明家',
  members:{
    father: '爸爸',
    mom: '媽媽',
    ming:'小明'
  },
}

var newFamily = {};
for (var key in family){
  newFamily[key] = family[key]
};

console.log(family === newFamily);  //false
console.log(family.members === newFamily.members); //true
```

ex 2
```js
//jQuery
var family = {
  name: '小明家',
  members:{
    father: '爸爸',
    mom: '媽媽',
    ming:'小明'
  },
}

var newFamily = jQuery.extend({},family);
console.log(family === newFamily);  //false
console.log(family.members === newFamily.members); //true
```

ex 3
```js
// ES6
var family = {
  name: '小明家',
  members:{
    father: '爸爸',
    mom: '媽媽',
    ming:'小明'
  },
}
var newFamily = Object.assign({},family);
console.log(family === newFamily);  //false
console.log(family.members === newFamily.members); //true
```

### 深層複製
將物件轉為字串再轉為物件 , 建立新參考位址

透過 JSON.parse() 出來的資料是一個物件，所以物件會有傳參考特性

ex1
```js
//深層複製
var family = {
  name: '小明家',
  members:{
    father: '爸爸',
    mom: '媽媽',
    ming:'小明'
  },
}

var newFamily = JSON.parse(JSON.stringify(family));
newFamily.name = '杰倫';
newFamily.members.ming = 'Jay';
console.log(family === newFamily);  //false
console.log(family.members === newFamily.members); //false
```
ex2
```js
var family = JSON.parse("{\"name\":\"小明家\",\"members\":{\"father\":\"爸爸\",\"mom\":\"媽媽\",\"ming\":\"小明\"}}");
var newFamily = family;
newFamily.members.father = "父親";

console.log(family.members.father); // 父親
console.log(newFamily.members.father); // 父親
```

## JSON
JSON內的字串一定是雙引號
JSON 的格式非常嚴格，多一個逗號少一個逗號，都會導致出現錯誤

### 讀取本地JSON
需用WebServer方式開啟

![](/images/load-JSON.jpg)
