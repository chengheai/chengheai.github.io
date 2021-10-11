---
title: 解决iOS12及一下弹框滚动会带动body内容滑动
date: 2019-11-20 21:32:55
tags: ['小技巧']
---

## 前言

在移动项目开发中往往会写一些 H5 页面，一个项目可能只有一两个页面，引入其他 UI 组件项目显得太臃肿，也没有啥必要，毕竟那些组件都是被人写的，有的时候不能一味的去使用他人写好的代码，有时间也需要自己谢谢，锻炼自己的同时也在巩固一些基础知识～～

## 插件【 NPM 】

[v-scroll-lock](https://github.com/phegman/v-scroll-lock)
这个插件虽然很好，但是在华为手机 📱 上却不起作用，而且还有不好的效果【亲测 🍄🐷🐸🦋】
[【在线测试】](https://v-scroll-lock.peterhegman.com/)

## 问题

一个页面弹出一个框，框里的内容可以滚动，但是滚动弹框时，body 内容也在动 【前提是你的 body 超过屏幕的高度】，然而在 iOS13 ，安卓手机没有问题，但是在 iOS12 及以下就会出现这个问题，通过代码 `document.body.style.overflow = "hidden";`，并不管用，原因是：在苹果 🧬 手机上就不存在这个属性，在它看来，我的页面有内容就该给客户全部展示出来，为什么要 hidden 呢 🍌，这个方法行不通时，我们准备给它添加另外一个属性`document.body.style.position = "fixed";`,发现可以正常，但是感觉怪怪的，因为 fixed 的 top 为 0

## 问题总结

～弹窗时给 body 设置 overflow:hidden；（ios 不理你）
～弹窗时给 body 设置 position:fixed；(滚动位置会丢失)

## 代码

```javascript
watch: {
  dialogStatus: function(news, olds) {
      if (news) {
        document.body.style.overflow = "hidden";
        let scrollTop =
          document.body.scrollTop || document.documentElement.scrollTop;
        document.body.style.cssText +=
          "position:fixed;width:100%;top:-" + scrollTop + "px;";
      } else {
        document.body.style.overflow = "";
        document.body.style.position = "static";
        let top = document.body.style.top;
        document.body.scrollTop = document.documentElement.scrollTop = -parseInt(
          top
        );
      }
    },
}
```
