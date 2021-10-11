---
title: vue中@keyup.enter.native输入法的回车与消息发送快捷键回车的冲突解决方法
date: 2018-12-24 10:33:58
tags: [vue, element]
---
## 问题
*当你在中文输入法下输入然后回车后，发现表单提交了。
### 代码
``` html
<el-col :span="14">
  <el-input type="text" v-model="loginForm.content" autocomplete="off" @keyup.enter.native ="submitForm('loginForm')" placeholder="图形验证码">
    <template slot="prepend"><span class="fa fa-picture-o" style="width: 13px"></span></template>
  </el-input>
</el-col>
```
### 结果
![文本](/images/WechatIMG27.png)
## 解决方法
### 代码更换
``` html
@keyup.enter.native ="submitForm('loginForm')"

                ||
                ||
                ||
              \\||//
               \\//
                ||
                👇
                👇
↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

@keypress.enter.native ="submitForm('loginForm')"
```

