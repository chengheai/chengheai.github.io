---
title: js原生方法实现react开发模式与组件化包装
date: 2020-02-25 22:04:02
tags: ['react']
---

## react 原生开发模式

```html
<!-- 更加接近react的开发模式 -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>

  <body>
    <div id="root"></div>
    <script>
      var root = document.getElementById('root');
      // 状态
      var state = {
        like: false,
      };
      var setState = function(newState) {
        state = {
          ...state,
          ...newState,
        };
        render();
      };
      // 渲染
      var render = function() {
        root.innerHTML = `
      <button type='button' style='color:${state.like ? 'red' : 'black'}' onclick='handleClick()'>
      ${state.like ? '已赞' : '喜欢'}
      </button>
      `;
      };
      var handleClick = function() {
        setState({
          like: !state.like,
        });
      };
      render();
    </script>
  </body>
</html>
```

## 组件化包装

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <!-- 组件化包装 -->

  <body>
    <div id="root"></div>
    <script>
      var root = document.getElementById('root');
      // 提取setState方法
      class Component {
        setState(newState) {
          this.state = {
            ...this.state,
            ...newState,
          };
        }
      }
      // extends 继承组件Component的setState方法 + super()
      class Button extends Component {
        constructor() {
          super();
          this.state = {
            like: true,
          };
          this.render();
        }
        // setState(newState) {
        //   this.state = {
        //     ...this.state,
        //     newState
        //   }
        //   this.render()
        // }
        render() {
          const state = this.state;
          return `
           <button type='button' style='color:${state.like ? 'red' : 'black'}'>
      ${state.like ? '已赞' : '喜欢'}
      </button>
        `;
        }
      }
      // extends 继承组件Component的setState方法 + super()
      class Title extends Component {
        constructor() {
          super();
          this.state = {
            text: '这是一个标题',
          };
        }
        // setState(newState) {
        //   this.state = {
        //     ...this.state,
        //     newState
        //   }
        // }
        render() {
          return `<h1>${this.state.text}</h1>`;
        }
      }
      // 组件复用
      class Wrap {
        constructor() {}
        render() {
          return `Wrap ${new Title().render()} ${new Button().render()}`;
        }
      }
      root.innerHTML = new Wrap().render();
    </script>
  </body>
</html>
```
