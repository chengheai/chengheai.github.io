---
title: el-input在vue中优雅实现禁止输入特殊字符
date: 2019-04-15 21:10:10
tags: ['element', 'vue']
---
### 前提补充
在vue中
``` html
<input v-model="text" />
```
等价于
``` html
<input
 :value="text"
 @input="e => text = e.target.value"
/>
```
### 需求
前端提交form表单要求，不能输入 @#¥%……&*.....
不是提示，而是 <font color="#660000">禁止输入</font>
### 效果
![](https://github.com/chengheai/review-demo-image/blob/master/2019-04-16%2009-48-19.2019-04-16%2009_49_10.gif?raw=true)
### 代码
** 在mian.js中添加【vue原型上添加方法，便于全局使用】
``` javascript
Vue.prototype.validForbid = function (value, number = 255) {
  value = value.replace(/[`~!@#$%^&*()_\-+=<>?:"{}|,./;'\\[\]·~！@#￥%……&*（）——\-+={}|《》？：“”【】、；‘’，。、]/g, '').replace(/\s/g, "");
  if (value.length >= number) {
    this.$message({
      type: "warning",
      message: `输入内容不能超过${number}个字符`
    });
  }
  return value;
}
```
** 在component.vue中加入
``` html
<el-input
  :value="form.name"
  @input="e => form.name = validSe(e)"
  maxlength="10"
  placeholder="过滤特殊字符长度10"
></el-input>
```
### over

#### 听说你要在线测试⁉️ [那就点我🐙吧](https://chengheai.github.io/daily-vue-demo/#/input)