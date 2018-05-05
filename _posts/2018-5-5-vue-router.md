---
layout: post
title:  "vue-router(2.2)"
categories: JavaScript
tags: JavaScript Vue
autor: rock
---

本文讲解vue中的路由管理，vue-router的使用




##  二、vue-router路由管理

> **前端路由**：在web开发中，路由是指**根据url分配到对应的页面**(也就是说一个url对应一个页面)
>
> **vue-router**: 通过管理url，实现url和组件的对应和通过url进行组件的切换。可以更方便的帮助我们构建单页应用
>
> **单页应用(spa)**: 加载单个html页面，并在用户与应用程序交互时动态更新该页面

#### 1、安装vue-router

> **安装vue-router模块**：``npm install vue-router --save``。如果是用的vue-cli脚手架，那他已经帮我们安装好了
>
> **引入模块**：``import VueRouter from 'vue-router'``
>
> **作为插件使用**：``Vue.use(VueRouter)``
>
> **定义容器**：``new VueRouter()``，即new一个实例,然后要把这个实例暴露出去
>
> **注入根实例**：``{router}``在main.js中引入刚刚暴露出来的实例，且把实例注入到根实例中去
>
> **实例**
```
// 在src目录新建一个router文件夹，router文件夹里面新建一个index.js文件
import Vue from 'vue'
import Router from 'vue-router'
Vue.use(Router)
export default new Router({})

// main.js
import router from './router'
new Vue({
  el: '#app',
  router // 注入到这里
})
```

#### 2、vue-router使用

>+ (1)**routes属性: 这个属性用来设置路由**。这个是一个**数组，数组的每一项都是一个对象**(即要配置的路由)。**对象里面需要一个路径对应一个组件(即一个path对应一个component)，当前重定向也是在这里面设置**
>   +  **path路径**：每一个路径对应一个组件，且**path对应的路径要在hash后面加才有效果**
>+ (2)**mode属性: 这个属性用来设置vue-router用哪种模式**。默认为hash模式，也可以设置为``mode: 'histoty'``
>+ (3)**linkActiveClass属性：这个属性用来设置全局的，<router-link>组件被激活时候新增的那个class的名字**。设置了之后，就可以在全局使用这个class名字来控制，默认值为router-link-active
>+ (4)**<router-view>组件**：这个是用来引入路由的组件。告诉路由渲染的位置，直接在页面使用这个标签就好了。**可以用name属性为组件指定名字**
>+ (5)**<router-link>组件**：这个是router中的点击组件
```
// router/index.js
import HelloWorld from '@/components/HelloWorld'
import about from '@/components/about'
export default new Router({
  mode: 'hash', // 默认值，可以不写
  linkActiveClass：'router-link-active', // 默认值
  routes: [ // 配置路由
    {name: 'home',path: '/',component: HelloWorld},
     {path: '/about',component: about}
  ]
})
// app.vue
<router-view></router-view> // 使用路由
// 浏览器url，假设浏览器url现在为localhost:8080/#,在它后面加个/about，
// url变成了localhost:8080/#/about，<router-view>对应的就变为了about的那个组件
```
#### 3、动态路径和query查询字符串

> **<1>动态路径**
>
> 有时候**路由的路径需要动态去生成**，这时候就需要用动态路由参数了。
>
>+ (1)**用动态参数的时候**: 这时候router中的path就不能用固定值了
>+ (2)**$router实例对象**：和vuex一样，在main.js注册之后，**每个组件中都可以通过$router来拿到实例对象**
>+ (3)**$route属性**：这个是每个组件实例都有的属性，是**当前激活的路由的信息对象**。如：用this.$route来访问。它的上面的属性如下：
>   + <1>this.$route.path字符串：对应当前路由的路径
>   + <2>**this.$route.params对象**：包含动态路由参数.**对象的键值就是router中设置的那个**。
>   + <3>**this.$route.query对象：获取url查询字符串中的参数**
>   + <4>this.$route.hash字符串：当前路由的hash值
```
// 这里这个:userID?就为动态匹配，最后的问号是正则的用法，表示可以匹配也可以没匹配到
// userID为设置的键值，可以在this.$route.params中访问，如this.$route.params.userID
{path: '/user/:userID?', 
  component: HelloWorld}

// 下面这个目标地址是用的动态拼接起来的，arr为存信息的数组
<router-link :to="'/hello/'+item.id" key='index' v-for='(item,index) of arr'>{{item.id}}</router-link>
```

