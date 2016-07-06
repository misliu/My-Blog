### 1、module模式

在javascript中，module模式为对象提供了公有和私有的方法和变量

#### a、一个简单的例子
可以先假设我们需要实现一个购物车，规定有一个’私有‘变量basket，以及将物品添加到购物车的方法addItem、改变html中id为count元素text的方法setText、和总计购物车中数目方法getItemCount三个公有方法，我们可以这样实现

```js
var basketModule = (function(_jQ){
    var basket = [];
    return {
        addItem: function(item){
            basket.push(item);
        },
        getItemCount: function(){
            return basket.length;
        }，
        setText: function(){
            _jQ('#count').text(basket.length);
        }
    }
})(jQuery)
```
#### b.通过闭包实现module
上面的例子中basket变量实际上完全与全局作用域隔离了，它只存在于闭包内，唯一能访问到它的只有addItem、getItemCount、setText三个函数

#### c.全局导入
上面的例子通过参数将全局变量jQuery导入到module中，这会使得变量的声明更清晰，当然我们也能通过这种方式拓展对象

#### d.全局导出
上面的例子中我们return值导出全局变量

这就是Module模式的基本内容，想了解更多可以看看Ben Cherry的文章，[戳这里](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html)

#### 2、Revealing Module模式

Revealing Module模式将公有指针指向私有函数上，在这里简单的贴一下Revealing Module模式的写法
```js
var basketModule = (function(){
    var basket = [];   
    function privateAddItem(item){
            basket.push(item);
    }
    function publicAddItem(){
            privateAddItem();
    }
    return {
        addItem: publicAddItem,
    }
})()
```
这种模式能改善可读性，但是这种指针的形势只适用于函数