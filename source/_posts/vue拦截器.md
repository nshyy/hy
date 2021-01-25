---
title: Vue拦截器
date: '2021/1/25 11:00:00'
categories:
  - Alipay
tags:
  - vue拦截器

---

# vue如何实现拦截器
首先在index.js里面需要拦截器的路由进行配置
```
{
  path: '/index',
  name: 'index',
  component: index,
  meta: {requireAuth: true} // 配置此条，进入页面前判断是否需要登录
}
```
配置完路由，在main.js添加，用vue-router提供的钩子函数beforeEach()对路由进行判断。
```
router.beforeEach((to, from, next) => {
  console.log(to);
  console.log(from);
  if (to.meta.requireAuth) { // 判断该路由是否需要登录权限
    if(sessionStorage.getItem('token')){ //判断本地是否存在token
      next();
    }else {
　　　　　　next({
　　　　　　　　path: 'login',
　　　　　　　　query: {redirect: to.fullPath } // 将跳转的路由path作为参数，登录成功在跳转到该路由
　　　　　　})

    }
  }
  else {
    next();
  }

});
```