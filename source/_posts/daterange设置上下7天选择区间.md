---
title: el-date-picker daterange设置上下7天选择区间
date: 2020-06-11 14:18:25
tags: ['element-ui']
---

## 需求

时间选择区间想个不能超过 7 天

## 代码

```html
<el-form-item label="搜索时间:">
	<el-date-picker
		v-model="form.rangeDate"
		value-format="yyyy-MM-dd"
		type="daterange"
		:picker-options="pickerOptions"
		clearable
		@change="handleFormChange"
	/>
</el-form-item>
```

```javascript
data(){
  let that = this // 关键
  return {
    pickerOptions: {
      disabledDate(time) {
        let timeOptionRange = that.timeOptionRange
        let secondNum = 60 * 60 * 24 * 7 * 1000
        if (timeOptionRange) {
          return time.getTime() > timeOptionRange.getTime() + secondNum || time.getTime() < timeOptionRange.getTime() - secondNum
        }
      }, onPick(time) {
        // 当第一时间选中才设置禁用
        if (time.minDate && !time.maxDate) {
          that.timeOptionRange = time.minDate
        }
        if (time.maxDate) {
          that.timeOptionRange = null
        }
      }
    }
  }
}
```
