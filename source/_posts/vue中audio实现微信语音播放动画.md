---
title: vue中audio实现微信语音播放动画
date: 2019-09-09 20:58:24
tags: ['vue']
---
## 预览
![](https://github.com/chengheai/review-demo-image/blob/master/2019-09-09%2021-01-51.2019-09-09%2021_02_34.gif?raw=true)
## 思路
拿到时长做倒计时，时长 = （ 时长 + 1 ） * 100; destroy的时候清除一下
## 代码
``` html
<div style="display:none">
  <audio controls="controls" ref="audio">
    <source :src="url || ''" type="audio/mpeg" />
  </audio>
</div>
<div class="voice-bg" @click="handlePlay">
  <img
    v-if="isPlaying"
    src="./../../assets/images/goods/voice-play.gif"
    alt=""
  />
  <img
    v-else
    src="./../../assets/images/goods/voice.png"
    alt=""
  />
  <span>{{ duration || 0 }}"</span>
</div>
```
``` javascript
data() {
  return {
    url: '',
    timer: null,
    isPlaying: false,
    currentTime: 0,

  }
},
methods:{
  handlePlay() {
    let audio = this.$refs.audio;
    if (!this.isPlaying) {
      audio.play();
      this.isPlaying = true;
      this.watchEnd();
    } else {
      audio.pause();
      clearTimeout(this.timer);
      this.isPlaying = false;
      audio.currentTime = 0;
    }
  },
  watchEnd() {
    let that = this;
    this.timer = setTimeout(() => {
      that.isPlaying = false;
    }, (that.duration + 1) * 1000);
  },
},
beforeDestroy() {
  clearTimeout(this.timer);
}
```
## 资料
### 关于获取audio信息接口
[七牛云🐂🐃🐕🐄](https://developer.qiniu.com/dora/api/1247/audio-and-video-metadata-information-avinfo)
``` javascript
this.$axios.get(`${this.info.url}?avinfo`).then(res => {
  let { streams } = res.data;
  this.duration = parseInt((streams && streams[0].duration) || 0);
});
```
### UI图片
[下载地址](https://github.com/chengheai/chengheai.github.io/issues/1)
[isPlaying gif](https://github.com/chengheai/chengheai.github.io/issues/1)
[isPause png](https://github.com/chengheai/chengheai.github.io/issues/1)
