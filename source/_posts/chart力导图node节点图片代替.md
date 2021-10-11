---
title: chart力导图node节点图片代替
date: 2019-02-14 17:14:30
tags: ['charts']
---

### 效果

![](https://github.com/chengheai/review-demo-image/blob/master/WX20190214-171553.png?raw=true)
### 源码
[在线demo](https://jsfiddle.net/BlackLabel/ezo65rv1/12/)
### 代码

```javascript
chart: {
      type: 'networkgraph'
  },

  plotOptions: {
      networkgraph: {
          layoutAlgorithm: {
              enableSimulation: true
          }
      }
  },

  series: [{
      link: {
          width: 2
      },
      data: [{
          from: 'chengheai-1',
          to: 'chengheai-2'
      }, {
          from: 'chengheai-2',
          to: 'chengheai-3'
      }],
      nodes: [{
          id: 'chengheai-1',
          marker: {
              width: 20,
              height: 20,
              symbol: 'url(https://avatars3.githubusercontent.com/u/26807610?s=88&v=4)'
          }
      }, {
          id: 'chengheai-2',
          marker: {
              width: 40,
              height: 40,
              symbol: 'url(https://avatars3.githubusercontent.com/u/26807610?s=88&v=4)'
          }
      }, {
          id: 'chengheai-3',
          marker: {
              width: 40,
              height: 40,
              symbol: 'url(https://avatars3.githubusercontent.com/u/26807610?s=88&v=4)'
          }
      }]
  }]
```
