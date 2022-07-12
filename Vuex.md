# Vuex 

vuex是一个专为vue.js应用程序开发的状态管理模式。



### 什么样的状态需要放入vuex中：

1、如果是大型项目，你一定遇到过多个状态，在多个界面间的共享问题。

2、比如用户登录状态用户名称、头像、地理位置信息

3、比如商品的收藏、购物车中的物品

4、这些状态信息，我们都可以放在统一的地方，对他进行保存和管理，而且他们还是响应式的



### vuex使用：

1、安装vuex

2、在src目录下创建store目录，在store目录中创建index.js文件

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

//1.安装插件
Vue.use(Vuex)
//2.创建对象
const store = new Vuex.Store({
    state:{},
    
})
//3.导出store对象
export default store
```

4、挂载到main.js

```javascript
import store from './store'

Vue.prototype.$store = store
```



### vuex数据修改：

![vuex](vuex.png)

vuex数据修改如果是异步方式，需要通过actions提交到mutaions里面，在同步到state中。

但是，如果是同步的方式，可以直接mutaions提交。vue这样做，是因为直接修改state状态会导致devtools（vue开发工具）监测不到state的改变。而通过mutations修改，则会被监测。

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

//1.安装插件
Vue.use(Vuex)
//2.创建对象
const store = new Vuex.Store({
    state:{
        connter:1000
    },
    mutations：{
    	//方法
    	increment(state){
    		state.counter++
		},
        decrement(state){
            state.counter--
        }
	},
    actions:{},
    getters:{},
    modules:{}    
})
//3.导出store对象
export default store



//compent使用
this.$store.commit('increment')
```



### vuex核心概念

##### 1.state 单一状态树（单一数据源）



##### 2.getters 

类似于单一属性的计算属性。对state中的数据做一个处理

使用方法：

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

//1.安装插件
Vue.use(Vuex)
//2.创建对象
const store = new Vuex.Store({
    state:{
        connter:1000
    },
    mutations：{
    	//方法
    	increment(state){
    		state.counter++
		},
        decrement(state){
            state.counter--
        }
	},
    actions:{},
    getters:{
        //演示getter基本使用方法
        powerCounter(state){
            return state.counter * state.counter
        },
        //getter作为参数添加到方法中
        more20stuLength(state,getter){
            return getter.more20stu.length
        },
        //用户传入的参数,通过构建函数处理
        moreAgeStu(state){
            // return function(age){
            //   return state.students.filter(s => s.age > age)
            // }
            return age =>{
              return  state.students.filter(s => s.age > age)
            }
        }    
    },
    modules:{}    
})
//3.导出store对象
export default store



//compnet getter使用
{{this.$store.getter.powerCounter}}
```

​	

##### 3.mutation

mutation 包含两个部分：

​	1.字符串的事件类型（type）

​	2.一个回调函数 (handler)，该回调函数的第一个参数就是state

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

//1.安装插件
Vue.use(Vuex)
//2.创建对象
const store = new Vuex.Store({
    state:{
        connter:1000
    },
    mutations：{
    	//方法 increment事件类型，剩下的为回调函数
    	increment(state){
    		state.counter++
		},
        decrement(state){
            state.counter--
        },
        //mutations 传递额外的参数方式称为payload
        incrementCount(state,count){
            state.counter+=count
        }
	},
    actions:{},
    getters:{
        //演示getter基本使用方法
        powerCounter(state){
            return state.counter * state.counter
        },
        //getter作为参数添加到方法中
        more20stuLength(state,getter){
            return getter.more20stu.length
        },
        //用户传入的参数,通过构建函数处理
        moreAgeStu(state){
            // return function(age){
            //   return state.students.filter(s => s.age > age)
            // }
            return age =>{
              return  state.students.filter(s => s.age > age)
            }
        }    
    },
    modules:{}    
})
//3.导出store对象
export default store

//compnet getter使用
//普通提交风格
{{this.$store.commit('incrementCount',5)}}
//特殊封装风格,这种风格提交传递的是一个payload对象
{{this.$store.commit({
    type:'incrementCount',
    count
})}}
```

mutation 提交风格

​	mutation除了可以通过commit进行提交。还可以通过包含type属性的对象方式提交。

##### mutation响应规则

mutation修改的属性可以做到响应式的，但是添加或删除元素是做不到响应式的，可以通过Vue.set、Vue.delete 来实现相应式

##### mutation类型常量



##### 4.action

异步操作需要在action中提交

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

//1.安装插件
Vue.use(Vuex)
//2.创建对象
const store = new Vuex.Store({
    state:{
        connter:1000
    },
    mutations：{
    	//方法 increment事件类型，剩下的为回调函数
    	increment(state){
    		state.counter++
		},
        decrement(state){
            state.counter--
        },
        //mutations 传递额外的参数方式称为payload
        incrementCount(state,count){
            state.counter+=count
        }
	},
    actions:{
        //定义一个异步方法
        aIncrement(context){
            setTimeout({
                context.commit('increment')
            },1000)  
        },
        //带参数的异步方法    
        aIncrement(context,payload){
            setTimeout({
                context.commit('increment')
            },1000) 
            console.log(payload)
        },
        //可以回调的异步方法（主要提示已经回调成功）
        aIncrement(context,payload){
            setTimeout({
                context.commit('increment')
            },1000) 
            console.log(payload)
        }    
    },
    getters:{
        //演示getter基本使用方法
        powerCounter(state){
            return state.counter * state.counter
        },
        //getter作为参数添加到方法中
        more20stuLength(state,getter){
            return getter.more20stu.length
        },
        //用户传入的参数,通过构建函数处理
        moreAgeStu(state){
            // return function(age){
            //   return state.students.filter(s => s.age > age)
            // }
            return age =>{
              return  state.students.filter(s => s.age > age)
            }
        }    
    },
    modules:{}    
})
//3.导出store对象
export default store

//compent action使用
//普通提交风格
{{this.$store.dispath('aIncrement')}}
//传参
this.$store.dispath('aIncrement','参数')
//回调风格


```



##### 5.module

vue使用单一状态树，那么也意味着很多状态都会交给vuex来管理，当应用变得非常复杂时，store对就有可能变得相当臃肿。为了解决这个问题，vuex永续我们将store分割成module，而每个模块都拥有自己的state、mutaition、action、getters等

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

//1.安装插件
Vue.use(Vuex)
//2.创建对象
const moduleA = {
    state：{
    name:'张三',
    mutations: {},
    actions:{},
    getters:{}
}
}
const store = new Vuex.Store({
    state:{
        connter:1000
    },
    mutations：{
    	udpateName(sate,payload)
    		state.name = payload
		}
	},
    actions:{
        aupdateName(context){
            setTimeout(() => {
                context.commit('updateName','wangwu')
            },1000)
        }
    },
    getters:{
        fullname(state){
            return state.name +'111'
        },
        fullname2(state,getter){
            return getters.fullname+'2222'
        },
        // 引用根模块操作，rootstate表示跟目录
        fullname3(state,getter,rootstate){
            return getters.fullname2+rootstate.connter
        }
    },
    modules:{
        a: moduleA
    }    
})
//3.导出store对象
export default store

//module 使用
{{this.$store.state.a.name}}
//module mutation
{{this.$store.commit('updateName','lisi')}}
//module getter
{{this.$store.getters.fullname}}
//异步修改
this.$store.dispath('aupdateName')


```