> **<2>query查询字符串**
>
> **查询字符串实际上就是url中的``?info=share``这种**。在router-link中设置不同的查询字符串，然后选中的时候根据路由上不同的path来匹配组件，**还可以通过路由信息对象来获取了之后传给后端**。他有两种设置方式：
>   + <1>直接在路径上写：``<router-link to='/user/1?info=share'></router-link>``
>   + <2>动态绑定：:to后面跟上一个对象,**把查询字符串写到这个对象的query属性里面去，query属性里面可以同时写上多个查询字符串，写多个的时候在url上显示的就是用&连接的形式**
```
// 要加上精确匹配，不然由于会两个都匹配到
<router-link exact to='/user/1?info=share'></router-link> // 这个是写死的
<router-link exact to='?info=follow'></router-link> // 不要前面的那些path这个query还是会自动加到url的最后去
// path里面跟一个空字符串，代表相对于当前路径；
<router-link exact :to="{path:'',query:{infor:'follow'}}"></router-link>
// url为/user/1?info=share&a=1
<router-link exact :to="{path:'',query:{infor:'share',a:'1'}}"></router-link>
<p>{{$route.query}}</p> // 获取查询字符串
```

#### 4、重定向和别名

> 重定向实际上就是没找到页面或者说访问一个页面的时候，重定向到另一个地方去。这些**大都在Router构造函数的routes属性中的路由对象中设置**。
>
>+ (1)**name属性**: 表示当前路由的名字，设置了之后可以方便在后面引入这个路由
>+ (2)**redirect属性**：用来设置重定向。属性的值可以是字符串、对象，也可以是一个函数。**当是函数的时候，用来动态设置重定向的目标，接收一个参数to(即目标路由对象，就是访问路由的信息，to.path就是需要重定向的那个path)，且需要返回一个值作为重定向的路径**。
>+ (3)**alias属性**：用来**给路由的路径名字指定一个别名**，通过这个别名访问的时候，路由会匹配到设置这个别名的组件，但是path上面还是这个别名，不会变为这个路由的真实path
```
{name: 'home',path: '/',component: HelloWorld, alias: '/index'}, // url设置为index也能匹配到这个路由
{
    path: '*', // 当前面的所有路径都没有匹配到的时候，用下面的重定向
    // 下面的4个都是设置重定向，只用选一种方式就好了
    redirect: '/home',
    redirect: {path: '/home'},
    redirect: {name: 'about'},
    redirect: (to) => { // 动态设置重定向的目标
        if (to.path === '/123') {
            return '/home';
        } else {
            return {name: 'about'};
        }
    }
}
```
#### 5、嵌套路由和路由元信息

> 嵌套路由其实就是**一个路由里面，包含着其他的子路由**。这些也**大都在Router构造函数的routes属性中的路由对象中设置**
>
>+ (1)**children属性: 这个属性为一个数组，里面的项为子路由的路由对象**。
>+ (2)**子路由的路径**: 子路由的路径有**两种**设置形式。(1)**加上‘/’和父路一样相对跟路径设置**，这样url上会把父路由的path覆盖,**如``localhost:8080/girl``.(2)相对于父路由设置，这样就不加上‘/’只写字符串**，这样路径中就会出现父路由加上子路由path,**如``localhost:8080/hello/girl``**
>+ (3)**设置默认的子路由**：有时候需要点开一个父路由的时候，默认出现一个选中的子路由，这时候以下面代码为例，**将children中的子路由对象的路径设置为‘’，并且把router-link中的路径设置为‘/hello’(和父路由一样的路径)，但是为了防止这个路径一直激活，还需要在router-link中设置exact属性**
>+ (4)**路由元信息meta**：在路由对象中设置，**可以在里面加一些自定义的属于这个路由的数据**，然后用$route.meta在组件中访问。
```
// router中的index.js
  routes: [
    {
      path: '/hello',
      component: hello,
      meta: {index: 1},
      children: [{ // 这里多了一个子路由
      	path: 'girl', // 这里是相对父路由来设置，设置为‘’，即为默认的路由
      	component: girl // girl组件为在src下面新建一个文件夹，然后创建在里面
      }]
    }
  ]
// hello.vue
<template>
    <div>
    <ul><router-link to='/hello/girl' tag='li'><a>{{$route.meta.index}}girl</a></router-link></ul> // 路径那里只写‘/hello’就为默认路由，并且添加上exact属性
    <router-view/> // 这里这个router-view为点击的时候，girl组件的渲染位置
    </div>
</template>
// App.vue
<template>
  <div id="app">
    <ul><router-link to="/hello" tag='li'><a>click it</a></router-link></ul>
    <router-view/>
  </div>
</template>
```
#### 6、命名视图

