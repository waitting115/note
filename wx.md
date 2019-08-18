我们都知道bindtap和catchtap都是当用户**点击**该组件的时候会在该页面对应的Page中找到相应的事件处理函数。但是bind事件绑定不会阻止冒泡事件向上冒泡，**catch**事件绑定可以**阻止冒泡**事件向上冒泡。如：



```html
<view id="outer" bindtap="handleTap1">
  	outer view
    <view id="middle" catchtap="handleTap2">
    middle view
    <view id="inner" bindtap="handleTap3">
      inner view
    </view>
</view>

Page({
    handleTap1:function(event){  //点击输出outer view bindtap
      console.log("outer view bindtap")
    },
    handleTap2: function (event) {  //点击输出middle view
      console.log("middle view catchtap")
    },
    handleTap3: function (event) {  //点击输出inner view bindtap  middle view catchtap
      console.log("inner view bindtap")
    },
})
<view id="outer" bindtap="handleTap1">
  	outer view
    <view id="middle" bindtap="handleTap2">
    middle view
    <view id="inner" bindtap="handleTap3">
      inner view
    </view>
</view>

Page({
    handleTap1:function(event){  //点击输出outer view bindtap
      console.log("outer view bindtap")
    },
    handleTap2: function (event) {  //点击输出outer view bindtap middle view
      console.log("middle view catchtap")
    },
    handleTap3: function (event) {  //点击输出outer view bindtap inner view bindtap  middle view catchtap
      console.log("inner view bindtap")
    },
})
```

![1558399782111](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558399782111.png)



![1558399805782](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558399805782.png)![1558400049100](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558400049100.png)![1558400986395](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558400986395.png)![1558401301044](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558401301044.png)![1558401669488](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558401669488.png)![1558401783018](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558401783018.png)![1558401806483](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558401806483.png)![1558403515637](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558403515637.png)![1558404691262](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558404691262.png)

![1558517800996](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558517800996.png)![1558520533698](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558520533698.png)![1558523325547](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558523325547.png)![1558523376557](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558523376557.png)![1558523443272](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558523443272.png)![1558523724384](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1558523724384.png)

![jiagou](E:\TyporaFiles\img\jiagou.jpg)

### swiper

> 基础库 1.0.0 开始支持，低版本需做[兼容处理](../framework/compatibility.html)。

滑块视图容器。其中只可放置[swiper-item](swiper-item.html)组件，否则会导致未定义的行为。

| 属性                    | 类型        | 默认值            | 必填 | 说明                                                         | 最低版本                                 |
| ----------------------- | ----------- | ----------------- | ---- | ------------------------------------------------------------ | ---------------------------------------- |
| indicator-dots          | boolean     | false             | 否   | 是否显示面板指示点                                           | [1.0.0](../framework/compatibility.html) |
| indicator-color         | color       | rgba(0, 0, 0, .3) | 否   | 指示点颜色                                                   | [1.1.0](../framework/compatibility.html) |
| indicator-active-color  | color       | #000000           | 否   | 当前选中的指示点颜色                                         | [1.1.0](../framework/compatibility.html) |
| autoplay                | boolean     | false             | 否   | 是否自动切换                                                 | [1.0.0](../framework/compatibility.html) |
| current                 | number      | 0                 | 否   | 当前所在滑块的 index                                         | [1.0.0](../framework/compatibility.html) |
| interval                | number      | 5000              | 否   | 自动切换时间间隔                                             | [1.0.0](../framework/compatibility.html) |
| duration                | number      | 500               | 否   | 滑动动画时长                                                 | [1.0.0](../framework/compatibility.html) |
| circular                | boolean     | false             | 否   | 是否采用衔接滑动                                             | [1.0.0](../framework/compatibility.html) |
| vertical                | boolean     | false             | 否   | 滑动方向是否为纵向                                           | [1.0.0](../framework/compatibility.html) |
| previous-margin         | string      | "0px"             | 否   | 前边距，可用于露出前一项的一小部分，接受 px 和 rpx 值        | [1.9.0](../framework/compatibility.html) |
| next-margin             | string      | "0px"             | 否   | 后边距，可用于露出后一项的一小部分，接受 px 和 rpx 值        | [1.9.0](../framework/compatibility.html) |
| display-multiple-items  | number      | 1                 | 否   | 同时显示的滑块数量                                           | [1.9.0](../framework/compatibility.html) |
| skip-hidden-item-layout | boolean     | false             | 否   | 是否跳过未显示的滑块布局，设为 true 可优化复杂情况下的滑动性能，但会丢失隐藏状态滑块的布局信息 | [1.9.0](../framework/compatibility.html) |
| easing-function         | string      | "default"         | 否   | 指定 swiper 切换缓动动画类型                                 | [2.6.5](../framework/compatibility.html) |
| bindchange              | eventhandle |                   | 否   | current 改变时会触发 change 事件，event.detail = {current, source} | [1.0.0](../framework/compatibility.html) |
| bindtransition          | eventhandle |                   | 否   | swiper-item 的位置发生改变时会触发 transition 事件，event.detail = {dx: dx, dy: dy} | [2.4.3](../framework/compatibility.html) |
| bindanimationfinish     | eventhandle |                   | 否   | 动画结束时会触发 animationfinish 事件，event.detail 同上     |                                          |

![1560488298371](C:\Users\Wei\AppData\Roaming\Typora\typora-user-images\1560488298371.png)

































































































































































