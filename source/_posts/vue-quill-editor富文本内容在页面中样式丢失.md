---
title: vue-quill-editor富文本内容在页面中样式丢失
date: 2019-08-20 21:47:11
tags: ['富文本', 'vue']
---
## 现象
用富文本的情况有很多，例如在后台管理系统排版好的富文本页面，准备在移动端页面去显示，或者在官网显示，两个项目不在一起，在管理系统排好的样式在显示页面显示的一塌糊涂，为啥呢？大多数情况是样式没有引入。
## 解决
给容器增加一个class ql-editor，才能正常显示，另外前面是主题类名，不同的主题显示不同的样式
``` css
  <link href="http://cdn.quilljs.com/1.3.4/quill.snow.css" rel="stylesheet">
  <link href="http://cdn.quilljs.com/1.3.4/quill.bubble.css" rel="stylesheet">
  <link href="http://cdn.quilljs.com/1.3.4/quill.core.css" rel="stylesheet">
```
 我这边是snow，那么前缀就是ql-snow
``` html
<div class="ql-snow ql-editor" v-html="report.foreword"></div>
```
关于图片上传之后无法提交，报413错误 [ Request Entity Too Large ],解决方法[将图片上传到七牛云](https://chengheai.github.io/2019/08/15/vue-quill-editor%E5%AF%8C%E6%96%87%E6%9C%AC%E7%BC%96%E8%BE%91%E5%99%A8%E5%AE%9E%E7%8E%B0%E5%9B%BE%E7%89%87%E6%8F%92%E5%85%A5%E4%B8%83%E7%89%9B%E4%BA%91%E4%B8%8A%E4%BC%A0/)
