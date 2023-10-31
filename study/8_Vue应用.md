## Element Plus

网址：https://element-plus.org/zh-CN/guide/installation.html

### 安装

```
# NPM
$ npm install element-plus --save
```

### 用法

#### 完整引入

如果你对打包后的文件大小不是很在乎，那么使用完整导入会更方便。

```
// main.ts
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'

const app = createApp(App)

app.use(ElementPlus)
app.mount('#app')
```

## 全局配置

在引入 ElementPlus 时，可以传入一个包含 `size` 和 `zIndex` 属性的全局配置对象。 `size` 用于设置表单组件的默认尺寸，`zIndex` 用于设置弹出组件的层级，`zIndex` 的默认值为 `2000`。

完整引入：

```
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import App from './App.vue'

const app = createApp(App)
app.use(ElementPlus, { size: 'small', zIndex: 3000 })
```

### Icon 图标

#### 安装

##### 使用包管理器

```

# NPM
$ npm install @element-plus/icons-vue
```

#### 注册所有图标

您需要从 `@element-plus/icons-vue` 中导入所有图标并进行全局注册。

```
// main.ts

// 如果您正在使用CDN引入，请删除下面一行。
import * as ElementPlusIconsVue from '@element-plus/icons-vue'

const app = createApp(App)
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
  app.component(key, component)
}
```

### input使用图标

要在输入框中添加图标，你可以简单地使用 `prefix-icon` 和 `suffix-icon` 属性。

```
<template>
	<el-input
        v-model="input2"
        class="w-50 m-2"
        placeholder="Type something"
        :prefix-icon="Search"
      />
  </template>
  <script setup>
import { Calendar, Search } from '@element-plus/icons-vue'

</script>    
```

## Axios

网址：https://www.axios-http.cn/docs/intro

### 安装

使用 npm:

```bash
$ npm install axios
```

### 使用

```vue
<script setup>
    //局部引入
import  axios from "axios";
const user = reactive({
    username:'',
    password:''
});
function login(){
    axios.post('http://localhost:9999/rbac/user/login', user)
    .then(function (response) {
        console.log(response);
    })
    .catch(function (error) {
        console.log(error);
    });
}
```

### 封装

创建axiosApi.js

```js
import  axios from "axios"
const instance = axios.create({
    baseURL: 'http://localhost:9999',//请求根路径
    timeout: 10000  //超时时间 10秒
  });
  // 添加请求拦截器
instance.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
instance.interceptors.response.use(function (response) {
    // 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 超出 2xx 范围的状态码都会触发该函数。
    // 对响应错误做点什么
    return Promise.reject(error);
  });
//暴露
  export default instance;
```

使用（登录案例）

```vue
<template>
    <div class="login">
    <el-form :model="user"  class="login-form">
      <h1 class="title">用户登录</h1>
      <el-form-item label="">
        <el-input
          v-model="user.username"
          placeholder="请输入账号"
          :prefix-icon="User"
        >
        </el-input>
      </el-form-item>
      <el-form-item label="">
        <el-input
          v-model="user.password"
          placeholder="请输入密码"
          :prefix-icon="Lock"
          show-password
        >
        </el-input>
    </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="login">登录</el-button>
      </el-form-item>
      
    </el-form>
  </div>
</template>

<script setup>
import { reactive } from "vue"
import  instance from "@/api/axiosApi"
import { User, Lock } from '@element-plus/icons-vue'
import { ElMessage } from 'element-plus'
import router from '@/router'
const user = reactive({
    username:'',
    password:''
});
function login(){
    // instance.request({method: 'post',url:'/rbac/user/login',data:user})
    instance.post('/rbac/user/login', user)
    .then(res =>{
        if(res.data.code === 200){
            //跳转home
            router.push("/");
        }else{
            ElMessage(res.data.msg);
        }
    })
    .catch(function (error) {
        ElMessage(error);
    });
}

</script>

<style scoped>

.login {
display: flex;
  justify-content: center;
  align-items: center; 
  height: 100%;

}
 .title {
   margin: 0px auto 30px auto;
  text-align: center;
  color: #707070;
}

.login-form {
  border-radius: 6px;
  background: #ffffff;
  width: 350px;
  padding: 25px 25px 5px 25px;
 
}
button{
    width: 100%; 
}

</style>
```

## Pinia

官网：https://pinia.vuejs.org/zh/introduction.html

### 安装

```

# 使用 npm
npm install pinia
```

创建一个 pinia 实例 (根 store) 并将其传递给应用：

js

```
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'

const pinia = createPinia()
const app = createApp(App)

app.use(pinia)
app.mount('#app')
```

### 使用

