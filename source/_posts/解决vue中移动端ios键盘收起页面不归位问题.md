---
title: 解决vue中移动端ios键盘收起页面不归位问题
date: 2020-03-24 20:36:07
tags: ['vue']
---

## 前言

网上找的例子
千篇一律～～～～～
![](https://github.com/chengheai/review-demo-image/blob/master/WX20200324-204643@2x.png?raw=true)

## 代码

```html
<template>
  <div>
    <div @focusout="inputBlur" @focusin="inputFocus">
      <input type="tel" placeholder="请输入手机号" />
      <input type="number" placeholder="请输入验证码" />
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        timer: null,
      };
    },
    methods: {
      inputBlur(e) {
        if (e && e.target && e.target.tagName && e.target.tagName.toLowerCase() === 'input') {
          this.timer = setTimeout(() => {
            window.scrollTo(0, 0);
          }, 0);
        }
      },
      inputFocus(e) {
        if (e && e.target && e.target.tagName && e.target.tagName.toLowerCase() === 'input') {
          clearTimeout(this.timer);
        }
      },
    },
  };
</script>

<style lang="scss" scoped></style>
```
