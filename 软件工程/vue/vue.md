创建 Vue 的时候创建一个实列，其中的 options 包括

- `el`

  - string | HTMLElement

  - 决定之后 Vue 管理的 DOM

  - ```vue
    el: "#app" el: document.getElementById("app")
    ```

- `data`

  - object | function

  - Vue 实例对应的数据对象

  - ```vue
    data: { counter: 0 }
    ```

- `methods`

  - {[key: string ]:function}

  - 定义属于 Vue 的一些方法，可以放在其他地方调用

  - ```vue
    methods:{ add: function(){counter++} }, add() {counter++} 一样的
    ```

- `computed` 计算属性， methods 也可以实现，但是每次调用 methods 都需要重新执行，computed 只需要一次(值修改都需要重新计算)

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

  - 计算属性都含有`set`和 `get`方法, 计算属性一般都没有 set 方法,为只读方法

  - ```
    name: {
    set: function(value){}  // value为设置的value
    set(value){} // 与上一样
    get: function(){return this.message}
    }
    ```

  - 可以在控制台使用 app.name= 'value' 进行测试，会调用 set 方法

- `filters` 过滤器, 类似 django

  - ```js
    filters:{
        add_2(num){
            return num+2
        }
    }
    ```

  - 使用方式

    ```js
    {
      {
        num | add_2;
      }
    }
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

- `created`

  - ```js
    new Vue({
      data: {
        a: 1,
      },
      created: function() {
        // `this` 指向 vm 实例
        console.log("a is: " + this.a);
      },
    });
    // => "a is: 1"
    ```

  - 也有一些其它的钩子，在实例生命周期的不同阶段被调用，如 [`mounted`](https://cn.vuejs.org/v2/api/#mounted)、[`updated`](https://cn.vuejs.org/v2/api/#updated) 和 [`destroyed`](https://cn.vuejs.org/v2/api/#destroyed)。生命周期钩子的 `this` 上下文指向调用它的 Vue 实例。

  - 不要在选项属性或回调上使用[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，比如 `created: () => console.log(this.a)` 或 `vm.$watch('a', newValue => this.myMethod())`。因为箭头函数并没有 `this`，`this` 会作为变量一直向上级词法作用域查找，直至找到为止，经常导致 `Uncaught TypeError: Cannot read property of undefined` 或 `Uncaught TypeError: this.myMethod is not a function` 之类的错误。

  [生命周期图 ](https://cn.vuejs.org/images/lifecycle.png)

- `watch`: 侦听器

  - 虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。这就是为什么 Vue 通过 `watch` 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

  - ```html
    <input v-model="question" />

    <script>
      var watchExampleVM = new Vue({
        el: "#app",
        data: {
          question: "",
        },
        watch: {
          // 如果 `question` 发生改变，这个函数就会运行
          question: function() {
            console.log("change");
          },
        },
      });
    </script>
    ```

  -

`v-for` ： 迭代遍历

`官方推荐使用的时候加一个:key`, 这样可以更高效的更新 DOM

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
    movies: ["1", "2", "3"],
    label: "movies",
  },
});
```

如果遍历 对象

```html
直接遍历的时候为value
<div v-for="item in items">
  {{item}}
</div>

<div v-for="(value, key) in items">// 前面的值为value {{key}}:{{value}}</div>

<div v-for="(value, key, index) in items">
  // 还可以获取index {{key}}:{{value}} {{index}}
</div>

data: { item:{name: "Edgar", age:20} } v-for
也可以接受整数。在这种情况下，它会把模板重复对应次数。

<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```

`v-once`: 显示的值为第一次显示的值，不会变化

```html
<div>
  {{message }}
</div>
<div v-once>{{message}}</div>
// 在调试台修改以app.message="anther value"修改message值的时候 //
使用了v-once的不会变，但是第一个会变
```

`v-html`

```html
<div>{{url}}</div>
// 显示全部的字符串
<div v-html="url"></div>
// 显示标签形式
```

`v-text`

```html
<div>
  {{message}}
</div>

<div v-text="message"></div>
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
<div v-cloak></div>
<!--主要是为了避免js执行的时候出现问题，直接显示{{message}}-->
在vue解析之前div中有一个v-cloak属性 在vue解析之后div中v-cloak属性消失
其实在实际开发中使用不多，因为我们使用过的模板都是被渲染过的
```

```css
[v-cloak] {
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

这里的 key 是 css 样式的属性名 backgroundColor
`key不会解释为变量，但是value会`
可以用驼峰命名法，也可以用`-`进行分割

backgroundColor background-color

```html
<script>
  const app = new Vue({
    el: "#app",
    data: {
      imgUrl: "https://vuejs.org/images/logo.png",
      isActive: true,
      isLine: true,
      size: 100,
      style: { fontSize: "100px" },
    },
    methods: {
      getStyle: function() {
        return { fontSize: this.size + "px" };
      },
    },
  });
</script>
```

Mustache 语法不能作用在 HTML attribute 上，遇到这种情况应该使用 [`v-bind` 指令](https://cn.vuejs.org/v2/api/#v-bind)：

```html
<div v-bind:id="dynamicId"></div>
```

`v-on` 事件监视， `可以省略v-on 使用@`

```html
<button @click="click()"></button>" 没有参数可以使用下面两种方式
<button @click="click"></button>" 有参数使用小括号，然后加入参数
<button @click="click(2)"></button>

