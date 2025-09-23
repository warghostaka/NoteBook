# vue3+vite 项目跨域配置（踩坑无数篇）_vue3vite4 配置变量跨域 - CSDN 博客

---

写这篇多少有点心情复杂，毕竟因为一个巨巨巨巨没意思的 bug 卡了两整天…
 废话不多说啦，开篇入题叭，希望大家都能改好自己的 bugggggg！！！

---

## 1.vite.config.js 配置

注意：因为我是用 **vite** 创建的，不是 vue-cli，当时搜了好多教程都教的是新建一个 vue.config.js，发现根本没有生效，所以，如果使用 vite 创建的项目就在 **vite.config.js** 里面配置如下代码：

以我要访问的疫情数据 api 为例，原 api  地址：
`https://api.inews.qq.com/testaxios/newsqa/v1/automation/modules/list?modules=FAutoCountryConfirmAdd,WomWorld,WomAboard`

```js
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';

export default defineConfig({
  plugins: [vue()],
  server: {
    port: 3000,
    proxy: {
      '/testaxios': {
        target: 'https://api.inews.qq.com/',
        // target就是你要访问的目标地址，可以是基础地址，这样方便在这个网站的其他api口调用数据
        ws: true,
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/testaxios/, ''),
        // 要记得加rewrite这句
      },
    },
  },
})
```

---

## 2.api 文件

我写代码的时候，把 api 相关的调用函数封装在了一个文件里面，在 api/index.js 文件里，然后其他地方在用的时候就可以直接调用函数了。

除此之外，axios 的请求也被我封装起来了，copy 的网上的封装代码，也可以直接引入 axios，需要的话拿走就好了。

### api / index.js

```js
import axios from "../utils/requst"     
// import axios from "axios"
const api = {
    // 疫情数据
    getNcov(){
        return axios.get("testaxios/newsqa/v1/automation/modules/list?modules=FAutoCountryConfirmAdd,WomWorld,WomAboard")
    },
    // 城市数据
    getNcovCity(){
        return axios.get("testaxios/newsqa/v1/query/inner/publish/modules/list?modules=statisGradeCityDetail,diseaseh5Shelf")
    },
    getNcovCity2(){
        return axios.get("newsqa/v1/query/inner/publish/modules/list?modules=chinaDayList,chinaDayAddList,nowConfirmStatis,provinceCompare")
    }
}
export default api;
```

注意：这里前面只要加上你上面配置的前缀 testaxios（前面不用 / 了），后面跟着原地址的后半部分就欧克了！

### utils / requst.js

```js
import axios from "axios"
import qs from "querystring"


/**
 * 处理失败的方法
 * status:状态码
 * info:信息
 */
const errorHandle = (status,info) =>{
    switch(status){
        case 400:
            console.log("语义有误，当前请求无法被服务器理解。除非进行修改，否则客户端不应该重复提交这个请求。")
            break;
        case 401:
            // token:令牌
            console.log("服务器认证失败")
            break;
        case 403:
            console.log("服务器已经理解请求，但是拒绝执行它");
            break;
        case 404:
            console.log("请检查网络请求地址")
            break;
        case 500:
            console.log("服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理。一般来说，这个问题都会在服务器的程序码出错时出现。")
            break;
        case 502:
            console.log("作为网关或者代理工作的服务器尝试执行请求时，从上游服务器接收到无效的响应。")
            break;
        default:
            console.log(info)
            break;
    }
}



/**
 * 创建axios实例对象
 */

const instance = axios.create({
    // 公共配置
    // baseURL:"http://iwenwiki.com",
    timeout:8000
})

/**
 * 处理拦截器
 */

 /**
  * 请求拦截
  */
instance.interceptors.request.use(
    config => {
        if(config.method === "post"){
            config.data = qs.stringify(config.data)
        }
        return config
    },
    error => Promise.reject(error)
)

/**
 * 响应拦截
 */
instance.interceptors.response.use(
    // 完成了
    response => response.status === 200 ? Promise.resolve(response) : Promise.reject(response),
    error => {
        const { response } = error;
        errorHandle(response.status,response.info);
    }
)


export default instance
```

---

## 3. 调用 api 相关函数

我是在 home.vue 里面调用的，直接 import api 文件以后就可以直接调用函数了

```js
mounted(){
      api.getNcov().then(res=>{
        console.log(res.data)
      }).catch((error)=>{console.log(error)});
    }
```



没啦！！！！！！！！！！ 冲啊啊啊啊啊啊啊！！！！！！！！！