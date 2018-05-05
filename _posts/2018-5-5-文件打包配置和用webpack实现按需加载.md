---
layout: post
title:  "文件打包配置和用webpack实现按需加载(2.5)"
categories: JavaScript
tags: JavaScript Vue
autor: rock
---

本文讲解文件打包配置和用webpack实现按需加载




## 一、实现按需加载

> 由于vue做单页的话他是有很多模块的，而**如果在进入首页的时候就加载全部模块，那么就特别的浪费性能，所以需要按需加载模块**。
>
>+ **按需加载实现**：把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件
>   + (1)vue异步组件。这个一般不单独使用，而是配合webpack方法使用
>   + (2)webpack代码分割功能，这个实际上是和vue异步组件结合的使用方法
```
// (1)vue异步组件
{components: {custom:(resolve,reject) => {}} }
// (2)使用webpack代码分割功能
require.ensure代码分块 // ensure的第三个参数是这个异步块的名字，名字相同的话，就会同时被调用
             require.ensure(依赖，回调函数，[chunk名字])
import函数，现在还在指定阶段，可能会替换掉require.ensure
```
> 异步组件实现：用es6的promise实现
```
// hello.vue
export default {
    components: {
    // 以前的话是在组件外面import导入组件，然后直接在这里使用，这里是用require导入的
        helloWord: (resolve) => { // 这里用的promise异步，不过它没有new promise，也没有执行promise实例的then
            setTimeout(() => {
                resolve(require('@/component/helloWord'));
            },2000);
        }
    }
}
```
> webpack代码分割功能实现：以前引入模块是用import直接导入，然后直接放在component上的；**现在就不用import直接导入了，而是放在promise里面return出来**
```
// router中的index.js
let hello = (resolve) => {
    return import('@/component/helloWord');
}
/*let hello = (resolve) => {
    return require.ensure([],() => {
        resolve(require('@/component/helloWord'));
    });
}*/
export default new Router({
    routes: [{
    	path: '/hello',
    	component: hello}]
})
```

## 二、apache配置

> package.json文件
```
// scripts属性
     + dev 这个就是开发环境启动
     + build 这个就是把文件打包，打包成一个可上线的文件，成功后会出现一个dist文件夹
```
> config中的index.js文件
```
// build属性
     + assetsRoot 这个是在根路径下生成dist文件夹
     + assetsSubDirectory 这个是生成dist文件夹里面的静态目录
     + assetsPublicPath 这个就是设置服务器上前端文件的相对根路径
```