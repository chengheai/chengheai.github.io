---
title: MarkDown
date: 2018-10-10 16:09:34
categories:
  - MarkDown学习笔记
tags: 
  - Nodejs
  - Angularjs
  - Angular
  - React
  - dva
  - JavaScript
  - Java
  - bash
  - MongoDB
  - HTML
  - CSS2
  - ES6
  - Git
  - Mac
---
## MarkDown 常用
### 1、标题 `格式：# 标题`

# 一级标题(# 一级标题)
## 二级标题(## 二级标题)
### 三级标题(### 三级标题)
#### 四级标题(#### 四级标题)
##### 五级标题(##### 五级标题)
###### 六级标题(###### 六级标题)

### 2、列表 

#### 2.1 有序列表 `格式：1.列表项`
1.列表项1（1.列表项1）
2.列表项2（2.列表项2）
3.列表项3（3.列表项3）
#### 2.2 无序列表 `格式：-列表项、*列表项、+列表项`
- 列表项1（- 列表项1）
+ 列表项2（+ 列表项2）
* 列表项3（* 列表项3）

### 3、链接与图片

#### 3.1 链接 `格式：\[文本](链接)`
\[百度](http://www.baidu.com)
#### 3.2 图片 `格式：\![文本]\(图片链接)`
![logo](/images/logo.png)
### 4、表格
###### 语法：`第二行中“—-|：–：|—”冒号用于设置表格的对齐方式，左边表示左对齐，右边表示右对齐，放两边表示居中，不放冒号（默认状态）表示表头居中内容居左。`
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
###### 简写模式，省略了两边的“|”。
``` bash
dog | bird  | cat
-----|-----|-----
bar  | bear | box
bar  | bear | box

```
dog | bird  | cat
-----|-----|-----
bar  | bear | box
bar  | bear | box
### 5、粗体和斜体
**我是粗体**`(**我是粗体**)`
*我是斜体* `(*我是斜体*)`
### 6、代码编写
``` javascript
(function (window, document) {
      var getRem = function () {
        if (document) {
          var html = document.documentElement;
          var hWidth = (html.getBoundingClientRect().width) * (750 / 352);
          console.log(hWidth)
          html.style.fontSize = hWidth / 16 + "px";
          console.log(html.style.fontSize)
        }
      };
      getRem();
      window.onresize = function () {
        getRem();
      }
})(window, document)

```