如果有参数，但是没有传入参数，默认vue会将浏览器的事件对象event传入
方法定义的时候需要event对象，同时也需要其他参数
<button @click="click(123, $event)"></button>
//
默认的时候如果参数是没有引号包括的字符串(符合变量命名规则的)，会被识别成data里面的元素
！！！！！！！！ click(param, event){ console.log(param, event) } 事件冒泡,
点击btn，那么div中的click也会被执行
<div @click="divClick">
  <button @click="btnClick"></button>
</div>
避免事件冒泡，使用修饰符 stop
<div @click="divClick">
  div html
  <button @click.stop="btnClick"></button>
</div>

<form action=".">
  <input type="submit" @click.prevent="submit" />
</form>

默认的话使用input submit 之后会默认的将所有的form中信息收集然后传到服务器，
使用prevent可以自定义上传的内容(可能需要使用ajax等) 监听键盘的输入 松开按键
<input type="text" @keyup="keyUp" />
Enter 键
<input type="text" @keyup.enter="enter" />

自定义组件需要使用.native 修饰符，否则无法监听到事件
<my-label @click.native></my-label>

<input type="text" @click.once="" /> 只监听第一次 其他事件都可以使用这样的修饰符
```

修饰符

```
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>

<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>

<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">

<input v-on:keyup.page-down="onPageDown">
```

为了在必要的情况下支持旧浏览器，Vue 提供了绝大多数常用的按键码的别名：

- `.enter`
- `.tab`
- `.delete` (捕获“删除”和“退格”键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

还可以通过全局 `config.keyCodes` 对象[自定义按键修饰符别名](https://cn.vuejs.org/v2/api/#keyCodes)：

```
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```

可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`

```html
<!-- Ctrl + Click -->
<div v-on:click.ctrl="doSomething">Do something</div>
```

`.exact` 修饰符允许你控制由精确的系统修饰符组合触发的事件。

```html
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button v-on:click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button v-on:click.ctrl.exact="onCtrlClick">A</button>
```

鼠标修饰符

- `.left`
- `.right`
- `.middle`

### 判断

`v-if v-else v-else-if`

```html
<div>
  <div v-if="score>=90">优秀</div>
  <div v-else-if="score>=80">良好</div>
  <div v-else>一般</div>
</div>
条件比较多的时候可以在computed属性里面进行计算

<div v-if="show">
  <label>username</label>
  <input type="text" id="username" />
</div>
<div v-else>
  <label>password</label>
  <input type="password" id="password" />
</div>
<button @click="show=!show"></button>

如果直接这样的话，由于vue底层的处理方式(元素复用)，
第一个input中输入值之后，切换到第二个input的时候，中间的值为原来的值
如何避免：定义一个key属性，不同的话会重新创建
<div v-if="show">
  <label>username</label>
  <input type="text" id="username" :key="username" />
</div>
<div v-else>
  <label>password</label>
  <input type="password" id="password" :key="password" />
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
    },
  });
</script>
```

`如果判断语句为false的时候，元素会display:none`

`v-if`则完全不在 dom 中

如果切换频率比较高的时候可以使用 v-show

如果只有一次使用的话，使用 v-if 比较好

`v-model`

- 双向绑定表单中的数据, 绑定的元素数据改变，对应的数据也变，对应的数据变，表单也变

- ```html
  <input type="text" v-model="message" />
  ```

- 修饰符

  ```html
  .lazy 当前元素失去焦点或回车等才进行实时绑定
  <input type="text" v-model.lazy="message" />

  .number 默认绑定之后数据均为字符串
  <input type="text" v-model.number="num" />
  {{typeof num}}
  ```

.trim 消除两端的空字符串

````





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

`reverse`



索引进行修改的是不会重新渲染的

通过索引值进行修改可以使用：

​```js
第一个参数表示要修改的对象，第二个表示修改的索引值，第三个表示替换的值
Vue.set(this.movies, 0, '海王')
````

{{ typeof message}} -> 获取类型

### [使用 JavaScript 表达式](https://cn.vuejs.org/v2/guide/syntax.html#使用-JavaScript-表达式)

迄今为止，在我们的模板中，我们一直都只绑定简单的属性键值。但实际上，对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。

```html
{{ number + 1 }} {{ ok ? 'YES' : 'NO' }} {{ message.split('').reverse().join('')
}}

<div v-bind:id="'list-' + id"></div>
```

表单的清空

```
this.form.resetFields()
```

### [动态参数](https://cn.vuejs.org/v2/guide/syntax.html#动态参数)

从 2.6.0 开始，可以用方括号括起来的 JavaScript 表达式作为一个指令的参数：

```html
<!--
注意，参数表达式的写法存在一些约束，如之后的“对动态参数表达式的约束”章节所述。
-->
<a v-bind:[attributeName]="url"> ... </a>
```

这里的 `attributeName` 会被作为一个 JavaScript 表达式进行动态求值，求得的值将会作为最终的参数来使用。例如，如果你的 Vue 实例有一个 `data` 属性 `attributeName`，其值为 `"href"`，那么这个绑定将等价于 `v-bind:href`。

同样地，你可以使用动态参数为一个动态的事件名绑定处理函数：

```html
<a v-on:[eventName]="doSomething"> ... </a>
```

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
    template: `<div>你好</div>`,
  });

  Vue.component("cpn", cpnC);
</script>
```

