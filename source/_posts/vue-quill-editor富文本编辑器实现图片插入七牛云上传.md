---
title: vue-quill-editor富文本编辑器实现图片插入七牛云上传
date: 2019-08-15 22:23:26
tags: ['vue', '富文本']
---
## 前言
vue-quill-editor是我们再使用vue框架的时候常用的一个富文本编辑器，在进行富文本编辑的时候，我们往往要插入一些图片，vue-quill-editor默认的处理方式是直接将图片转成base64编码，这样的结果是整个富文本的html片段十分冗余，通常来讲，每个服务器端接收的post的数据大小都是有限制的，这样的话有可能导致提交失败，数据要传递很久才全部传送到服务器。
因此，在富文本编辑的过程中，对于图片的处理，我们更合理的做法是将图片上传到服务器，再将图片链接插入到富文本中。
## 代码
### template
``` html
<template>
  <div>
    <!-- 图片上传组件辅助-->
    <el-upload
      class="avatar-uploader"
      :action="serverUrl"
      name="img"
      :headers="header"
      :show-file-list="false"
      :on-success="uploadSuccess"
      :on-error="uploadError"
      :before-upload="beforeUpload"
    ></el-upload>
    <!--富文本编辑器组件-->
    <el-row v-loading="quillUpdateImg">
      <quill-editor
        v-model="detailContent"
        ref="myQuillEditor"
        :options="editorOption"
        @change="onEditorChange($event)"
        @ready="onEditorReady($event)"
      ></quill-editor>
    </el-row>
  </div>
</template>
```
### script
``` javascript
// 工具栏配置
const toolbarOptions = [
  ['bold', 'italic', 'underline', 'strike'], // toggled buttons
  ['blockquote', 'code-block'],

  [{ header: 1 }, { header: 2 }], // custom button values
  [{ list: 'ordered' }, { list: 'bullet' }],
  [{ script: 'sub' }, { script: 'super' }], // superscript/subscript
  [{ indent: '-1' }, { indent: '+1' }], // outdent/indent
  [{ direction: 'rtl' }], // text direction

  [{ size: ['small', false, 'large', 'huge'] }], // custom dropdown
  [{ header: [1, 2, 3, 4, 5, 6, false] }],

  [{ color: [] }, { background: [] }], // dropdown with defaults from theme
  [{ font: [] }],
  [{ align: [] }],
  ['link', 'image', 'video'],
  ['clean'], // remove formatting button
];
export default {
  data() {
    return {
      serverUrl: '', // 这里写你要上传的图片服务器地址
      header: { token: sessionStorage.token }, // 有的图片服务器要求请求头需要有token之类的参数，写在这里
      detailContent: '', // 富文本内容
      editorOption: {
        placeholder: '',
        theme: 'snow', // or 'bubble'
        modules: {
          toolbar: {
            container: toolbarOptions, // 工具栏
            handlers: {
              image: function(value) {
                if (value) {
                  document.querySelector('#quill-upload input').click();
                } else {
                  this.quill.format('image', false);
                }
              },
            },
          },
        },
      },
    };
  },
  methods: {
    // 富文本图片上传前
    beforeUpload() {
      // 显示loading动画
      this.quillUpdateImg = true;
    },

    uploadSuccess(res, file) {
      // res为图片服务器返回的数据
      // 获取富文本组件实例
      let quill = this.$refs.myQuillEditor.quill;
      // 如果上传成功
      if (res.status === 200) {
        // 获取光标所在位置
        let length = quill.getSelection().index;
        // 插入图片  res.info为服务器返回的图片地址
        quill.insertEmbed(length, 'image', res.info);
        // 调整光标到最后
        quill.setSelection(length + 1);
      } else {
        this.$message.error('图片插入失败');
      }
      // loading动画消失
      this.quillUpdateImg = false;
    },

    // 富文本图片上传失败
    uploadError() {
      // loading动画消失
      this.quillUpdateImg = false;
      this.$message.error('图片插入失败');
    },
    onEditorReady(quill) {
      console.log('editor ready!', quill);
    },
    onEditorChange({ quill, html, text }) {
      console.log('editor change!', quill, html, text);
      this.detailContent = html;
    },
  },
};
```