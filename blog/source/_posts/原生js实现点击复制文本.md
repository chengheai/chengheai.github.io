---
title: 原生js实现点击复制文本
date: 2019-08-05 20:54:12
tags: ['小技巧']
---
## 参考文档
[queryCommandState](https://developer.mozilla.org/en-US/docs/Web/API/Document/queryCommandState)
[clipboardData](https://developer.mozilla.org/en-US/docs/Web/API/ClipboardEvent/clipboardData)
[queryCommandSupported](https://developer.mozilla.org/en-US/docs/Web/API/Document/queryCommandSupported)
[execCommand](https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand)
## 预览
[demo](https://chengheai.github.io/daily-vue-demo/#/copy-board)
## 代码
``` javascript
  handleCopy(text) {
    if (window.clipboardData && window.clipboardData.setData) {
      return window.clipboardData.setData('Text', text);
    } else if (document.queryCommandSupported && document.queryCommandSupported('copy')) {
      const textarea = document.createElement('textarea');
      textarea.textContent = text;
      textarea.style.position = 'fixed';
      textarea.style.bottom = '0';
      textarea.style.zIndex = '99999';
      document.body.appendChild(textarea);
      textarea.select();
      try {
        return document.execCommand('copy');
      } catch (ex) {
        // eslint-disable-next-line
        console.warn('Copy to clipboard failed.', ex);
        return false;
      } finally {
        document.body.removeChild(textarea);
      }
    }
  }
```

