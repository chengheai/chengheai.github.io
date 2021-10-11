---
title: Mint-ui-infinite-scroll解决重复加载
date: 2019-01-22 10:40:09
tags: [Mint-ui, vue]
---
## 第一种方法（当数据不多的情况下使用【比如排行榜10条】）
### 思路
每次上滑的时候都调一次接口，直到没有数据就将infinite-scroll-disabled 设为true，每次截取slice(i * 3,  (i + 1) * 3）; 3是每次显示的条数
## 代码
``` html
<div class="r-contatin">
  <ul
    class="r-item-wrapper"
    v-infinite-scroll="loadMore"
    infinite-scroll-disabled="loading"
    infinite-scroll-distance="10"
  >
    <li v-for="(item, key) in list" :key="key">
      <div>{{item}}=={{key}}</div>
    </li>
  </ul>
</div>

<p v-if="!noMore" class="loading-tip">
  <mt-spinner color="rgba(255, 51, 153, 1)" type="fading-circle"></mt-spinner>
  <span style="margin-left: 5px;">更多加载中...</span>
</p>
<p class="no-more" v-else>没有更多了~</p>
```
``` javascript
data() {
  return {
    list: [],
    moreList: [],
    i: 0,
    loading: false,
    noMore: false,
  };
},
filters: {
},
mounted() {
  this.getData(this.obj);
},
methods: {
  getData(obj) {
    let i = 0;
    let that = this;
    axios.post(obj)
      .then(res => {
        console.log(res);
        if (res.data.code === "200") {
          that.list = res.data.result.rankingDetailList.slice(
            i * 3,
            (i + 1) * 3
          );
        } else {
          Toast("载入失败");
        }
      })
      .catch(err => {
        Toast("载入失败");
        console.log(err);
      });
  },
  loadMore() {
    let that = this;
    that.loading = true;
    that.noMore = false;
    axios.post(that.obj)
      .then(res => {
        that.i++;
        console.log(res);
        that.moreList = res.data.result.rankingDetailList.slice(
          that.i * 3,
          (that.i + 1) * 3
        );
        if (that.moreList.length === 0) {
          that.noMore = true;
          that.loading = true;
        } else {
          that.loading = false;
          that.noMore = false;
          that.moreList.forEach(function(item) {
            that.list.push(item);
          });
        }
      })
      .catch(function(error) {
        console.info(error);
      });
  },
}
```
## 第二种方法（当数据很多并有分页的情况下使用【比如点菜菜单 >= 20 条】）
### 思路
第一次进来传第一页，之后每次上滑page++，当返回的数据长度为0时，把上滑功能关闭
##代码
``` html
<div class="real-content" v-infinite-scroll="busy" infinite-scroll-distance="10">
  <div class="item" v-for="g in good" :key="g.item">
    ...
  </div>
</div>
```
``` javascript
loadMore(){
	this.busy = true;
	this.pageSize++;
  axios({
    //请求数据
	})
  this.busy = false;

```