创建store文件夹，创建storeApi.js

```
import { defineStore } from 'pinia'

export const tokenStore = defineStore('token', {
    //一个返回初始状态的函数
    state: () => ({ token: null }),
    //相当于组件中的 method
    actions: {
      updateToken(token) {
        this.token = token;
      },
    },
  })
```

登录保存token

```vue
<template>
    <div class="login">
    <el-form :model="user"  class="login-form">
      <h1 class="title">用户登录</h1>
      <el-form-item label="">
        <el-input
          v-model="user.username"
          placeholder="请输入账号"
          :prefix-icon="User"
        >
        </el-input>
      </el-form-item>
      <el-form-item label="">
        <el-input
          v-model="user.password"
          placeholder="请输入密码"
          :prefix-icon="Lock"
          show-password
        >
        </el-input>
    </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="login">登录</el-button>
      </el-form-item>
      
    </el-form>
  </div>
</template>

<script setup>
import { reactive } from "vue"
import  instance from "@/api/axiosApi"
import { User, Lock } from '@element-plus/icons-vue'
import { ElMessage } from 'element-plus'
import router from '@/router'
import { tokenStore } from '@/store/storeApi'
const user = reactive({
    username:'',
    password:''
});
function login(){
    // instance.request({method: 'post',url:'/rbac/user/login',data:user})
    instance.post('/rbac/user/login', user)
    .then(res =>{
        if(res.data.code === 200){
            //保存token
            tokenStore().updateToken(res.data.res);
            //跳转home
            router.push("/");
        }else{
            ElMessage(res.data.msg);
        }
    })
    .catch(function (error) {
        ElMessage(error);
    });
}

</script>

<style scoped>

.login {
display: flex;
  justify-content: center;
  align-items: center; 
  height: 100%;

}
 .title {
   margin: 0px auto 30px auto;
  text-align: center;
  color: #707070;
}

.login-form {
  border-radius: 6px;
  background: #ffffff;
  width: 350px;
  padding: 25px 25px 5px 25px;
 
}
button{
    width: 100%; 
}

</style>
```

拦截器添加token到header

```js
import  axios from "axios"
import { tokenStore } from '@/store/storeApi'

const instance = axios.create({
    baseURL: 'http://localhost:9999',//请求根路径
    timeout: 10000  //超时时间 10秒
  });
  // 添加请求拦截器
instance.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    console.log('instance.interceptors.request');
    console.log(tokenStore().token);
    //token放入header
    config.headers['Authorization-Token'] = tokenStore().token;
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
instance.interceptors.response.use(function (response) {
    // 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 超出 2xx 范围的状态码都会触发该函数。
    // 对响应错误做点什么
    return Promise.reject(error);
  });


  //暴露
  export default instance;
```

### 刷新token丢失

App.vue

```vue
<script setup>
import { RouterLink, RouterView } from 'vue-router'
// import HelloWorld from './components/HelloWorld.vue'
import { tokenStore } from '@/store/storeApi'
let store = tokenStore();
  
    if (sessionStorage.getItem("store")) {
          store.updateToken(sessionStorage.getItem("store"))
     }

     //在页面刷新时将vuex里的信息保存到sessionStorage里
     window.addEventListener("beforeunload",()=>{
      console.log('刷新页面')
        sessionStorage.setItem("store",store.token)
    })
</script>
```

登录token有值，报403

第一种方式：请求拦截器将不需要验证的url的token设为null

```js

import axios from 'axios'
import { tokenStore } from '@/store/storeApi'
import router from '@/router'

const instance = axios.create({
    baseURL: 'http://localhost:9999',//请求根路径
    timeout: 10000//超时时间
  });

  // 添加请求拦截器
  instance.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    //判断是否需要token
    if(config.url =='/rbac/user/login')
    config.headers['Authorization-Token'] = null;
    //将token添加到header
    else
    config.headers['Authorization-Token'] = tokenStore().token;
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
instance.interceptors.response.use(function (response) {
    // 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 超出 2xx 范围的状态码都会触发该函数。
    // 对响应错误做点什么
    console.log('error');
    console.log(error.response.status);
    //没有权限
    if(error.response.status == 403){
        //跳转到登录页
        router.push('/login');
    }
    return Promise.reject(error);
  });
  //暴露
  export  default instance;
```

第二种方式：在router文件夹下的index.js中添加路由守卫

```js
router.beforeEach((to, from, next) => {
  const isAuthenticated = tokenStore().token == null ? false:true;
  //访问登录页 token赋值null
  if(to.name === 'Login'){
    tokenStore().token = null;
  }
  //访问非登录页且token是null
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  else next()
})
```

