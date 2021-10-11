---
title: vue中audio动态绑定src不能播放
date: 2019-09-05 20:55:50
tags: ['vue']
---
## 前言
略去。。。
## 代码
``` html
<audio id="audio" controls>
  <source :src="audUrl || '../assets/alarm.ogg'" type="audio/ogg" />
</audio>
```
## 问题
### 发现问题
绑定的时候,发现并不管用
``` javascript
let audio = document.querySelector('#audio')
axios.get()
  .then(res => {
    this.audUrl = res.data.xxx
  })
```
### 解决问题思路
～打印res.data.xxx是有值的。那为啥不生效呢？【 排除接口问题 】
～审查html audio元素，src属性是不是我们想要的返回值 【 排除页面属性挂载问题 】
～重新写一个audio标签写在页面上，引入接口返回的url地址 【 排除接口返回资源是否正确及audio标签是否正常 】
。。。。。

## 解决
代码改为
``` html
<audio ref='audio' controls>
  <source :src="audUrl || '../assets/alarm.ogg'" type="audio/ogg" />
</audio>
```
绑定的时候
``` javascript
this.$refs.audio.src = res.data.xxx
```
#### 下一节，Vue中实现微信语音播放暂停功能
[传送门](https://chengheai.github.io/2019/09/09/vue%E4%B8%ADaudio%E5%AE%9E%E7%8E%B0%E5%BE%AE%E4%BF%A1%E8%AF%AD%E9%9F%B3%E6%92%AD%E6%94%BE%E5%8A%A8%E7%94%BB/)