> 命名视图实际上就是给router-view起个名字，然后在路由的components里面配置一下就好了，这样**用来在一个路由里面同时使用多个router-view**
>
>+ (1)**以前的时候，一个路由对应一个组件，然后把这个组件渲染到对应于那个router-view；由于现在有多个router-view了，所以路由对象里面也该变成components**。
>+ (2)**路由对象的components属性**：这个属性为一个对象，里面有一个**default属性，代表将后面的组件加载到没有设置name的router-view；其余的键名为router-view的name，键值为组件名**。
```
// App.vue
<router-link to="/hello" tag='li'><a>click it</a></router-link>
<router-view name='slider'></router-view>
<router-view></router-view>

// router/index.js
new Router({
  routes: [{path: '/hello',
      components: {
          default：hello,
          slider: Slider // Slider为组件
      }}]
})
```

#### 7、hash和history模式

> hash模式一般是在低版本中使用，高版本一般都用history模式，因为<router-link>组件是可以兼容hash模式的，且history模式可以使用浏览器的前进后退按钮。
>
>+ **hash模式：hash就是**用vue-router的时候，默认在**url的最后加上的那个``#/``**。而**hash模式就是点击的时候切换hash而不是切换相对路径**，所以，a标签中就应该设置为``href='#/about'``而不是``href='/about'``(这种方法是切换相对路径)。另外，**首页的默认url为``http://localhost:8080/#/``,所以router中设置为``path:'/'``的组件，会直接替换掉<router-view>出现在首页**
```
// App.vue
<a href="/HelloWorld">click it</a> // 这个是切换相对路径，url会变成http://localhost:8080/HelloWorld#/，不会引入组件
<a href="#/HelloWorld">click it</a> // 这个是切换hash，url会变成http://localhost:8080/#/HelloWorld，会将下面的<router-view/>切换为HelloWorld组件
<router-view/>

// router/index.js
new Router({
  routes: [{path: '/HelloWorld',
      component: HelloWorld}]
})
```
>+ **history模式**：这个模式就没有``#/``了，而在a标签中设置也**可以设置为相对路径**了，即``href='/about'``。**但是，这样的话由于每次点击都要触发a的跳转，就不是单页应用了，解决办法是去掉a链接跳转的默认行为**。另外，**首页的默认url为``http://localhost:8080``**
>   + **<router-link>组件**：这个是vue中用来替换a标签的组件，在浏览器中默认被渲染成a标签的，**有个to属性相当于href**。但是，它**取消了a标签的默认行为，点击的时候就不会刷新页面了**。另外，**这个组件是可以兼容hash模式的，即当处于hash模式的时候，这个组件里面的地址是加在hash后面的**
```
// App.vue
<router-link to='/HelloWorld'>click it</router-link> // 点击之后url为http://localhost:8080/HelloWorld
<router-view/>

// router/index.js
new Router({
  mode: 'history',
  routes: [{path: '/HelloWorld',
      component: HelloWorld}]
})
```
#### 8、<router-link>组件

