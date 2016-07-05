### react首屏优化
### 在这里简单的总结一下我所知道的首屏优化的方案：

1. 通过webpack的UglifyJsPlugin插件对代码进行压缩
2. 提取第三方库
2. 通过webpack实现按需加载
3. 通过服务器对代码进行gzip压缩
4. 服务器端渲染首屏

### 下面将具体介绍一下这几种方案的使用：
#### 通过webpack的UglifyJsPlugin插件对代码进行压缩
通过简单的UglifyJsPlugin配置可以将代码压缩至原来的一半大小

```js
new webpack.optimize.UglifyJsPlugin({
            compress: {
                warnings: false
            }
        })
```
#### 提取第三方库
像 react , redux之类的库和我们的源代码放在一起打包，体积肯定会很大。所以可以在 webpack 中设置：

```js
entry:{
        vendor:['react','redux']
    },
plugins: [
        new webpack.optimize.CommonsChunkPlugin('vendor',  'vendor.js')
  ]
```
这样我们只需要在html中引入vendor.js就行了

#### 通过webpack实现按需加载
通过与react－router＋webpack配合我们可以实现按需加载（[react router文档传送门](http://react-guide.github.io/react-router-cn/docs/guides/advanced/DynamicRouting.html)），上一段代码：

```js
<Route path='/message' getComponent={
    (nextState ,cb) => {
        require.ensure([] , (require) => {
            cb(null,require('./routes/message/index.jsx'));
        },'message');
    }
} />
```
以上是博主留言区的路由，再看webpack：

```js
output:{
        path: __dirname+'/bundle/',
        publicPath:'/bundle/',
        filename: '[name].bundle.js',
        chunkFilename: '[name].chunk.js'
    }
```
我们只需要在output中配置好文件名就行，注意：publicPath是设定以http请求的方式请求静态资源的路径

通过分割我们就不必要一次将整站加载下来，大大的减少了首屏加载时间，当然个人觉得如果页面不多或者文件本身不大没必要使用按需加载，gzip或许更适合

博主的博客实现了按需加载－。－，可以打开控制台在Network里查看。
#### 通过服务器对代码进行gzip压缩
经过webpack的UglifyJsPlugin插件后，博主的博客还有1.15M对于我来说也是个不小的数目，这时候不得不感叹，gzip真心强大，经过gizp压缩，博客变成了300k左右，在这里博主就不贴博主的渣渣nginx配置了，大家可自行谷歌百度

### 个人总结
如果应用不大，使用UglifyJsPlugin＋gzip就能满足首屏的要求，没有必要折腾按需加载，按需加载适用于较大型的页面多的应用，强行按需加载（比如说博主）只会增加请求量，gzip压缩效果也不会那么明显，只能说谨慎使用按需加载吧

[点我看demo源码，如果对你有点帮助，希望能随手star](https://github.com/redsx)

