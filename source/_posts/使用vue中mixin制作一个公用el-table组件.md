---
title: 使用vue中mixin制作一个公用el-table组件
date: 2019-07-09 13:20:17
tags: ['vue', 'element-ui']
---
## 官网原话
混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项
## 功能
～1.选项合并
～1.全局混入
～1.自定义选项合并策略
具体看官网介绍：https://cn.vuejs.org/v2/guide/mixins.html

## 需求
假如我们的项目中有很多重复类似的页面，比如admin管理系统中，有很多很多的表格页面，你是不是每个页面都去写pageNum，pageSize，handleSizeChange，handleCurrentChange.....
这样一来页面太多重复代码了。那怎么提取优化呢？🐳。你可以组件提出来写。经网上查阅资料所得：
*1. 组件在引用之后相当于在父组件内开辟了一块单独的空间，来根据父组件props过来的值进行相应的操作，单本质上两者还是泾渭分明，相对独立。<br />
*2. mixin则是在引入组件之后，则是将组件内部的内容如data等方法、method等属性与父组件相应内容进行合并。相当于在引入后，父组件的各种属性方法都被扩充了
  单纯组件引用：
    => 父组件 + 子组件 >>> 父组件 + 子组件
  mixin：
    => 父组件 + 子组件 >>> new父组件

## 实例
### 想要实现的内容
把项目中table非常多的东西都是可以复用的例如分页，表格高度，加载方法， loading 声明等一大堆的东西。下面我们来整理出来一个简单通用混入 list.js
## DEMO
[👻 点我呀 🐳](https://chengheai.github.io/daily-vue-demo/#/table)
[ react👅 + redux🌈 + dvajs🦞 + mobx👺 + antd-design🤡](https://chengheai.github.io/daily-react-demo/index.html#/)
## list代码
list.js
``` javascript

const list = {
  data() {
    return {
      loading: false,
      pageParam: {
        pageNum: 1, // 页码
        pageSize: 10, // 每页显示条数
      },
      total: 0, // 总数
      pageSizes: [10, 20, 30, 50], // 每页条数
      pageLayout: 'total, sizes, prev, pager, next, jumper', // 分页布局
      pagerCount: 5, // 大于等于 5 且小于等于 21 的奇数
      list: [],
    };
  },
  methods: {
    // 分页回掉事件
    handleSizeChange(val) {
      this.pageParam.pageSize = val;
      this.getList();
    },
    handleCurrentChange(val) {
      this.pageParam.pageNum = val;
      this.getList();
    },
    /**
     * 请求返回的回调
     * @param {*} apiResult
     * @returns {*} promise
     */
    listSuccessCb(apiResult = {}) {
      return new Promise((resolve, reject) => {
        let tempList = []; // resolve return出的list
        try {
          this.loading = false;
          tempList = apiResult.data.data.list;
          this.total = apiResult.data.totalCount;
          // 直接抛出
          resolve(tempList);
        } catch (error) {
          reject(error);
        }
      });
    },
    /**
     * 处理异常情况
     * 错误时将loading置为 false
     */
    listExceptionCb(error) {
      this.loading = false;
      console.error(error);
    },
  },
  created() {
    this.$nextTick().then(() => {
      console.log('我是table mixin')
    });
  },
};
export default list;

```
## 应用代码
``` javascript
<template>
  <div>
    <el-table
      :data="list"
      v-loading="loading"
      style="width: 100%">
      <el-table-column
        prop="date"
        label="日期"
        width="180">
      </el-table-column>
      <el-table-column
        prop="name"
        label="姓名"
        width="180">
      </el-table-column>
      <el-table-column
        prop="address"
        label="地址">
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :page-sizes="pageSizes"
      :pager-count="pagerCount"
      :layout="pageLayout"
      :total="total">
    </el-pagination>
  </div>
</template>

<script>
import mixin from '@/mixins/list' // 引入
export default {
  name: 'mixins-demo',
  mixins: [mixin], // 使用mixins
  data () {
    return {
      searchForm: {
        // age: 18,
        // six: 'male'
      }
    }
  },
  methods: {
    // 加载列表
    getList () {
      this.loading = true;
      const params = { ...this.searchForm, ...this.pageParam }
      this.$http.get('https://www.api.com/api/v1/query', {params}).then(res => {
        if (res.data.code === 0) {
          this.listSuccessCb(res).then((list) => {
            this.list = list
          }).catch((err) => {
            this.listExceptionCb(err)
            // console.log(err)
          })
        }
      })
    }
  },
  created() {
    console.log('我是组件自己的created');
  },
  mounted() {
    this.getList()
  }
}
</script>

<style>

</style>

```
使用了 mixins 之后一个简单的有 loading, 分页,数据的表格大概就只需要上面这些代码。

现在在list.js中我们可以直接调用组件的方法，比如在分页回调事件中调用组件的 getList()方法，在组件中直接调用 list.js中的代码，如直接访问 this.pageParam


