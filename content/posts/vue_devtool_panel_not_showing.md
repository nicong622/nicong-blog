---
title: Vue Devtool Panel not Showing
date: 2020-10-07
---

Chrome 的 vue devtool 插件已经检测到当前页面使用了 Vue ，但是 Chrome 开发者工具里没有出现 Vue devtool panel，要怎么处理？

## 第一步

首先检查一下项目里是否手动设置了 `Vue.config.devtools` 。把这个配置设为 `true` 的时候就能看到 Vue devtool panel 。这个配置需要在创建 Vue 实例之前。根据 [Vue官方文档](https://vuejs.org/v2/api/#devtools) 的说法，`Vue.config.devtools` 在开发时候默认是 `true` 。

设置完之后重新构建一次，然后重新打开 Chrome devtool 或者修改 Chrome devtool 的主题，就能看到 Vue devtool。

多数情况下这一步就能解决问题。

## 第二步

如果上一步还是不能解决问题，再试试在创建了 Vue 实例之后执行 `window.__VUE_DEVTOOLS_GLOBAL_HOOK__.Vue = app.constructor;` 其中 app 是 Vue 的实例。

或者直接在 Chrome devtool 里执行 `window.__VUE_DEVTOOLS_GLOBAL_HOOK__.Vue = Vue;`。

## 参考

https://github.com/vuejs/vue-devtools/issues/620#issuecomment-368948291