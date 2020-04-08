

创建Vue的时候创建一个实列，其中的options包括

- `el `

  - string | HTMLElement

  - 决定之后Vue管理的DOM

  - ```vue
    el: "#app"
    el: document.getElementById("app")
    ```

- `data `

  - object | function

  - Vue实例对应的数据对象

  - ```vue
    data: {
    counter: 0
    }
    ```

- `methods`

  - {[key: string ]:function}

  - 定义属于Vue的一些方法，可以放在其他地方调用

  - ```vue
    methods:{
    add: function(){counter++}
    },
    add() {counter++}  一样的
    
    ```

- `computed`    计算属性， methods也可以实现，但是每次调用methods都需要重新执行，computed只需要一次(值修改都需要重新计算)

  - 对属性进行处理

  - ```
    data: {
    firstName: "Edgar",
    lastName: "Cyberist"
    }
    computed:{// 不用取动词的短语
    name: function(){
    return this.firstName+" "+lastName
    }
    }
    `调用的时候直接使用name，不需要加小括号`
    ```
    
  - 计算属性都含有`set`和 `get`方法, 计算属性一般都没有set方法,为只读方法
  
  - ```
    name: {
    set: function(value){}  // value为设置的value
    set(value){} // 与上一样
    get: function(){return this.message}
    }
    ```
  
  - 可以在控制台使用app.name= 'value' 进行测试，会调用set方法
  
- `filters` 过滤器, 类似django

  - ```js
    filters:{
        add_2(num){
            return num+2
        }
    }
    ```

  - 使用方式

    ```js
    {{num| add_2}}
    ```

- `components` 组件

  - ```
    cpnC = Vue.extentd({
    template: `<div><标题/div>`
    })
    局部组件注册
    components:{
     cpn: cpnC
    }
    
    全局组件注册
    Vue.component("cpn", cpnC)  // 注册全局， cpn 为使用的对象 
    <cpn></cpn>
    ```

  - 







`v-for`  ： 迭代遍历

`官方推荐使用的时候加一个:key`, 这样可以更高效的更新DOM

```vue
 <ul>
        <li v-for="movie in movies">{{ movie }}</li>
    </ul
     <v-for="(movie, index) in movies">  index 为索引从0开始
```
```js
const app = new Vue({
        el: "#app",
        data: {
            movies:["1", "2", "3"],
            label: "movies"
        }
    })
```

如果遍历 对象

```html
直接遍历的时候为value
<div v-for="item in items">
    {{item}}
</div>

<div v-for="(value, key) in items">  // 前面的值为value
    {{key}}:{{value}}
</div>

<div v-for="(value, key, index) in items">  // 还可以获取index
    {{key}}:{{value}}  {{index}}
</div>

data:
{
item:{name: "Edgar", age:20}
}
```







`v-once`: 显示的值为第一次显示的值，不会变化

```html
<div>
    {{message }}
</div>
<div v-once>{{message}}</div>
// 在调试台修改以app.message="anther value"修改message值的时候
// 使用了v-once的不会变，但是第一个会变
```



`v-html`

```html
 <div>{{url}}</div>  // 显示全部的字符串
 <div v-html="url"></div>  // 显示标签形式
```



`v-text`

```html
<div>
    {{message}}
</div>

<div v-text='message'></div>  
<!--上面的两种方式等价，但是没有第一种灵活，并且会覆盖掉标签中的原有文本-->
```



`v-pre`

```html
<div v-pre>
   {{message}}
</div>
<!--显示就是 {{message}}，不作解析-->
```



`v-cloak`

```html
<div v-cloak>
    
</div>
<!--主要是为了避免js执行的时候出现问题，直接显示{{message}}-->
在vue解析之前div中有一个v-cloak属性
在vue解析之后div中v-cloak属性消失
其实在实际开发中使用不多，因为我们使用过的模板都是被渲染过的
```

```css
[v-cloak]{
    display: none;
}
```



`v-bind`

可以简化成 `:event_name`   

```html
<!--绑定-->
首先我们不能这样使用,会直接显示imgUrl
<img src="{{imgUrl}}">

<img v-bind:src='imgUrl'>
语法糖写法 <img :src="imgUrl">
其他的属性 class, href 都可以这样使用

动态绑定对象
<!--active 为class属性名， line为属性名-->
<div class="test" :class={active: isActive, line: isLine}>  <!--test为固定的class，一定会有，vue会进行合并-->
    {{message}}
</div>

对象绑定
<div :style="{key:value}""></div>
函数绑定
 <div :style="getStyle()"></div>      
<div :style="{fontSize: size+'px'}"></div>
<div :style="[style]"></div>
```
 这里的key是css样式的属性名 backgroundColor 
 `key不会解释为变量，但是value会`
