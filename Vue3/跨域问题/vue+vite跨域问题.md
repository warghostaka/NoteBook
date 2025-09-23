# vue3 + vite 跨域问题

---

项目中遇到了跨域问题，（虽然后端还没有运行），但是还是找资料解决了前端处理跨域的问题

参考文章：[*vue3+vite 项目跨域配置（踩坑无数篇）_vue3vite4 配置变量跨域*](file://D:\Workspace\NoteBook\VueNote\跨域问题\simpread-vue3+vite 项目跨域配置（踩坑无数篇）_vue3vite4 配置变量跨域 - CSDN 博客.html)

---

## vite.config.js 配置

根据文章内容，修改了vite.config.js文件配置
**配置 Vite 开发服务器的代理（proxy）功能，用于解决开发环境下的跨域问题**

**changOrigin 就是允许跨域,rewrite就是在这个路径上遇到/api的他就会就此路径上的/api替换为空字符串.**

```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  server: {
    host: '0.0.0.0',
    port: 5173,		//前端运行接口

    proxy: {
      '/api': {		//代理路径前缀
        target: 'http://127.0.0.1:8360',		//后端地址
		//ws: true,				//代理 WebSocket 请求。true 表示代理 WebSocket 请求
		changeOrigin: true,		// 允许跨域
		rewrite: (path) => path.replace(/^\/api/, ''),		//去掉路径中的 /api 前缀
      },
    },
  },

})
```

### 代码解析

1. ##### **前端端口**：

	* `port: 5173`：指定前端开发服务器运行在 `5173` 端口。

2. ##### **代理配置**：

	* `'/api'`：这是一个代理路径前缀。你可以根据项目需求修改这个前缀（例如 `/backend` 或其他）。
	* `target: 'http://localhost:8360'`：指定后端服务的地址。这里假设后端运行在 `localhost:8360`。
	* `changeOrigin: true`：允许跨域请求。
	* `rewrite: (path) => path.replace(/^\/api/, '')`：将请求路径中的 `/api` 前缀去掉。例如，如果你请求 `/api/user`，实际会转发到 `http://localhost:8360/user`。

3. ##### **使用示例**：

	假设你在前端代码中发起如下请求：

	```js
	axios.get('/api/user')
	  .then(response => {
	    console.log(response.data);
	  });
	```

	* 这个请求会被 Vite 代理拦截，并转发到 `http://localhost:8360/user`。
	* 由于 `rewrite` 函数的作用，`/api` 前缀会被去掉

## utils/request.js 配置

一般来说，在调用函数中，加上你上面配置的前缀就完成了

参考文章中解决的是第三方接口存在的跨域问题，因此直接在第三方接口调用函数前加上前缀testaxios问题解决

我的项目中解决的是接口不同造成的不同源跨域问题，因此直接修改utils/request.js文件中的baseURL，相当于在所有的调用函数前加上前缀`/api`

```js
import axios from 'axios';
import useToken from '../stores/token';
import router from '../router';
import { showLoadingToast, showToast, closeToast } from 'vant';

const service = axios.create({ baseURL :'/api'})
```

