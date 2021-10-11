---
title: vue项目按需使用keep-alive
date: 2019-05-23 22:05:40
tags: ['vue', 'vue-router']
---

## 需求

list 页面做缓存
add/edit 页面不做缓存
detail 页面不做缓存
...........

## 前人法子

```javascript
<keep-alive>
  <!-- 需要缓存 -->
  <router-view :include="include" v-if="$route.meta.keepAlive" />
</keep-alive>

<!-- 不需要缓存 -->
<router-view v-if="!$route.meta.keepAlive" />
```

```javascript
export default {
	name: 'app',
	data: () => ({
		include: [],
	}),
	watch: {
		$route(to, from) {
			//如果要to的页面是需要 keepAlive 缓存的，把 name push 进 include数组
			if (to.meta.keepAlive) {
				!this.include.includes(to.name) && this.include.push(to.name);
			}
			//如果要form的页面是 keepAlive缓存的，
			//再根据 deepth 来判断是前进还是后退
			//如果是后退
			if (from.meta.keepAlive && to.meta.deepth < from.meta.deepth) {
				var index = this.include.indexOf(from.name);
				index !== -1 && this.include.splice(index, 1);
			}
		},
	},
};
```

## 粗暴法子
``` html
<transition name="move" mode="out-in">
  <keep-alive>
    <!-- 需要缓存的视图组件 -->
    <router-view v-if="$route.meta.keepAlive" />
  </keep-alive>
</transition>
<!-- 不需要缓存的视图组件 -->
<router-view v-if="!$route.meta.keepAlive" />
```
``` javascript

watch: {
  $route(to, from) {
    console.log(to);
    // 如果是后退
    if (to.meta.deepth < from.meta.deepth) {
      to.meta.keepAlive = false;
    } else {
      to.meta.keepAlive = true;
    }
  }
},
```
```javascript
new Router({
  routes: [
    {
      path: '/',
      name: 'index',
      component: () => import('./views/keep-alive/index.vue'),
      meta: {
        deepth: 1, // 同级 不刷新
        keepAlive: true
      }
    },
    {
      path: '/list',
      name: 'list',
      component: () => import('./views/keep-alive/list.vue'),
      meta: {
        deepth: 1 // 同级不刷新
        keepAlive: true
      }
    },
    {
      path: '/detail',
      name: 'detail',
      component: () => import('./views/keep-alive/detail.vue'),
      meta: {
        deepth: 0, // 进入list不刷新页面
        keepAlive: false
      }
    },
    {
      path: '/edit',
      name: 'edit',
      component: () => import('./views/keep-alive/edit.vue'),
      meta: {
        deepth: 2, // 进入list刷新页面
        keepAlive: false
      }
    }
  ]
})
```
