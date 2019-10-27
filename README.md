# 小程序开发实践

> 小程序不是更强大的公众号，是新的应用形态 ——张小龙

### 原生微信小程序

参考 [为何我们要用 React 来写小程序 - Taro 诞生记](https://aotu.io/notes/2018/06/25/the-birth-of-taro/index.html)
总结的四个问题

1. 单页面主文件结构繁琐（ts或js, json, wxml, wxcss）
2. 语法既像React, 又像Vue
3. 组建/方法命名规范不统一（中划线分割/单词连写/驼峰写法 混杂）
4. 不完整的前端开发体验（webpack、ES6+语法、CSS预处理器等的缺失）

开发方式：

1. 像素单位rpx，使用750宽度适配
2. 保持页面层级，避免过多嵌套

### Taro多端开发

第三方框架一览
目前（2019 年 3 月）——

WePY （类 Vue.js）
mpvue （Vue.js + H5/百度/字节/支付宝）
Min （类 Vue.js）
Taro （React.js + H5/百度/字节/支付宝/RN）
nanachi （React.js + H5/百度/字节/支付宝）
uni-app （Vue.js + H5/百度/字节/支付宝/Native）

在这里我们对比下原生小程序、Uni-app 和 Taro 的语法

```js
<swiper indicator-dots="{{indicatorDots}}"
  autoplay="{{autoplay}}" interval="{{interval}}" duration="{{duration}}">
  <block wx:for="{{background}}" wx:key="*this">
    <swiper-item>
      <view class="swiper-item {{item}}"></view>
    </swiper-item>
  </block>
</swiper>
```

```js
<swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
    <swiper-item>
        <view class="swiper-item uni-bg-red">A</view>
    </swiper-item>
    <swiper-item>
        <view class="swiper-item uni-bg-green">B</view>
    </swiper-item>
    <swiper-item>
        <view class="swiper-item uni-bg-blue">C</view>
    </swiper-item>
</swiper>
```

```jsx
<Swiper className='test-h' indicatorColor={{indicatorDots}} indicatorActiveColor='#333' vertical circular indicatorDots autoplay>
  <SwiperItem>
    <View className='demo-text-1'>1</View>
  </SwiperItem>
  <SwiperItem>
    <View className='demo-text-2'>2</View>
  </SwiperItem>
  <SwiperItem>
    <View className='demo-text-3'>3</View>
  </SwiperItem>
</Swiper>
```

不管Uni-app 还是 Taro, 都是为了表现微信小程序语法而适配的实现方式。所以在开发之前，至少都看下微信小程序官方文档。

开发体验：

1. 对接口方法用 Taro. + API 名称来进行调用，如果出现未定义，则使用对应小程序平台的命名空间（如 wx、my、swan、tt 等）来进行调用。
2. 样式选择：Sass、Less 和 stylus 随便选择
3. 区分开发和线上环境的 API和变量: 在config中的dev.js和prod.js中设置： defineConstants

#### 解决打包后样式丢失等问题：

如果你在开发中遇到了开发环境时样式没有问题，但是编译打包后出现部分样式丢失，可能是因为 csso 的 restructure 特性。可以在 plugins.csso 中将其关闭：

#### 解决编译压缩过的 js 文件出错的问题

如果你遇到了编译时，压缩过的 js 文件过编译器报错，可以将其排除编译：
```js
weapp: {
  compile: {
    exclude: [
      'src/utils/qqmap-wx-jssdk.js',
      'src/components/third-party/wemark/remarkable.js',
    ],
  },
},
```
可以配置中根据weapp、h5来配置编译方式（配置不同的webpack）

#### 引入自定义组件

比如使用 [wemark组件](https://github.com/TooBug/wemark)的步骤的例子

#### 实现雪碧图

> todo

扩展阅读： [Taro 优秀学习资源汇总](http://taro-club.jd.com/topic/17/taro-%E4%BC%98%E7%A7%80%E5%AD%A6%E4%B9%A0%E8%B5%84%E6%BA%90%E6%B1%87%E6%80%BB)

### 小程序Canvas开发： Spritejs

[Spritejs](http://spritejs.org/#/) 是一款由360奇舞团开源的跨终端canvas绘图库，可以基于canvas快速绘制结构化UI、动画和交互效果，并发布到任何拥有canvas环境的平台上（比如浏览器、小程序和node）。
由于是npm包，可以很好的集成到taro中。

### 扩展：小游戏

微信小游戏是Canvas实现的，官方推荐的Cocos、Egret、Laya