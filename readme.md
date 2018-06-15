# 个人小程序笔记（前端）

标签（空格分隔）： 前端 小程序

---

> 微信小程序开发者文档：https://developers.weixin.qq.com/miniprogram/dev/

> the docs for the WeChat Mini Program: https://developers.weixin.qq.com/miniprogram/dev/


- [x] 文章列表前台的开发
- [ ] 后台开发（连接微信公众号和微信小程序）
- [ ] 前后端整合发布

## 新闻列表

### Swiper（轮播图） 组件


**html**
```html
<template name="swiperItem">
  <image class="swiper-image" src="{{imgSrc}}"></image>
  <view class="swiper-container">
    <text class="swiper-date">{{date}}</text>
    <text class="swiper-title">{{title}}</text>
    <view>
      <image class="swiper-avatar" src="{{avatar}}"></image>
      <text class="swiper-author">{{author}}</text>
    </view>
  </view>
</template>

<swiper catchtap="onSwiperTap" vertical="{{false}}" indicator-dots="true" autoplay="true" interval="5000">
  <block wx:for="{{swiperList}}" wx:for-item="item" wx:for-index="idx">
    <swiper-item catchtap="onPostTap" data-postId="{{item.postId}}">
      <template is="swiperItem" data="{{...item}}" />
    </swiper-item>
  </block>
</swiper>
```

坑点：swiper组件样式只能加在swiper标签上，不能加在swiper-item上。

### 数据绑定

WXML 中的动态数据均来自对应 Page 的 data。

数据绑定使用 Mustache 语法（双大括号）将变量包起来，可以作用于：

**html**
```html
<view> {{ message }} </view>
```

**js**
```js
Page({
  data: {
    message: 'Hello MINA!'
  }
})
```

组件属性\关键字\控制属性(需要在双引号之内)

PS：可以通过APPDATA来查看数据绑定的情况

**html**
```html
<view id="item-{{id}}"> </view>
```

**js**
```js
Page({
  data: {
    id: 0
  }
})
```

在js里可以通过this.setData({})来实现

```js
this.setData({
       postList:myPostList,
       swiperList: mySwiperList
      });
```


### 事件与事件对象

什么是事件？

- 事件是视图层到逻辑层的通讯方式。
- 事件可以将用户的行为反馈到逻辑层进行处理。
- 事件可以绑定在组件上，当达到触发事件，就会执行逻辑层中对应的事件处理函数。
- 事件对象可以携带额外信息，如 id, dataset, touches。

事件绑定和冒泡

事件绑定的写法同组件的属性，以 key、value 的形式。

key 以bind或catch开头，然后跟上事件的类型，如bindtap、catchtouchstart。自基础库版本 起，bind和catch后可以紧跟一个冒号，其含义不变，如bind:tap、、catch:touchstart。
value 是一个字符串，需要在对应的 Page 中定义同名的函数。不然当触发事件的时候会报错。
bind事件绑定不会阻止冒泡事件向上冒泡，catch事件绑定可以阻止冒泡事件向上冒泡。

### Template模板的使用[复用]

WXML提供模板（template），可以在模板中定义代码片段，然后在不同的地方调用。

定义模板
使用 name 属性，作为模板的名字。然后在<template/>内定义代码片段，如：

```js
<!--
  index: int
  msg: string
  time: string
-->
<template name="msgItem">
  <view>
    <text> {{index}}: {{msg}} </text>
    <text> Time: {{time}} </text>
  </view>
</template>
```
