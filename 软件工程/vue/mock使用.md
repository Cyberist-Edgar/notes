### mock使用

[官网]( http://mockjs.com/ )

```
cnpm install axios mockjs json5 --save  # json5 解决json中无法加注释的问题
```



创建一个mock文件夹，

index.js处理拦截的信息

```js
const fs = require('fs')
const path = require("path")
const JSON5 = require("json5")
const Mock = require('mockjs')


function getJsonFile(filePath) {
    let json = fs.readFileSync(path.resolve(__dirname, filePath), 'utf-8')
    return JSON5.parse(json)
}

module.exports = function (app) {
        app.get("/user/userinfo", function (rep, res) {
            let json = getJsonFile('./userInfo.json5');
            res.json(Mock.mock(json));
        })
}

```



.json5可以保存各种信息文件, 可以使用mock中的句法，需要json5模块



然后在`vue.config.js`中处理拦截

```js
 devServer:{
        before: require('./mock/index.js')  // 拦截请求
    },
```





测试的时候，上面的方法似乎不太稳定，可以使用下面的方法

首先，创建一个拦截的js文件，中间的内容如下：

```js
const Mock = require('mockjs')


Mock.mock("/user/userinfo/", 'get', {
    "data|10":[
        {
            id: "@increment(1)",
            name: "@cname()"
        }
    ]
})

Mock.mock("/user/login/", "post", function(options){  // 用户 提交的内容在body里面
    console.log(options)
    return Mock.mock({
        "data|10":[
            {
                id: '@id()',
                name: '@cname()'
            }
        ]

    })
})
```

然后在`src/main.js`中引入该文件即可

```js
import Vue from 'vue'
import App from './App.vue'
import router from "./router/index.js"
import axios from 'axios'
import '../mock/' // 如果是index可以省略具体的文件名
import store from './vuex/store'
Vue.prototype.$axios = axios;

Vue.use(Andt);

Vue.config.productionTip = true;

new Vue({
  store,
  router,
  render: h => h(App),

}).$mount('#app')

```

