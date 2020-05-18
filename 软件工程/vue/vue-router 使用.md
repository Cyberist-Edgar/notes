### vue-router 使用

先安装

```bash
npm install vue-router
```

然后调用

```js
import Vue from 'vue'
import Router from 'vue-router'
Vue.use(Router);
```

引入需要显示的组件，然后设置`path`

```js
import Home from "xxx"
export default new Router({
routes:[
    {
      path: '/index',
      name: 'index',
      component: Home
    },
]
```

为了使用这个路由，在模板中需要使用`<router-view>`,可以放在任何需要显示的地方

```vue
<template>
    <div id="app">
        <router-view></router-view>
    </div>
</template>

<script>
    export default {
        name: 'App',
    }
</script>

```



`history`模式

默认情况下，vue-router使用的是URL hash 来储存路径，如果要访问 /index 页面，需要使用 http://xxx.com/#index的方法， 如果要使用history模式，只需要：

```js
import Home from "xxx"
export default new Router({
mode: 'history',  // 修改为history模式，默认为hash
routes:[
    {
      path: '/index',
      name: 'index',
      component: Home
    },
]
```



### 动态路由

如果想要让组件按照某一定的匹配来进行访问，可以如下指定路由

```js
import User from "xxx"
export default new Router({
mode: 'history',  // 修改为history模式，默认为hash
routes:[
    {
      path: '/user/:id',
      name: 'user',
      component: User
    },
]
```

这样的话，任何匹配`/user/:id`的路径都可以访问到对应的信息

如果我们想要获取到对应的`id`，可以使用：

```js
this.$route.params  // 默认情况下获取到的数据都是字符串
// {
// id: "1234"
// }
// 直接获取id
this.$route.params.id
```

路由的参数可以作为一个组件的属性进行传送，指定`props:true`即可

```js
export default new Router({
mode: 'history',  // 修改为history模式，默认为hash
routes:[
    {
      path: '/user/:id',
      name: 'user',
      component: User
      props:true
    },
]

const User= {
    props: ['id'],
    template: `<p>{{id}}</p>`
}
```



### 嵌套路由

嵌套路由允许使用另一个`<router-view/>`来显示子路由下的内容

比如说一个个人中心的网站，侧边栏和导航都是一样的，只用内容不同，我们就可以在内容那里使用router-view来显示子路由下的组件内容

```js
{
      path: '/user/profile/',
      name: 'profile',
      component: Profile, // 该组件中某个位置含有 router-view
      children:[
        {
          path: 'files',  // 不要加  / 否则被认为是根目录
          name: 'files',
          component: Files  // 该组件将在router-view中显示
        },
        {
          path: 'log',
          name: 'log',
          component: LoginLog  // 一样在其中显示
        }
      ]
    },
```



#### 重定向

对用户访问的网址重定向到另外一个网址

```js
export default new Router({
  mode: 'history',
  routes: [{
      path: "/",
      redirect: '/index'  // 比如重定向到index
    },]
 // 也可以对path取个别名，这样两个网址都可以访问同样的内容
  export default new Router({
  mode: 'history',
  routes: [{
      path: "/",
      alias: '/index'
      component: Home
    },]  
```



### 链接导航

使用`<a>`可以实现，但是使用`<router-link>`更好

```vue
<router-link to="/user/1234">xxxx</router-link>
```

当使用该链接的时候，不需要重新加载一个页面，可以大幅度提升网站性能，因为这时候大部分静态内容在之前使用过的可以接着使用

#### 常用属性

- `tag`属性

  默认情况下router-link会渲染成a标签，使用tag标签可以指定为想要的标签类型

  ```vue
  <router-link to="/user/1234" tag='li'>xxxx</router-link>
  // 会被渲染成
  <li>xxxx</li>
  // 当时上述的方式并不是最优的方式，因为我们不能够明确知道其是一个链接
  // 我们可以使用
  <router-link to="/user/1234" tag='li'><a>xxxx</a></router-link>
  // 这样渲染的为：
  <li><a href='/user/1234'>xxx</a></li>
  ```