> 这个组件在浏览器中是找不到的，它会被替换为其他标签。**这个组件中不能用@click绑定事件处理函数**
>
>+ (1)**to属性**：这个属性**用来指定跳转的目标路由**，后面**可以直接跟path，也可以跟路由的name**
```
<router-link to='/HelloWorld' tag='p'>click it</router-link> // 生成为p标签
<router-link :to='{hello}' tag='p'><a>click it</a></router-link>
```
>+ (2)**tag属性**：这个属性**用来指定router-link组件在浏览器中生成的标签类型**，默认为a
```
<router-link to='/HelloWorld' tag='p'>click it</router-link> // 生成为p标签
// 如果要让这个p标签变的可以点击的话，就在这个里面再加个a标签,这样router-link中的地址就到a上面去了
<router-link to='/HelloWorld' tag='p'><a>click it</a></router-link>
```
>+ (3)**组件中可以加上其它标签**：在组件的文本部分加上其它的html标签，在浏览器中依然会存在
```
<router-link to='/HelloWorld' tag='li'><p>dwdsa</p></router-link> 
// 浏览器中渲染为
<li><p>dwdsa</p></li>
```
>+ (4)**组件激活新增的class：当组件被激活的时候，组件上面就会新增一个叫router-link-active的class**，这个class名字是可以修改的 
>   + 全局修改名字：在Router构造函数的linkActiveClass属性中修改
>   + 修改单一组件的名字：在组件上使用active-class属性，后加上需要修改成的class名字
```
<router-link to='/HelloWorld' active-class='my-active'></router-link> 
```
>+ (5)**event属性：这个属性用来修改组件的触发方式**，默认为click
```
<router-link to='/HelloWorld' event='mouseover'></router-link>  // 鼠标移入的时候触发
```
>+ (6)**exact属性：这个属性用来使路径精确匹配**，即，不用这个的话，如果有跳转根路径的路由，那么跳转到根路径之上的路由的时候，根路径的那个也会被默认激活。如：设置了‘/’在点击‘/about’的时候，‘/’也会被激活
```
<router-link to='/' exact></router-link>  // 精确匹配
```
#### 9、编程式导航

> 除了用view-link这种切换导航的方式之外，还有一种叫编程式导航。这种方法借助router的实例方法，通过编写代码来实现导航的切换
>
>+ **histoty栈**：当页面的url刷新的时候，这些url就会被加入到历史记录栈中。
>
>+ **编程式导航函数**：主要用的就是**back()(向后退一步)、forward()(向前一步)、go()(指定移动的步数)、push()(导航到一个url，并且把它压入栈中)、replace()(替换掉栈顶的那个记录)**，这几个函数。
```
<p @click='aFun'></p>
export default {
    methods: {
        aFun() {
            this.$router.back(); // 向后一步
            this.$router.push('/user'); //  跳转到user
        }
    }
}
```


#### 10、组件切换时候的过渡动画

