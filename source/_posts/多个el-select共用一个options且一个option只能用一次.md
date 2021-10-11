---
title: 多个el-select共用一个options且一个option只能用一次
date: 2019-10-21 22:01:16
tags: ['element-ui']
---

## 需求

一个表单多个下拉框，假如 options 里有中国，美国，第一个 select 选择里中国那么第二个 select 的 options 中国就置为灰色，反之我第二个选里中国，第一个 options 中中国就置为灰色

## 效果

![](https://github.com/chengheai/review-demo-image/blob/master/2019-10-21%2022-06-05.2019-10-21%2022_07_14.gif?raw=true)

## DEMO

<font size=8>[在线 demo ⛩⛽️🚁✈️📡💈🧧🏮](https://chengheai.github.io/daily-vue-demo/#/select)</font>

## 代码

```html
<template>
  <div>
    <el-form :model="ruleForm" ref="ruleForm" label-width="100px" class="demo-ruleForm">
      <div v-for="(item, index) in ruleForm.list" :key="index">
        <el-select
          v-model="item.type"
          clearable
          @change="selectChange(item.type,index)"
          placeholder="请选择"
        >
          <el-option
            v-for="(item,optionIndex)  in options"
            :key="optionIndex"
            :label="item.label"
            :disabled="getDisable(item.value)"
            :value="item.value"
          ></el-option>
        </el-select>
        <i
          v-if="index === 0 && ruleForm.list[0].type !== '' && ruleForm.list.length<2"
          class="el-icon-circle-plus-outline ico"
          @click="add"
        ></i>
        <i v-if="index !==0" class="el-icon-remove-outline ico" @click="del(index)"></i>
      </div>
    </el-form>
  </div>
</template>
```

```javascript
<script>
/* eslint-disable */
// 添加数组自定义事件 用来删除数组中的某一项
Array.prototype.indexOf = function(val) {
  for (var i = 0; i < this.length; i++) {
    if (this[i] == val) {
      return i;
    }
  }
  return -1;
};
Array.prototype.remove = function(val) {
  var index = this.indexOf(val);
  if (index > -1) {
    this.splice(index, 1);
  }
};
export default {
  data() {
    return {
      ruleForm: {
        list: [
          {
            type: '',
          },
        ],
      },
      options: [
        {
          label: '中国',
          value: 1,
        },
        {
          label: '美国',
          value: 2,
        },
      ],
      selectedOptions: [],
    };
  },
  methods: {
    selectChange(value, index) {
      console.log(arguments);
      console.log(value, index);
      this.selectedOptions[index] = value;
    },
    getDisable(value) {
      if (this.selectedOptions.indexOf(value) >= 0) {
        return true;
      } else {
        return false;
      }
    },
    add() {
      this.ruleForm.list.push({
        type: '',
      });
    },
    del(index) {
      // 删除时恢复可选
      if (this.ruleForm.list[index] || this.ruleForm.list[index] == 0) {
        this.selectedOptions.remove(this.ruleForm.list[index].type);
      }
      console.log(this.selectedOptions);
      this.ruleForm.list.splice(index, 1);
    },
  },
};
</script>

<style lang="css" scoped>
.ico {
  font-size: 40px;
}
</style>

```
