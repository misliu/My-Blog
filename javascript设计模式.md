## 初识javascript设计模式一

### 1、Constructor模式

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