> 由于在vue中直接是一个组件来替换router-view，所以用普通的css很难在组件切换的时候为它设置渐变。所以，**vue提供了一个transition组件，和一些用在它上面的class类名，用来实现组件切换时候的动画效果**。
>
>+ (1)**transition组件**：把要运动的元素(如router-view)放在这个transition里面，**当这个元素插入、移除、改变的时候就可以用这些过渡动画了，而控制过渡动画是通过控制一些类名(或者钩子函数)来实现的**。它的**类名默认是以v-开头的，但是可以改变，后面的是不能改变的**，如下：
>   + <1>**v-enter**: 定义组件进入过渡的开始状态。**不透明的为0，即opacity：0**
>   + <2>**v-enter-active**: 定义组件进入活动状态。**用css3的transition属性来设置运动的持续时间和运动形式**
>   + <3>**v-enter-to**: 定义组件进入的结束状态。**不透明的为1，即opacity：1**
>   + <4>**v-leave**: 定义组件离开过渡的开始状态。**不透明的为1，即opacity：1**
>   + <5>**v-leave-active**: 定义组件离开活动状态。**用css3的transition属性来设置运动的持续时间和运动形式**
>   + <6>**v-leave-to**: 定义组件离开的结束状态。**不透明的为0，即opacity：0**
>+ (2)**设置name属性改变类名：设置name之后，上面那6个相关类名的开头都由v-变为name-开头**。且transition组件就会使用改变之后的类名，而不是用默认的那个v-开头的类名了。name属性在transition中设置。**但是在transition中设置class是无效的**
>+ (3)**设置mode属性改变过渡模式**：过渡模式有两种，**第一种是in-out(新元素先进行过渡进来，完成后当前元素过渡离开)，第而种是out-in(当前元素过渡离开，完成后新元素先进行过渡进来)**。在transition元素的mode属性中设置。
>+ (4)**设置鼠标点击所在位置左边的导航，当前元素从右滑走，点击所在位置右边的导航，当前元素从左边滑走**：<1>给每个导航的路由设置一个meta元信息，代表所在导航的index，<2>然后在组件中用watch监控$route，获得to和from的index，然后比较，再根据比较的结果给transition动态绑定一个name(有两种name就要有两套class)
```
// APP.vue
<template><div id="app">
    <ul><router-link to="/hello" tag='li'><a>link to hello</a></router-link>
    <router-link to="/HelloWorld" tag='li'><a>link to HelloWord</a></router-link></ul>
    <transition name='my' mode='out-in'><router-view/></transition> // 用的是out-in模式。组件切换的时候，就会用到下面那些类
  </div></template>
  
<style>
.v-enter {opacity: 0;} 
.v-enter-to {opacity: 1;}
.v-enter-active {transition: 1s;}
.v-leave {opacity: 1;}
.v-leave-to {opacity: 0;}
.v-leave-active {transition: 2s;}
// 由于在transition元素上设置了name，改变了它的默认类名，则不会用上面的那些类名，而是用下面这些
// 下面几个直接就可以实现滑动效果
.my-enter {transform: translateX(100%);}
.my-enter-to {transform: translateX(0);}
.my-enter-active {transition: 1s;}
.my-leave {transform: translateX(0);}
.my-leave-to {transform: translateX(-100%);}
.my-leave-active {transition: 1s;}
</style>
```

#### 11、导航钩子函数

> 导航钩子函数和普通的钩子函数还是有很大差别的。**导航钩子函数主要是在导航发生变化的时候，用来拦截导航，让它完成跳转或者取消**。所以可以用钩子函数来判断是否进行了登录
>
>+ (1)**钩子函数的执行位置和种类**：这个执行位置就是钩子函数可以被调用的位置
>   + **router实例上(全局)设置**: beforeEach(**在进入所有导航之前调用**)、afterEach(**在进入所有导航之后调用**)
>   + **单个路由上设置**: beforeEnter(**在进入被设置的导航之前调用**)
>   + **组件中**: beforeRouteEnter、beforeRouteUpdate(**比如，点击这个导航里面的二级导航，他就会被调用**)、beforeRouteLeave
>+ (2)**钩子函数的参数：to、from、next**。to代表要进入的目标路由对象；from代表要离开的路由对象；**next为一个函数，决定跳转或者取消导航，不调用这个函数则取消跳转，它的参数为跳转的地址，不加参数则跳转到本来要跳转的位置**。
>+ **注1**：全局钩子是接受一个函数作为参数的，而这个函数的参数是to、from、next；两个局部钩子是直接接受那3个参数的。**且在钩子函数里面是访问不到this的(因为它在有this之前执行的)，所以如果要访问根实例上的插件用``router.app.$插件名字``(这个只有全局的时候才能用)**
```
// router文件夹中index.js
export default new Router({
    routes: [{path: '/HelloWorld',
    	meta: {login: false，title: hello},
    	beforeEnter(to,from,next){}, // 在路由中设置的，局部的钩子函数 
    	component: HelloWorld}]})
// 下面两个都是全局的钩子函数
router.beforeEach((to,from,next) => {if (!to.meta.login) {next('/login');} else {next();}})
router.afterEach((to,from) => {window.document.title = to.meta.title;})// 这个就不需要控制next了，因为它已经进入导航了
// hello.vue 下面设置的是组件级的钩子函数
export default {
    data() {return {msg: this.$store.state.msg};},
    beforeRouteEnter(to,from,next){},
    beforeRouteUpdate(to,from,next){}} // 组件级，不设置next的话，依然是不能进入组件的
```