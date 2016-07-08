### singleton模式

#### 基本概念
单体是只能被实例化一次并且可以通过一个重所周知的访问点访问的类，我们可以用单体来划分命名空间并将相关的方法和属性组织在一起

#### 对象字面量单体
一个简单的例子：
```js
var myNamespace = {
    someDataA: [],
    _somePrivateData: [],
    publicFun: function(item){
                basket.push(item)
             }
}
```
上面是一个最简单的单体，我们可以看到myNamespace可以划分命名空间，我们也将一些属性和方法归纳在一个对象中，我们甚至可以约定_下划线开头的属性和方法为私有（这只是个约定），我们也可以通过闭包实现私有方法

#### 带闭包的singleton
通过闭包实现真正的私有，其实这也就是设计模式二中的module模式：
```
myNamespace.singleton = (function () {
                                var privateData = [];
                                return {
                                    publicFunction: function(item){
                                        basket.push(item)
                                    },
                                };
                            })()
```
#### laying loading
我们可以通过惰性加载延迟构建实例。我们可以看一下静态实例和惰性加载实例调用方式：

静态实例中: `myNamespace.singleton.publicFunction()`

惰性加载: `myNamespace.singleton.getInstance().publicFunction()`

以下是惰性加载实例➡
```js
myNamespace.singleton = (function () {
    var instance;
    function init() {
        var privateData = [];
        return {
            publicFunction: function(item){
                basket.push(item)
            },
        };
    }
    return {
        getInstance: function () {
            if(!instance){
                instance = init();
            }
            return instance;
        }
    };
})()
```
惰性加载实现当我们需要时我们才将方法和属性加载出来,这种按需加载的实现使其适用于开销大的单体

当然惰性加载的缺点显而易见，它较复杂、代码不直观、不易理解

#### 分支
分支可用于浏览器兼容性问题，我们可以在实例化时确定浏览器适合的一套方法，举个简单的例子：

在用js获取dom样式时往往会有ie的 `currentStyle` 方法和其他大部分支持的 `getComputedStyle()` 这时候我们就可以通过分支来确定一个单体

```js
myNameSpace.getStyle = (function(
    var funcA = function(ele,attr){
        var computedStyle = document.defaultView.getComputedStyle(ele,null)
        return computedStyle[attr];
    }
    var funcB = function(ele,attr){
        var computedStyle = ele.currentStyle;
        return computedStyle[attr];
    }
    return document.defaultView.getComputedStyle ? funcA : funcB ;
))()
```
当然分支并不一定是高效的选择，在此只是举例可以这样做

[原文链接：http://blog.mdzzapp.com](http://blog.jishuzhai.site/#/article/初识javascript设计模式三?_k=37a01s)