可以用驼峰命名法，也可以用` - `进行分割

backgroundColor background-color

```
<script>
 const app = new Vue({
        el: "#app",
        data: {
    imgUrl: "https://vuejs.org/images/logo.png",
            isActive: true,
            isLine: true,
            size: 100,
            style:{fontSize:'100px'}
        },
        methods: {
        getStyle: function(){
        return {fontSize: this.size+'px'}
        }
        }
    })
</script>
```



`v-on` 事件监视， `可以省略v-on 使用@`

```html
<button @click="click()"></button>" 没有参数可以使用下面两种方式
<button @click="click"></button>"

有参数使用小括号，然后加入参数 
<button @click="click(2)"></button>

如果有参数，但是没有传入参数，默认vue会将浏览器的事件对象event传入

方法定义的时候需要event对象，同时也需要其他参数
<button @click="click(123, $event)"></button> 
// 默认的时候如果参数是没有引号包括的字符串(符合变量命名规则的)，会被识别成data里面的元素  ！！！！！！！！
click(param, event){
console.log(param, event)
}

事件冒泡, 点击btn，那么div中的click也会被执行
<div @click="divClick">
    <button @click="btnClick"> </button>
</div>
避免事件冒泡，使用修饰符 stop
<div @click="divClick">
    div html
    <button @click.stop="btnClick"> </button>
</div>

<form action="." >
    <input type="submit" @click.prevent="submit">
</form>

默认的话使用input submit 之后会默认的将所有的form中信息收集然后传到服务器，
使用prevent可以自定义上传的内容(可能需要使用ajax等)


监听键盘的输入
松开按键
<input type="text" @keyup="keyUp">
Enter 键
<input type="text" @keyup.enter="enter">

自定义组件需要使用.native 修饰符，否则无法监听到事件
<my-label @click.native></my-label>  

<input type="text" @click.once=""> 只监听第一次

其他事件都可以使用这样的修饰符
```

修饰符

.stop

.prevent

.native

.once

@keyup.键名

@keydown.键名





### 判断

`v-if v-else  v-else-if`

```html
 <div>
        <div v-if="score>=90">优秀</div>
        <div v-else-if="score>=80">良好</div>
        <div v-else>一般</div>
    </div>
条件比较多的时候可以在computed属性里面进行计算

<div v-if='show'>
    <label>username</label>
    <input type='text' id='username'>
</div>
<div v-else>
    <label>password</label>
    <input type='password' id='password'>
</div>
<button @click="show=!show"></button>

如果直接这样的话，由于vue底层的处理方式(元素复用)，
第一个input中输入值之后，切换到第二个input的时候，中间的值为原来的值

如何避免：定义一个key属性，不同的话会重新创建
<div v-if='show'>
    <label>username</label>
    <input type='text' id='username' :key='username'>
</div>
<div v-else>
    <label>password</label>
    <input type='password' id='password' :key='password'>
</div>
<button @click="show=!show"></button>
```



`v-show` 显示

```html
<div v-show="isShow">显示</div>
<script>
    const app = new Vue({
        el: "#app",
        data: {
    isShow: false,
        }
    })
</script>
```

`如果判断语句为false的时候，元素会display:none`

`v-if`则完全不在dom中

如果切换频率比较高的时候可以使用v-show

如果只有一次使用的话，使用v-if比较好





`v-model`

- 双向绑定表单中的数据, 绑定的元素数据改变，对应的数据也变，对应的数据变，表单也变

- ```html
  <input type='text' v-model='message'>
  ```

- 修饰符

  ```html
  .lazy  当前元素失去焦点或回车等才进行实时绑定
  <input type='text' v-model.lazy='message'>
  .number  默认绑定之后数据均为字符串
  <input type='text' v-model.number='num'>
  {{typeof num}}
  .trim 消除两端的空字符串
  ```

  



并不是对data中数据的改变都是响应式的，常见的可以响应的如下：

`push    `

`pop   `

`shift` // 删除数组中的第一个元素

`unshift ` // 在数组中的最前面加元素

`splice`  // 从起始位置开始删除数据/插入/替换

第一个元素表示位置

删除元素---> splice 第二个参数表示删除的元素个数

添加元素 ---> 第二个参数取0即可，后面的元素为需要插入的元素

替换元素 --> 第二个元素表示需要替换的元素个数，后面的元素为替换的元素

`sort`

`remove`



索引进行修改的是不会重新渲染的

通过索引值进行修改可以使用：

```js
第一个参数表示要修改的对象，第二个表示修改的索引值，第三个表示替换的值
Vue.set(this.movies, 0, '海王')
```



{{ typeof message}} -> 获取类型













## 组件化

创建组件构造器，注册组件，使用组件, 可以重复使用