在 Vue2.x 中基本上不使用 extend，而使用语法糖(简写模式)

`默认注册的组件为全局组件，可以在所有的Vue实例下使用`

直接在 Vue 实例中`components`属性定义，为局部组件

父组件，子组件，每个组件都需要自己注册才能单独使用，但是父组件中包含子组件，子组件不注册也能在父组件中渲染

```js
const childComponent = Vue.extend({
  template: `<div>This is the child</div>`,
});
const parentComponent = Vue.extend({
  template: `<div>This is the parent</div>
	<child></child>`, // 使用child组件
  components: {
    child: childComponent,
  },
});
const app = new Vue({
  el: "#app",
  components: {
    parent: parentComponent,
  },
});
// app里面只能使用<parent></parent>
// 不能用<child></child>
```

语法糖使用组件

```vue
// 全局组件 Vue.component("cpn", { template:`
<div></div>
` }) // 局部组件 let app = new Vue({ el: "#app", components: { "cpn1":{
template:`
<div></div>
` } } })
```

组件的抽离

```html
<template id="cpn">
  <div>
    <h2>Hello</h2>
    <p>Test</p>
  </div>
</template>
<script>
  Vue.component("cpn", {
    template: "#cpn", // 取上面的id
  });
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
<template id="cpn">
  <div>
    <h2>{{counter}}</h2>
    // 组件的这种东西不能用new Vue中的data，只能为组件里的data
    <button @click="increase">+</button>
  </div>
</template>
<script>
  Vue.component("cpn", {
    template: "#cpn",
    // 这里的data必须是一个method
    data() {
      return {
        counter: 0,
      };
    },
    methods: {
      increase() {
        counter++;
      },
    },
  });
</script>
```

### 父子组件的通信

父传子：props

在子组件中使用 props，父组件中使用:props_name = ''进行绑定

子传父：事件

```html
<div id="app">
  <cpn v-bind:message="parentMessage"></cpn>
  <!--v-bind-->
</div>
<script src="vue.js"></script>

<script>
  const app = new Vue({
    el: "#app",
    data: {
      parentMessage: "父组件子组件信息",
    },
    components: {
      cpn: {
        template: `<div>{{message}}</div>`,
        //props: ["message"],  // 父传给子的数据，可以自定义名称
        props: {
          message: String, // 可以限制类型
          array: Array,
          // 还可以提供默认值
          test: {
            type: String,
            default: "Hello",
          },
        },
      },
    },
  });
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
          // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
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

`type`:

- `String`
- `Number`
- `Boolean`
- `Array`
- `Object`
- `Date`
- `Function`
- `Symbol`

子传父

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>子通信与父</title>
  </head>
  <body>
    <div id="app">
      <cpn @item-click="cpnClick"></cpn> // 不能使用驼峰命名法,
      这里会将子组件传递的参数给cpnClick，而不是event
    </div>

    <template id="cpn">
      // 子组件的模板
      <div>
        <ul>
          <li v-for="item in categories" @click="btnClick(item)">
            {{item.name}}
          </li>
        </ul>
      </div>
    </template>

    <script src="vue.js"></script>

    <script>
      const cpn = {
        //  子组件的
        template: "#cpn",
        data() {
          return {
            categories: [
              { id: 1, name: "第1个" },
              { id: 2, name: "第2个" },
              { id: 3, name: "第3个" },
              { id: 4, name: "第4个" },
            ],
          };
        },
        methods: {
          btnClick(item) {
            console.log(item);
            this.$emit("item-click", item); // 发送给父组件 事件名称，参数
          },
        },
      };

      const app = new Vue({
        el: "#app",
        data: {},
        components: {
          cpn,
        },
        methods: {
          cpnClick(item) {
            console.log(item);
          },
        },
      });
    </script>
  </body>
</html>
```

#### 父组件使用子组件的方法，

子组件指定`ref`

```html
<template>
    <Header ref='header'>
        </template>
```

```
this.$refs.header.childerMethod();
```

## 插槽内容

Vue 实现了一套内容分发的 API，这套 API 的设计灵感源自 [Web Components 规范草案](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md)，将 `` 元素作为承载分发内容的出口。

它允许你像这样合成组件：

```html
<navigation-link url="/profile">
  Your Profile
</navigation-link>
```

然后你在 `` 的模板中可能会写为：

```html
<a v-bind:href="url" class="nav-link">
  <slot></slot>
</a>
```

当组件渲染的时候，`` 将会被替换为“Your Profile”。插槽内可以包含任何模板代码，包括 HTML：

```
<navigation-link url="/profile">
  <!-- 添加一个 Font Awesome 图标 -->
  <span class="fa fa-user"></span>
  Your Profile
</navigation-link>
```

甚至其它的组件：

```html
<navigation-link url="/profile">
  <!-- 添加一个图标的组件 -->
  <font-awesome-icon name="user"></font-awesome-icon>
  Your Profile
</navigation-link>
```

如果 `没有包含一个` 元素，则该组件起始标签和结束标签之间的任何内容都会被抛弃。

