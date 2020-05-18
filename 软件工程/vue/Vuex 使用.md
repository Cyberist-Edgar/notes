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