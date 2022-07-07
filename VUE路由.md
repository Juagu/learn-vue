### VUE路由

------



#### vue路由传参方式

------



#### $router 和$route区别

------

$router是全局的路由，他是包含vue路由对象，包含vue路由所有的方法。

$route是当前激活的路由，可以拿到当前路由的参数信息。

#### 导航守卫

------

导航守卫:



##### 全局导航守卫

------

​    

  ```javascript
  //路由配置代码index.js
  // 创建VueRouter对象
  const routes = [
    {
      path: '',
      // redirect重定向
      redirect: '/home',
    },
    {
      path: '/home',
      component: Home,
      mate: {
        title:'首页'
      }
    },
    {
      path: '/about',
      component: About,
      mate: {
          title:'关于'
      }
    }
  ]
  //index.js
  //路由导航 前置钩子 当路由跳转之前回调
  router.beforeEach((to,from,next) => {
      //路由从from跳转到to
      //matched 在to对象里面，可以console.log(to)来查找一下
      document.title = to.matched[0].meta.title
      //next方法必须写，不然路由无法下一步跳转
      next()
  })
  //后置钩子 当路由跳转之后回调
  router.afterEach((to,from)=>{
      
  })
  
  ```

#####   路由独享守卫

------



  ```javascript
  const routes = [
    {
      path: '/users/:id',
      component: UserDetails,
      beforeEnter: (to, from) => {
        // reject the navigation
        return false
      },
    },
  ]
  ```
##### keep-alive 

------

keep-alive是vue内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。

属性：

include	字符串或者正则表达，只有匹配的组件会被缓存

exclude	字符串或者正则表达，任何匹配的组件都不会被缓存

```javascript
<keep-alive>
    <router-viwe/>
</keep-alive>
//这两个函数，只有该组件被保持了状态使用了keep-alive时，才会生效
activated() {},
deactivated() {}，
```

如何记录用户使用状态：

1、通过kepp-alive中的activated,deactivated函数

2、首页中使用path属性记录离开时的路径，在beforeRouteLeave中将当前路由赋值给path    





### 文件路径引用问题

在bulid文件夹下找到webpack.base.conf.js文件,修改resolve下面内容

```javascript
resolve:{
    extesions:['.js','.vue','.json'],
    alias: {
        '@':resolve('src'),
        'assets':resolve('src/assets'),
        'components':resolve('src/components'),
        'views':resolve('src/views'),
    }
}
```

引用方式：

js: import xx from 'views/xxx'

标签：~views/xxx