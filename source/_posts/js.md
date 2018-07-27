---
title: 《js高级程序设计》笔记
date: 2018-03-01 21:46:12
tags:
categories: 技术
---
 >

# Function

## arguments.callee
使用场合：递归算法

解决问题：消除代码的紧密耦合

问题：严格模式下报错

```javascript
function factorial(num){
 if (num <=1) {
 return 1;
 } else {
 return num * factorial(num-1)
 }
}
//等同于
function factorial(num){
 if (num <=1) {
 return 1;
 } else {
 return num * arguments.callee(num-1)
 }
} 
```
## caller
这个属性中保存着调用当前函数的函数的引用

问题：严格模式下报错

原因：加强这门语言的安全性，这样第三方代码就不能在相同的环境里窥视其他代码了
```javascript
function outer(){
 inner();
}
function inner(){
 alert(inner.caller);
}
outer();//outer函数源码
//等同于
function outer(){
 inner();
}
function inner(){
 alert(arguments.callee.caller);
}
outer(); 
```
# Object
## 访问器属性
使用场合：即设置一个属性的值会导致其他属性发生变化
```javascript
var book = {
 _year: 2004,
 edition: 1
};
Object.defineProperty(book, "year", {
 get: function(){
 return this._year;
 },
 set: function(newValue){
 if (newValue > 2004) {
 this._year = newValue;
 this.edition += newValue - 2004;
 }
 }
});
book.year = 2005;
alert(book.edition); //2 

//等同于
var book = {
 _year: 2004,
 edition: 1
};
//定义访问器的旧有方法
book.__defineGetter__("year", function(){
 return this._year;
});
book.__defineSetter__("year", function(newValue){
 if (newValue > 2004) {
 this._year = newValue;
 this.edition += newValue - 2004;
 }
});
book.year = 2005;
alert(book.edition); //2 
```
