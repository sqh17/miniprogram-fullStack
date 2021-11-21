# 跟着李艺老师学习《微信小程序全栈开发实战》笔记

### wxs和小程序之间的关系
wxs是微信推出来的语言，因为微信小程序双线程，一线程作用与视图线程view，一线程作用与逻辑线程app service。之间的联系通过setData进行驱动，中间通过WeiXinJsBridge与微信底层的native（微信平台能力，本地存储，网络，其他硬件能力比如调用手机的电话震动）进行通讯，效率低下，由于setData在频繁更新和大数据更新的有瓶经，影响渲染效率，所以有了wxs语言虽然部分和js相似，他直接操作视图数据，避免了通讯开销。

### 小程序的启动方式
- 冷启动  
  加入用户已经打开过某个小程序，然后在一定时间内再次打开，此时它不需要重新启动，只是将后台状态的小程序切换到前台，
- 热启动  
  用户首次打开小程序，或者是小程序被微信主动销毁之后再次打开，这时候小程序需要重新加载并启动

### 检测新版本
小程序冷启动时如果发现新版本，将会异步下载新版本的代码包，并且同时用客户端本地的代码包进行启动，既新版本的小程序需要等下一次冷启动才用得上
### 小程序主动销毁的情况
1. 小程序进入后台，在后台维持运行；超过一定的时间，大概是5分钟；就会被微信主动销毁
2. 短时间内，目前为5秒钟，连续2次以上收到系统内存警告；微信就会对小程序进行主动销毁。将会收到提示：运行内存不足，请重新打开小程序。我们可以用 wx.onMemoryWarning 监听内存的警告事件，提前处理

### 生命周期
* onload
* onShow
* onReady
* onHide
* onUnload

### rich-text
类似于jsx的方式，只不过以标签形式存在
https://developers.weixin.qq.com/miniprogram/dev/component/rich-text.html
```javascript
nodes: [{
  name: 'div',
  attrs: {
    class: 'div_class',
    style: 'line-height: 60px; color: #1AAD19;'
  },
  children: [{
    name: 'img',
    src:'xxx'
  },{
    name:'p',
    attr:{
      class:'p',
    }
  }]
}]
```

### hover-class，hover-stop-propagation，hover-stay-time，hover-start-time
* hover-class:点击时的样式类名
* hover-stop-propagation 阻止冒泡，点击之作用与当前元素，不会因父级元素影响
* hover-stay-time 点击时状态持续多久
* hover-start-time 多久开始点击状态

### 生成海报大致方式
1. wx.createCanvasContent 生成画布
2. 画布上绘制
3. wx.canvasToTempFilePath 保存到本地，并生成一个临时图片路径
4. wx.saveImageToPhotosAlbum 保存临时文件到本地