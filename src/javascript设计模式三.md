## 初识javascript设计模式三
### singleton模式

#### 基本概念
singleton模式将类的实例化次数限制为一次，也就是说，如果实例不存在，就创建一个对象实例，否则返回实例化对象的引用，例如：

```js
var singleton = (function () {
    var instance;
    function init() {
        var basket = [];
        return {
            addItem: function(item){
                basket.push(item)
            },
            getItemCount: function(){
                return basket.length
            }
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
var single1 = singleton.getInstance();
var single2 = singleton.getInstance();
single1.addItem({item:1});
console.log(single1.getItemCount() == single2.getItemCount())//true
```
通过上面的例子能够得出， `single1 === single2`结果为true

#### 延迟执行
直接使用被初始化的静态对象也能达到同样的效果，singletion与它的区别就在与，singleton可以实现延迟构建