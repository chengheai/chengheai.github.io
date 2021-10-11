---
title: vue-ssr服务端渲染添加多个swiper
date: 2019-08-27 20:51:48
tags: ['SSR','vue']
---
## 问题
一个ssr项目添加多个swiper会引起轮播图混乱。
## 前言
在使用nuxt.js实际开发项目中，会出现类似这样的情况，一个页面写两个【PC端一个，移动端一个】或者多个【其他需求】，在官方github有关服务端渲染的文档少之又少，只有一个简单的使用[demo](https://github.com/surmon-china/vue-awesome-swiper/blob/master/examples/nuxt-ssr-example/nuxt-ssr-example.vue),且与vue-cli项目使用还有点差别，那怎么解决？
## 单个使用
``` html
<template>
  <div v-swiper:mySwiper="swiperOption">
    <div class="swiper-wrapper">
      <div class="swiper-slide" v-for="banner in banners">
        <img :src="banner">
      </div>
    </div>
    <div class="swiper-pagination swiper-pagination-bullets"></div>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        banners: [
          '/1.jpg',
          '/2.jpg',
          '/3.jpg'
        ],
        swiperOption: {
          loop: true,
          slidesPerView: 'auto',
          centeredSlides: true,
          spaceBetween: 30,
          pagination: {
            el: '.swiper-pagination',
            dynamicBullets: true
          },
          on: {
            slideChange() {
              console.log('onSlideChangeEnd', this);
            },
            tap() {
              console.log('onTap', this);
            }
          }
        }
      }
    },
    mounted() {
      console.log('app init', this)
      setTimeout(() => {
        this.banners.push('/5.jpg')
        console.log('banners update')
      }, 3000)
      console.log(
        'This is current swiper instance object', this.mySwiper, 
        'I will slideTo banners 3')
       this.mySwiper.slideTo(3)
    }
  }
</script>
```
## 多个使用

``` html
<template>
  <div v-swiper:mySwiper="swiperOption" key='1'>
    <div class="swiper-wrapper">
      <div class="swiper-slide" v-for="banner in banners">
        <img :src="banner">
      </div>
    </div>
    <div class="swiper-pagination swiper-pagination-bullets"></div>
  </div>
  <div v-swiper:mySwiper="swiperOption" key='2'>
    <div class="swiper-wrapper">
      <div class="swiper-slide" v-for="banner in banners">
        <img :src="banner">
      </div>
    </div>
    <div class="swiper-pagination swiper-pagination-bullets"></div>
  </div>
  <div v-swiper:mySwiper="swiperOption" key='3'>
    <div class="swiper-wrapper">
      <div class="swiper-slide" v-for="banner in banners">
        <img :src="banner">
      </div>
    </div>
    <div class="swiper-pagination swiper-pagination-bullets"></div>
  </div>
  <div v-swiper:mySwiper="swiperOption" key='N'>
    <div class="swiper-wrapper">
      <div class="swiper-slide" v-for="banner in banners">
        <img :src="banner">
      </div>
    </div>
    <div class="swiper-pagination swiper-pagination-bullets"></div>
  </div>
</template>
<script>
// your codes
.....
</script>
```
这样一来就不会有重复的mySwiper，从而解决了多个swiper混乱的问题