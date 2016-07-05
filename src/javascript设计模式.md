## 初识javascript设计模式一

### Constructor模式

#### a、对象赋值方法

1. 使用点语法
2. 中括号语法
3. Object.defineProperty／Object.defineProperties

1、2方法较为常用，主要展示一下第三种方法：

```js
var obj = {};
var config = {
    value:'123',
    enumberable:true,
    writable:true,
    configurable:true
    //get:...
    //set:...
}
Object.defineProperty(obj,'key',config);
```
config中参数介绍：

* value 设置属性的值
* enumberable 设置能否通过for－in循环返回属性
* writable 设置能否修改属性的值
* configurable 设置能否通过delete删除属性从而重现定义
* get 读取属性时调用的函数
* set 写入属性时调用的函数

#### b、Constructor
es5中不支持类的概念，但是它却有new关键词，我们可以通过构造函数创建特定的对象，像Object和Array都是原生的构造函数，下面看一个例子：

```js
function Person(name,age){
    this.name = name;
    this.age = age;
    this.sayName = function(){
        console.log(this.name);
    }
}
var p1 = new Person('mdzz','18');
console.log(p1.constructor === Person);//true
```
#### c、prototype属性

以上的写法使每个对象都有一个sayName方法，创建两个sayName方法显然是没有必要的

当然我们可能会想到这样写：

```js
function Person(name,age){
    this.name = name;
    this.age = age;
    this.sayName = sayName;
}
function sayName(){
        console.log(this.name);
}
```
当然这实现了全局定义一个sayName，但是这样又使它失去了封装性

这时候原型属性就派上用场，我们创建的每个函数都有一个prototype，这是一个指针，我们可以通过它共享属性和方法

```js
function Person(name,age){
    this.name = name;
    this.age = age;
}
Person.prototype.sayName = function(){
        console.log(this.name);
}
```
这样就实现了每个对象实例都共享一个sayName方法。
需要注意的是，每当代码读取对象的某个属性的时候都会执行一次搜索，先从实例本身，如果找到了就返回，如果没有就在原型中找，直到找到Object如果没有就返回undefined，下面看一个例子：

```js
function Person(name){
    this.name = name;
}
Person.prototype.sayName = function(){
        console.log(this.name);
}
Person.prototype.age = 18;
var p1 = new Person('mdzz1');
var p2 = new Person('mdzz2');
p1.age = 19;
console.log(p1.age)//19
console.log(p2.age)//18
```
不难看出为p1添加一个实例属性时，这个属性会屏蔽原型中的同名属性，也可以通过 `hasOwnProperty` 来确定是访问的原型属性还是实例属性

```js
console.log(p1.hasOwnProperty('name')) //---true
console.log(p2.hasOwnProperty('name')) //---true
console.log(p1.hasOwnProperty('age')) //---true
console.log(p2.hasOwnProperty('age')) //---false
```
以上参考了《javascript设计模式》、《javascript高级程序设计》如果有写的不对的请留言指出，希望能用这种和大家分享的方法能学到更多^_^

[依旧附上我的原文链接](http://blog.jishuzhai.site/#/article/初识javascript设计模式一?_k=zxgtrj)