有时我们需要多个插槽。例如对于一个带有如下模板的 `<base-layout>` 组件：

```html
<div class="container">
  <header>
    <!-- 我们希望把页头放这里 -->
  </header>
  <main>
    <!-- 我们希望把主要内容放这里 -->
  </main>
  <footer>
    <!-- 我们希望把页脚放这里 -->
  </footer>
</div>
```

对于这样的情况，`` 元素有一个特殊的 attribute：`name`。这个 attribute 可以用来定义额外的插槽：

```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

一个不带 `name` 的 `` 出口会带有隐含的名字“default”。

在向具名插槽提供内容的时候，我们可以在一个 `元素上使用`v-slot`指令，并以`v-slot` 的参数的形式提供其名称：

```html
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

现在 `元素中的所有内容都将会被传入相应的插槽。任何没有被包裹在带有`v-slot`的` 中的内容都会被视为默认插槽的内容。

然而，如果你希望更明确一些，仍然可以在一个 ` 中包裹默认插槽的内容：

```html
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <template v-slot:default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

任何一种写法都会渲染出：

```html
<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>
```

注意 **`v-slot` 只能添加在 ` 上** (只有[一种例外情况](https://cn.vuejs.org/v2/guide/components-slots.html#独占默认插槽的缩写语法))，这一点和已经废弃的 [`slot` attribute](https://cn.vuejs.org/v2/guide/components-slots.html#废弃了的语法) 不同。

## 编译作用域

当你想在一个插槽中使用数据时，例如：

```
<navigation-link url="/profile">
  Logged in as {{ user.name }}
</navigation-link>
```

该插槽跟模板的其它地方一样可以访问相同的实例属性 (也就是相同的“作用域”)，而**不能**访问 `` 的作用域。例如 `url` 是访问不到的：

```
<navigation-link url="/profile">
  Clicking here will send you to: {{ url }}
  <!--
  这里的 `url` 会是 undefined，因为 "/profile" 是
  _传递给_ <navigation-link> 的而不是
  在 <navigation-link> 组件*内部*定义的。
  -->
</navigation-link>
```

作为一条规则，请记住：

> 父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

## 自定义组件的 `v-model`

一个组件上的 `v-model` 默认会利用名为 `value` 的 prop 和名为 `input` 的事件，但是像单选框、复选框等类型的输入控件可能会将 `value` attribute 用于[不同的目的](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox#Value)。`model` 选项可以用来避免这样的冲突：

```
Vue.component('base-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean
  },
  template: `
    <input
      type="checkbox"
      v-bind:checked="checked"
      v-on:change="$emit('change', $event.target.checked)"
    >
  `
})
```

现在在这个组件上使用 `v-model` 的时候：

```
<base-checkbox v-model="lovingVue"></base-checkbox>
```

这里的 `lovingVue` 的值将会传入这个名为 `checked` 的 prop。同时当 `` 触发一个 `change` 事件并附带一个新的值的时候，这个 `lovingVue` 的属性将会被更新。

注意你仍然需要在组件的 `props` 选项里声明 `checked` 这个 prop。

## `.sync` 修饰符

在有些情况下，我们可能需要对一个 prop 进行“双向绑定”。不幸的是，真正的双向绑定会带来维护上的问题，因为子组件可以修改父组件，且在父组件和子组件都没有明显的改动来源。

这也是为什么我们推荐以 `update:myPropName` 的模式触发事件取而代之。举个例子，在一个包含 `title` prop 的假设的组件中，我们可以用以下方法表达对其赋新值的意图：

```js
this.$emit("update:title", newTitle);
```

然后父组件可以监听那个事件并根据需要更新一个本地的数据属性。例如：

```vue
<text-document
  v-bind:title="doc.title"
  v-on:update:title="doc.title = $event"
></text-document>
```

为了方便起见，我们为这种模式提供一个缩写，即 `.sync` 修饰符：

```
<text-document v-bind:title.sync="doc.title"></text-document>
```

注意带有 `.sync` 修饰符的 `v-bind` **不能**和表达式一起使用 (例如 `v-bind:title.sync=”doc.title + ‘!’”` 是无效的)。取而代之的是，你只能提供你想要绑定的属性名，类似 `v-model`。

当我们用一个对象同时设置多个 prop 的时候，也可以将这个 `.sync` 修饰符和 `v-bind` 配合使用：

```vue
<text-document v-bind.sync="doc"></text-document>
```

这样会把 `doc` 对象中的每一个属性 (如 `title`) 都作为一个独立的 prop 传进去，然后各自添加用于更新的 `v-on` 监听器。

将 `v-bind.sync` 用在一个字面量的对象上，例如 `v-bind.sync=”{ title: doc.title }”`，是无法正常工作的，因为在解析一个像这样的复杂表达式的时候，有很多边缘情况需要考虑。

---

### 资源打包

开发一个 vue 项目，步骤

- 用 vue-cli (2 版本)初始化一个项目
- 按照依赖 `npm install`
- `npm run dev`进行开发者模式
- `npm run build`来构建一个项目
- 将 dist 下面的文件拷贝到需要的目录，一定要在服务器上运行才有效

`vue.config.js`中配置多个页面应用

[完整的配置参考文件](https://cli.vuejs.org/zh/config)

### outputDir

- Type: `string`

- Default: `'dist'`

  当运行 `vue-cli-service build` 时生成的生产环境构建文件的目录。注意目标目录在构建之前会被清除 (构建时传入 `--no-clean` 可关闭该行为)。

### assetsDir

- Type: `string`

- Default: `''`

  放置生成的静态资源 (js、css、img、fonts) 的 (相对于 `outputDir` 的) 目录。

  从生成的资源覆写 filename 或 chunkFilename 时，`assetsDir` 会被忽略。

### indexPath

- Type: `string`

- Default: `'index.html'`

  指定生成的 `index.html` 的输出路径 (相对于 `outputDir`)。也可以是一个绝对路径

### filenameHashing

- Type: `boolean`

- Default: `true`

  默认情况下，生成的静态资源在它们的文件名中包含了 hash 以便更好的控制缓存。然而，这也要求 index 的 HTML 是被 Vue CLI 自动生成的。如果你无法使用 Vue CLI 生成的 index HTML，你可以通过将这个选项设为 `false` 来关闭文件名哈希

### pages

- Type: `Object`

- Default: `undefined`

  在 multi-page 模式下构建应用。每个“page”应该有一个对应的 JavaScript 入口文件。其值应该是一个对象，对象的 key 是入口的名字，value 是：

  - 一个指定了 `entry`, `template`, `filename`, `title` 和 `chunks` 的对象 (除了 `entry` 之外都是可选的)；

  - 或一个指定其 `entry` 的字符串。

  - ```js
    module.exports = {
      pages: {
        index: {
          // page 的入口
          entry: "src/index/main.js",
          // 模板来源
          template: "public/index.html",
          // 在 dist/index.html 的输出
          filename: "index.html",
          // 当使用 title 选项时，
          // template 中的 title 标签需要是 <title><%= htmlWebpackPlugin.options.title %></title>
          title: "Index Page",
          // 在这个页面中包含的块，默认情况下会包含
          // 提取出来的通用 chunk 和 vendor chunk。
          chunks: ["chunk-vendors", "chunk-common", "index"],
        },
        // 当使用只有入口的字符串格式时，
        // 模板会被推导为 `public/subpage.html`
        // 并且如果找不到的话，就回退到 `public/index.html`。
        // 输出文件名会被推导为 `subpage.html`。
        subpage: "src/subpage/main.js",
      },
    };
    ```

    当在 multi-page 模式下构建时，webpack 配置会包含不一样的插件 (这时会存在多个 `html-webpack-plugin` 和 `preload-webpack-plugin` 的实例)。如果你试图修改这些插件的选项，请确认运行 `vue inspect`。

### productionSourceMap

- Type: `boolean`

- Default: `true`

  如果你不需要生产环境的 source map，可以将其设置为 `false` 以加速生产环境构建

### crossorigin

- Type: `string`

- Default: `undefined`

  设置生成的 HTML 中 `和` 标签的 `crossorigin` 属性。

  需要注意的是该选项仅影响由 `html-webpack-plugin` 在构建时注入的标签 - 直接写在模版 (`public/index.html`) 中的标签不受影响。

## index.html 文件设置

### 插值

因为 index 文件被用作模板，所以你可以使用 [lodash template](https://lodash.com/docs/4.17.10#template) 语法插入内容：

- `<%= VALUE %>` 用来做不转义插值；
- `<%- VALUE %>` 用来做 HTML 转义插值；
- `<% expression %>` 用来描述 JavaScript 流程控制。

除了[被 `html-webpack-plugin` 暴露的默认值](https://github.com/jantimon/html-webpack-plugin#writing-your-own-templates)之外，所有[客户端环境变量](https://cli.vuejs.org/zh/guide/mode-and-env.html#using-env-variables-in-client-side-code)也可以直接使用。例如，`BASE_URL` 的用法：

```html
<link rel="icon" href="<%= BASE_URL %>favicon.ico" />
```

## 模式

**模式**是 Vue CLI 项目中一个重要的概念。默认情况下，一个 Vue CLI 项目有三个模式：

- `development` 模式用于 `vue-cli-service serve`
- `production` 模式用于 `vue-cli-service build` 和 `vue-cli-service test:e2e`
- `test` 模式用于 `vue-cli-service test:unit`

注意模式不同于 `NODE_ENV`，一个模式可以包含多个环境变量。也就是说，每个模式都会将 `NODE_ENV` 的值设置为模式的名称——比如在 development 模式下 `NODE_ENV` 的值会被设置为 `"development"`。

你可以通过为 `.env` 文件增加后缀来设置某个模式下特有的环境变量。比如，如果你在项目根目录创建一个名为 `.env.development` 的文件，那么在这个文件里声明过的变量就只会在 development 模式下被载入。

你可以通过传递 `--mode` 选项参数为命令行覆写默认的模式。例如，如果你想要在构建命令中使用开发环境变量，请在你的 `package.json` 脚本中加入：

```text
"dev-build": "vue-cli-service build --mode development",
```

## vue-cli-service serve

```text
用法：vue-cli-service serve [options] [entry]

选项：

  --open    在服务器启动时打开浏览器
  --copy    在服务器启动时将 URL 复制到剪切版
  --mode    指定环境模式 (默认值：development)
  --host    指定 host (默认值：0.0.0.0)
  --port    指定 port (默认值：8080)
  --https   使用 https (默认值：false)
```

`vue-cli-service serve` 命令会启动一个开发服务器 (基于 [webpack-dev-server](https://github.com/webpack/webpack-dev-server)) 并附带开箱即用的模块热重载 (Hot-Module-Replacement)。

除了通过命令行参数，你也可以使用 `vue.config.js` 里的 [devServer](https://cli.vuejs.org/zh/config/#devserver) 字段配置开发服务器。

命令行参数 `[entry]` 将被指定为唯一入口，而非额外的追加入口。尝试使用 `[entry]` 覆盖 `config.pages` 中的 `entry` 将可能引发错误。

## [vue-cli-service build](https://cli.vuejs.org/zh/guide/cli-service.html#vue-cli-service-build)

```text
用法：vue-cli-service build [options] [entry|pattern]

选项：
  --mode        指定环境模式 (默认值：production)
  --dest        指定输出目录 (默认值：dist)
  --modern      面向现代浏览器带自动回退地构建应用
  --target      app | lib | wc | wc-async (默认值：app)
  --name        库或 Web Components 模式下的名字 (默认值：package.json 中的 "name" 字段或入口文件名)
  --no-clean    在构建项目之前不清除目标目录
  --report      生成 report.html 以帮助分析包内容
  --report-json 生成 report.json 以帮助分析包内容
  --watch       监听文件变化
```

`vue-cli-service build` 会在 `dist/` 目录产生一个可用于生产环境的包，带有 JS/CSS/HTML 的压缩，和为更好的缓存而做的自动的 vendor chunk splitting。它的 chunk manifest 会内联在 HTML 里。

这里还有一些有用的命令参数：

- `--modern` 使用[现代模式](https://cli.vuejs.org/zh/guide/browser-compatibility.html#现代模式)构建应用，为现代浏览器交付原生支持的 ES2015 代码，并生成一个兼容老浏览器的包用来自动回退。
- `--target` 允许你将项目中的任何组件以一个库或 Web Components 组件的方式进行构建。更多细节请查阅[构建目标](https://cli.vuejs.org/zh/guide/build-targets.html)。
- `--report` 和 `--report-json` 会根据构建统计生成报告，它会帮助你分析包中包含的模块们的大小。

## [vue-cli-service inspect](https://cli.vuejs.org/zh/guide/cli-service.html#vue-cli-service-inspect)

```text
用法：vue-cli-service inspect [options] [...paths]

选项：

  --mode    指定环境模式 (默认值：development)
```

你可以使用 `vue-cli-service inspect` 来审查一个 Vue CLI 项目的 webpack config。更多细节请查阅[审查 webpack config](https://cli.vuejs.org/zh/guide/webpack.html#审查项目的-webpack-config)。

## [查看所有的可用命令](https://cli.vuejs.org/zh/guide/cli-service.html#查看所有的可用命令)

有些 CLI 插件会向 `vue-cli-service` 注入额外的命令。例如 `@vue/cli-plugin-eslint` 会注入 `vue-cli-service lint` 命令。你可以运行以下命令查看所有注入的命令：

```bash
npx vue-cli-service help
```

你也可以这样学习每个命令可用的选项：

```bash
npx vue-cli-service help [command]

```

## 跨域问题解决-------------important

### devServer

- Type: `Object`

  [所有 `webpack-dev-server` 的选项](https://webpack.js.org/configuration/dev-server/)都支持。注意：

  - 有些值像 `host`、`port` 和 `https` 可能会被命令行参数覆写。
  - 有些值像 `publicPath` 和 `historyApiFallback` 不应该被修改，因为它们需要和开发服务器的 [publicPath](https://cli.vuejs.org/zh/config/#baseurl) 同步以保障正常的工作。

### [devServer.proxy](https://cli.vuejs.org/zh/config/#devserver-proxy)devServer.proxy

- Type: `string | Object`

  **如果你的前端应用和后端 API 服务器没有运行在同一个主机上**，你需要在开发环境下将 API 请求代理到 API 服务器。这个问题可以通过 `vue.config.js` 中的 `devServer.proxy` 选项来配置。

  `devServer.proxy` 可以是一个指向开发环境 API 服务器的字符串：

  ```js
  module.exports = {
    devServer: {
      proxy: "http://localhost:4000",
    },
  };
  ```

  这会告诉开发服务器将任何未知请求 (没有匹配到静态文件的请求) 代理到`http://localhost:4000`。

  如果你想要更多的代理控制行为，也可以使用一个 `path: options` 成对的对象。完整的选项可以查阅 [http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware#proxycontext-config) 。

  ```js
  module.exports = {
    devServer: {
      proxy: {
        "/api": {
          target: "<url>",
          ws: true,
          changeOrigin: true,
        },
        "/foo": {
          target: "<other_url>",
        },
      },
    },
  };
  ```

---

# axios 的使用

first install

```
npm install axios
# or
yarn add axios
```

引入

```js
import axios from "axios"; // 模块项目中
const axios = require("axios"); // 普通的调用
```

GET 方式

```js
axios.get('/user?ID=12345')
  .then(function (response) {
    // handle success
    console.log(response);
  })
  .catch(function (error) {
    // handle error
    console.log(error);
  })
  .finally(function () {
    // always executed
  });

// Optionally the request above could also be done as
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  ....;

//-----如果想要异步查询
async function getUser() {
  try {
    const response = await axios.get('/user?ID=12345');
    console.log(response);
  } catch (error) {
    console.error(error);
  }
}
```

POST 方式

```js
axios.post("/user", {
  firstName: "Fred",
  lastName: "Flintstone",
});
```

函数中调用

```js
function getUserAccount() {
  return axios.get("/user/12345");
}

function getUserPermissions() {
  return axios.get("/user/12345/permissions");
}

axios.all([getUserAccount(), getUserPermissions()]).then(
  axios.spread(function(acct, perms) {
    // Both requests are now complete
  })
);
```

使用 api 形式

```js
// Send a POST request
axios({
  method: "post",
  url: "/user/12345",
  data: {
    firstName: "Fred",
    lastName: "Flintstone",
  },
});
```

### 常见的配置项

```js
{
  // `url` is the server URL that will be used for the request
  url: '/user',

  // `method` is the request method to be used when making the request
  method: 'get', // default

  // `baseURL` will be prepended to `url` unless `url` is absolute.
  // It can be convenient to set `baseURL` for an instance of axios to pass relative URLs
  // to methods of that instance.
  baseURL: 'https://some-domain.com/api/',

  // `transformRequest` allows changes to the request data before it is sent to the server
  // This is only applicable for request methods 'PUT', 'POST', 'PATCH' and 'DELETE'
  // The last function in the array must return a string or an instance of Buffer, ArrayBuffer,
  // FormData or Stream
  // You may modify the headers object.
  transformRequest: [function (data, headers) {
    // Do whatever you want to transform the data

    return data;
  }],

  // `transformResponse` allows changes to the response data to be made before
  // it is passed to then/catch
  transformResponse: [function (data) {
    // Do whatever you want to transform the data

    return data;
  }],

  // `headers` are custom headers to be sent
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` are the URL parameters to be sent with the request
  // Must be a plain object or a URLSearchParams object
  params: {
    ID: 12345
  },

  // `paramsSerializer` is an optional function in charge of serializing `params`
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function (params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  },

  // `data` is the data to be sent as the request body
  // Only applicable for request methods 'PUT', 'POST', and 'PATCH'
  // When no `transformRequest` is set, must be of one of the following types:
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - Browser only: FormData, File, Blob
  // - Node only: Stream, Buffer
  data: {
    firstName: 'Fred'
  },

  // syntax alternative to send data into the body
  // method post
  // only the value is sent, not the key
  data: 'Country=Brasil&City=Belo Horizonte',

  // `timeout` specifies the number of milliseconds before the request times out.
  // If the request takes longer than `timeout`, the request will be aborted.
  timeout: 1000, // default is `0` (no timeout)

  // `withCredentials` indicates whether or not cross-site Access-Control requests
  // should be made using credentials
  withCredentials: false, // default

  // `adapter` allows custom handling of requests which makes testing easier.
  // Return a promise and supply a valid response (see lib/adapters/README.md).
  adapter: function (config) {
    /* ... */
  },

  // `auth` indicates that HTTP Basic auth should be used, and supplies credentials.
  // This will set an `Authorization` header, overwriting any existing
  // `Authorization` custom headers you have set using `headers`.
  // Please note that only HTTP Basic auth is configurable through this parameter.
  // For Bearer tokens and such, use `Authorization` custom headers instead.
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

  // `responseType` indicates the type of data that the server will respond with
  // options are: 'arraybuffer', 'document', 'json', 'text', 'stream'
  //   browser only: 'blob'
  responseType: 'json', // default

  // `responseEncoding` indicates encoding to use for decoding responses
  // Note: Ignored for `responseType` of 'stream' or client-side requests
  responseEncoding: 'utf8', // default

  // `xsrfCookieName` is the name of the cookie to use as a value for xsrf token
  xsrfCookieName: 'XSRF-TOKEN', // default

  // `xsrfHeaderName` is the name of the http header that carries the xsrf token value
  xsrfHeaderName: 'X-XSRF-TOKEN', // default

  // `onUploadProgress` allows handling of progress events for uploads
  onUploadProgress: function (progressEvent) {
    // Do whatever you want with the native progress event
  },

  // `onDownloadProgress` allows handling of progress events for downloads
  onDownloadProgress: function (progressEvent) {
    // Do whatever you want with the native progress event
  },

  // `maxContentLength` defines the max size of the http response content in bytes allowed
  maxContentLength: 2000,

  // `validateStatus` defines whether to resolve or reject the promise for a given
  // HTTP response status code. If `validateStatus` returns `true` (or is set to `null`
  // or `undefined`), the promise will be resolved; otherwise, the promise will be
  // rejected.
  validateStatus: function (status) {
    return status >= 200 && status < 300; // default
  },

  // `maxRedirects` defines the maximum number of redirects to follow in node.js.
  // If set to 0, no redirects will be followed.
  maxRedirects: 5, // default

  // `socketPath` defines a UNIX Socket to be used in node.js.
  // e.g. '/var/run/docker.sock' to send requests to the docker daemon.
  // Only either `socketPath` or `proxy` can be specified.
  // If both are specified, `socketPath` is used.
  socketPath: null, // default

  // `httpAgent` and `httpsAgent` define a custom agent to be used when performing http
  // and https requests, respectively, in node.js. This allows options to be added like
  // `keepAlive` that are not enabled by default.
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // 'proxy' defines the hostname and port of the proxy server.
  // You can also define your proxy using the conventional `http_proxy` and
  // `https_proxy` environment variables. If you are using environment variables
  // for your proxy configuration, you can also define a `no_proxy` environment
  // variable as a comma-separated list of domains that should not be proxied.
  // Use `false` to disable proxies, ignoring environment variables.
  // `auth` indicates that HTTP Basic auth should be used to connect to the proxy, and
  // supplies credentials.
  // This will set an `Proxy-Authorization` header, overwriting any existing
  // `Proxy-Authorization` custom headers you have set using `headers`.
  proxy: {
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // `cancelToken` specifies a cancel token that can be used to cancel the request
  // (see Cancellation section below for details)
  cancelToken: new CancelToken(function (cancel) {
  })
}
```

#### 返回数据的常用属性

```js
{
  // `data` is the response that was provided by the server
  data: {},

  // `status` is the HTTP status code from the server response
  status: 200,

  // `statusText` is the HTTP status message from the server response
  statusText: 'OK',

  // `headers` the headers that the server responded with
  // All header names are lower cased
  headers: {},

  // `config` is the config that was provided to `axios` for the request
  config: {},

  // `request` is the request that generated this response
  // It is the last ClientRequest instance in node.js (in redirects)
  // and an XMLHttpRequest instance in the browser
  request: {}
}
```

### axios 有时候收不到信息

使用原型进行处理

```js
Vue.prototype.$axios = axios;

this.$axios({
  method: "post",
  url: "user/login",
  data: qs.stringify({
    username: values.username,
    password: values.password,
  }),
});
```

---

### Vuex 使用

#### 基本概念

vuex 是实现组件全局状态管理的一种机制，可以方便的实现组件之间数据的共享，尤其是当组件的的数据传输跨越层数比较多的时候

#### 好处

- 能够在 vuex 中集中管理共享的数据，易于开发和后期维护
- 能够高效地实现组件之间的数据共享，提高开发效率
- 储存在 vuex 中的数据都是响应式的，能够实时保持与页面的同步

什么样的数据适合

一般情况下，只有组件之间共享的数据，才有必要储存在 vuex 中，对于组件中的私有数据，依旧存储在组件自身的 data 中即可

#### 基本使用

1. 安装相关依赖

   ```bash
   npm install vuex --save
   ```

2. 导入 vuex 包

    ```js
    import Vuex from "vuex";
    Vue.use(Vuex);
    ```

3. 创建 store 对象

   ```js
   const store = new Vuex.Store({
     // state 中存放的是全局共享的数据
     state: {
       count: 0,
     },
   });
   ```

4. 将 store 对象挂载在 vue 实例中

    ```js
    new Vue({
      el: "#app",
      render: (h) => h(app),
      store,
    });
    ```

### State
State提供唯一的公共数据源，所有共享的数据都要放到Store的State中进行储存

访问State实例
  - 第一种方式
    ```
    this.$store.state.data_name
    ```
  - 第二种方式
    ```js
    // 从中Vuex按需导入mapState 函数
    import { mapState } from 'vue'

    // 将当前组件需要的全局数据映射到当前组件的 computed 属性中，但如果不设置其他的，不能够进行修改，只能进行访问
computed:{
      ...mapState(["count"])
    }
    
```



### Mutation

用来变更Store中的数据

- 只能够通过mutation更改store中的数据，不可以直接进行操作

- 虽然比较繁琐一些，但是可以监控所有数据的变化

- ```js
  const store = new Vuex.Store({
      state:{
          count: 0
      },
      // 定义一个mutations
      mutations: {
          add(state){
              // 变更状态
              state.count ++
          }
      }
  })
  
  
  import { mapMutations } from 'vuex'
  // 触发mutation
  methods: {
      add(){
          // 触发的第一种方式
          this.$store.commit("add")
      },
          // 指定mutations函数，映射为当前组件的methods函数
          ...mapMutations(["add", "dec"])
  }
  ```



### Action

Action 用来处理异步任务

如果要处理异步操作，必须要通过Action，不能使用Mutation，但在Action中总是通过触发Mutation间接变更数据

```js
actions:{
        // 可以改变state中的数据，如果在mutation中使用，不会改变state的值
        addSync(context){
            setTimeout(()=>{
                context.commit("add")
            }, 1000)
        }
    },
        
// 第一种触发方式
add_action(){
      this.$store.dispatch("addSync")
    },
        
// 第二种方式
import { mapActions } from 'vuex'

methods: {
    ...mapActions(["addSync"])
}
```



### Getter

对Store中的数据进行加工处理形成新的数据，类似计算属性

```js
 getters:{
        show: state =>{
            return "目前count为：" + state.count
        }
    }
// 第一种方式
this.$store.getters.show
// 第二种方式
import { mapGetters } from 'vuex'

computed: {
    ...mapGetters(["show"])
}
```