- `active-class`属性

  当router-link组件中的to属性中的路径和当前的路径相匹配的时候链接就被激活了，active-class就会变成class

  

原生事件监听

如果想要给router-link添加一个事件监听器，可以使用@click之类的事件，但是需要添加`.native`修饰

```vue
<!--这样无法实现-->
<router-link to="/user/1234" @click="handleClick">xxxx</router-link>
<!--这样可以-->
<router-link to="/user/1234" @click.native="handleClick">xxxx</router-link>

<!--也可以使用v-on来监听-->
<router-link to="/user/1234" v-on:click="handleClick">xxxx</router-link>
激发需要通过 this.$emit("click"[, params])
```



#### 编程式导航

使用`router.push()` 可以跳转指定的路由，同样使用`router.replace()`也可以实现，但是如果后退的话不会返回之前的网页

`router.go(num)`可以回退或者前进网页记录，负数为回退，如果数目超过总的，则不会调用



#### 导航守卫

如果想要限制未登录的用户，禁止这些用户访问网站的某一部分，可以为路由器添加一个`router.beforeEach(to, from, next)`守卫，to -> 去哪里    from-> 从哪里来  next 回调

```js
router.beforeEach((to, from, next)=>{
if(to.path.startsWith("/account") && !userAuthenticated()){ // 如果用户访问某些特定网页没有登录的时候
next('/login')  // 去登陆
}
else{
next() // 可以正确访问, 确保要调用
}
})
```



如果用大量的网站路由，一个一个检查太冗余，可以在每个路由中设置`meta`元信息，然后在守卫那里查看该属性即可

```js
export default new Router({
  mode: 'history',
  routes: [{
      path: "/user/1234",
      component: User,
      meta: {
          requireAuth: true  // 属性名可以自定义
      }  
    },]
router.beforeEach((to, from, next)=>{
if(to.meta.requireAuth && !userAuthenticated()){ // 如果用户访问某些特定网页没有登录的时候
next('/login')  // 去登陆
}
else{
next() // 可以正确访问
}
})
```

当使用嵌套路由的时候，`to.meta指向的是子路由的元信息，而非父路由`，也就是说，如果在/account上添加meta信息，如果用户访问的是/account/profile，那么获得的meta对象是关于该子路由的

这个时候可以使用 `to.matched`方法来进行处理

```js
router.beforeEach((to, from, next)=>{
const requireAuth = to.matched.some((record)=>{return record.meta.requireAuth}) // 获取相关路由中的meta信息，如果有一个true则返回true

if(requireAuth && !userAuthenticated()){ // 如果用户访问某些特定网页没有登录的时候
next('/login')  // 去登陆
}
else{
next() // 可以正确访问
}
})
```

#### 路由独享的守卫

在路由配置上直接定义 `beforeEnter` 守卫：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

与全局前置守卫的方法参数是一样的。



#### 组件内的守卫

可以在路由组件内直接定义以下路由导航守卫：

```js
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```



#### 路由顺序

路由在匹配的时候选取匹配的`第一个url`，其余的不会





#### 404界面

可以利用路由搜索的顺序来渲染一个显示错误页面

```js
const router = new VueRouter({
  routes: [
    //  其他的路由
      {
          path: "*",
          component: PageNotFound
      }
  ]
})
```

在子路由中也可以使用



#### 路由命名

可以对每一个路由进行命名，然后可以使用名称来进行链接

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      name: "foo"
      }
    }
  ]
})

<router-link to="{name: 'foo'}"></router-link>
// <=> <router-link to="{path: '/foo'}"></router-link>
// <=> <router-link to="/foo"></router-link>

// 在push中也可以使用
router.push({"name": "foo"})


// 可以传入参数
<router-link to="{name: 'foo', params: {id: 2}}"></router-link> 
//相当于  /foo/2
```

