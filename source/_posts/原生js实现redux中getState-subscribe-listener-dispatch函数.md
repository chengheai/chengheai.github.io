---
title: '原生js实现redux中getState,subscribe,listener,dispatch函数'
date: 2020-03-01 20:05:52
tags: ['redux']
---

## 原生代码

```html
<!DOCTYPE html>
<html lang="en">
  <!-- 原生js实现dispatch 函数 -->

  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>

  <body>
    <div id="title">标题</div>
    <div id="content">内容</div>
    <script>
      var render = function(state) {
        document.getElementById('title').innerHTML = state.title;
        document.getElementById('content').innerHTML = state.content;
      };
      // 自定义一个dispatch过程
      var appReducer = function(state, action) {
        // 返回默认的state
        if (!state) {
          return {
            title: '初始化title',
            content: '初始化content',
          };
        }
        //返回相应的state
        switch (action.type) {
          case 'CHANGE_TITLE':
            // state修改
            return {
              ...state,
              title: action.newTitle,
            };
          default:
            return state;
        }
      };
      // 希望调用createStore就返回一个store
      // 纯函数 传入什么返回什么
      var createStore = function(reducer) {
        var state = null;
        // 监听者们
        var listeners = [];
        var dispatch = function(action) {
          //
          state = reducer(state, action); // reducer返回新的state
          listeners.forEach(listener => listener());
        };
        dispatch({});
        getState = function() {
          return state;
        };
        // 订阅
        var subscribe = function(listener) {
          listeners.push(listener);
        };
        return {
          subscribe,
          dispatch,
          getState,
        };
      };
      // store 包含subscribe,dispatch,getState
      var store = createStore(appReducer);

      store.subscribe(function() {
        render(store.getState());
      });
      store.subscribe(function() {
        console.log(store.getState());
      });
      store.dispatch({
        type: 'CHANGE_TITLE',
        newTitle: 'this is new title',
      });
      render(store.getState());
    </script>
  </body>
</html>
```