```html
<div id="app">
 <cpn></cpn>
 <cpn></cpn>

</div>
    <script src="vue.js"></script>

<script>
    const cpnC = Vue.extend({
        template:
        `<div>你好</div>`
    });

    Vue.component("cpn", cpnC);
</script>
```

在Vue2.x中基本上不使用extend，而使用语法糖(简写模式)

`默认注册的组件为全局组件，可以在所有的Vue实例下使用`

直接在Vue实例中`components`属性定义，为局部组件

父组件，子组件，每个组件都需要自己注册才能单独使用，但是父组件中包含子组件，子组件不注册也能在父组件中渲染

```js
const childComponent = Vue.extend({
    template:
    `<div>This is the child</div>`
})
const parentComponent = Vue.extend({
    template:
    `<div>This is the parent</div>
	<child></child>`, // 使用child组件
    components:{
        child: childComponent
    }
})
 const app = new Vue({
        el: "#app",
     components:{
         parent: parentComponent
     }
    })
 // app里面只能使用<parent></parent>
 // 不能用<child></child>
```

语法糖使用组件

```vue
// 全局组件
Vue.component("cpn", {
template:`<div></div>`
})
// 局部组件
    let app = new Vue({
        el: "#app",
        components:
            {
                "cpn1":{
                    template:`<div></div>`
                }
    }
    })
```

组件的抽离

```html
<template id='cpn'>
<div>
    <h2>Hello</h2>
    <p>Test</p>
</div>
</template>
<script>
Vue.component("cpn",{
    template: "#cpn" // 取上面的id
})
</script>
或者
<script>
let app = new Vue({
el: "#app",
components:
    {"cpn1":
      template: "#cpn"
    }
})
</script>
```



```html
<template id='cpn'>
<div>
    <h2>{{counter}}</h2>  // 组件的这种东西不能用new Vue中的data，只能为组件里的data
<button @click="increase">+</button>
    </div>
</template>
<script>
    Vue.component("cpn", {
        template: "#cpn",
        // 这里的data必须是一个method
        data() {
            return {
                counter: 0
            }
        },
        methods: {
            increase() {
                counter++
            }
        }
    })
 </script>
```





### 父子组件的通信

父传子：props 写在子组件里面

子传父：事件

```html

<div id="app">
    <cpn v-bind:message="parentMessage"></cpn>   <!--v-bind-->
</div>
<script src="vue.js"></script>

<script>

    const app = new Vue({
        el: "#app",
        data: {
            parentMessage: "父组件子组件信息" 
        },
        components: {
            "cpn": {
                template: `<div>{{message}}</div>`,
                //props: ["message"],  // 父传给子的数据，可以自定义名称
                 props:{
                    message: String ,  // 可以限制类型
                    array: Array,  
                    // 还可以提供默认值
                    test:{
                        type: String,
                        default: "Hello"
                    }
                }
            }
        }
    })
</script>

```

更加多的选项

```js
  const cpn2 = {
        template: `<div>{{message}}<ul><li v-for="a in array">{{a}}</li></ul>{{test}}</div>`,
        props: {
            message: {
                type: String,
                default: "无"
            },
            array: {
                type: Array,
                default: function () {
                    return []
                }
            },
            test: {
                type: [String, Number], // 支持多个类型
                default: "test"  // 默认值
                required: true  // 一定要提供该属性
            }
        }
    };
const app = new Vue({
        el: "#app",
        data: {
            parentMessage: "父组件子组件信息",
            array: [1, 2, 3],
            test: ""
        },
        components: {
            cpn2  // 属性增加写法，会将其转化成cpn2: value 的形式
        }
    })
```

子传父

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>子通信与父</title>
</head>
<body>

<div id="app">
<cpn @item-click="cpnClick"></cpn> // 不能使用驼峰命名法, 这里会将子组件传递的参数给cpnClick，而不是event

</div>

<template id="cpn">
    <div>
        <ul>
            <li v-for="item in categories" @click="btnClick(item)">{{item.name}}</li>
        </ul>

    </div>
</template>

    <script src="vue.js"></script>

<script>
    const cpn = {
        template: '#cpn',
        data() {
            return {
                categories: [
                    {id: 1, name: "第1个"},
                    {id: 2, name: "第2个"},
                    {id: 3, name: "第3个"},
                    {id: 4, name: "第4个"},
                ]
            }
        },
        methods: {
            btnClick(item){
                console.log(item);
                this.$emit("item-click", item)  // 发送给父组件 事件名称，参数
            }
        }
    };

    const app = new Vue({
        el: "#app",
        data: {
        },
        components:{
            cpn
        },
        methods: {
            cpnClick(item){
                console.log(item)
            }
        }
    })
</script>

</body>
</html>
```



