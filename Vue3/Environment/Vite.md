# Vite相关配置

参考文章：*[Vite是什么？怎样使用Vite创建Vue3项目？【详细教程】](https://blog.csdn.net/zy1992As/article/details/133610708)*

---

**Vite提供了两种创建项目的命令**

- 手动创建项目的命令

- 通过模板自动创建项目的命令

---

## 手动创建项目

使用npm或yarn包管理工具都可以搭配Vite手动创建项目，具体命令如下。

```shell
# 使用npm create命令创建项目
npm create vite@latest
# 使用yarn create命令创建项目
yarn create vite
```

上述命令展示了两种包管理工具用于创建Vite项目，在使用时任选其一即可。npm create和yarn create命令后跟一个vite包名，表示初始化Vite。vite@latest表示在 npm中安装最新版本的Vite

---

### yarn 命令创建项目时报错

参考文章：[*创建vite错误*](https://blog.csdn.net/weixin_43824526/article/details/121319955?ops_request_misc=&request_id=&biz_id=102&utm_term=yarn%E7%9A%84%E5%AE%89%E8%A3%85%E5%8C%85%E9%BB%98%E8%AE%A4%E6%98%AF%E5%9C%A8c%E7%9B%98%E7%9A%84%E8%80%8C%E6%88%91&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-121319955.142^v101^pc_search_result_base7&spm=1018.2226.3001.4187)

#### 报错方式

![image-20250102183508838](D:\Workspace\NoteBook\VueNote\Environment\assets\image-20250102183508838.png)

#### 错误原因

yarn的安装包默认是在c盘的而我yarn安装在D盘的所以就会报这样的错误！

使用命令查看位置

```python
yarn global dir
```

**查到**

![image-20250102183656499](.\assets\image-20250102183656499.png)

### 解决办法

1. 将yarn的全局路径改到D盘就行了，在D盘创建yarn文件夹在文件下创建一个golbal和cache文件夹
2. 执行下列命令，把`yarn`装在`node_modules`

```
yarn config set global-folder "D:\Node\node_modules\yarn\gloabl"
yarn config set cache-folder "D:\Node\node_modules\yarn\cache"
```

---

执行`yarn create vite`命令后，Vite会提示填写项目名称

**选择前端框架**

<img src=".\assets\image-20250102185631792.png" alt="image-20250102185631792" style="zoom:67%;" />

**选择语法类型**

<img src=".\assets\image-20250102185751110.png" alt="image-20250102185751110" style="zoom: 80%;" />

**现在可以根据提示运行代码**

![image-20250102185835238](.\assets\image-20250102185835238.png)

上述命令中，yarn dev命令是Vue3项目中package.json文件里面scripts节点配置的命令。除了yarn dev命令外，还有2个常用命令yarn build和yarn preview，它们分别表示构建生产环境项目和构建本地预览环境项目。这3个命令实际上都是别名，是为了便于开发人员记忆。当执行这3个命令时，yarn会读取当前项目的package.json文件中的命令配置，找到真正的命令并执行。

Vue 3项目的`package.json`文件中的命令配置如下

```
"scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
```

上述配置中，dev是vite的别名，build是vite build的别名，preview是vite preview的别名。也就是说，当执行yarn dev时，实际执行的命令是yarn vite。别名可以自定义，如果修改了别名，在执行命令时需要使用修改后的别名

#### 运行项目

<img src=".\assets\image-20250102190819028.png" alt="image-20250102190819028" style="zoom: 80%;" />

<img src=".\assets\image-20250102190904856.png" alt="image-20250102190904856" style="zoom: 50%;" />

---

## 通过模板自动创建项目

参考文章：*[Vite是什么？怎样使用Vite创建Vue3项目？【详细教程】](https://blog.csdn.net/zy1992As/article/details/133610708)*

---

## Vue 3项目的目录结构

Vue 3项目创建成功后，使用VS Code编辑器打开项目目录，可以看到一个默认生成的项目目录结构，如右图所示。

![image-20250102191944517](.\assets\image-20250102191944517.png)

下面简要介绍Vue 3项目的目录结构中各个目录和文件的作用，具体如下。

- `vscode`：存放VS Code编辑器的相关配置。

- `node_modules`：存放项目的各种依赖和安装的插件。

- `public`：存放不可编译的静态资源文件，当进行项目构建时，该目录下的文件会被复制到dist目录，该目录下的文件需要使用绝对路径访问。

- `src`：源代码目录，保存开发人员编写的项目源代码。

- `src\assets`：存放可编译的静态资源文件，例如图片、样式文件等。该目录下的文件需要使用相对路径访问。

-  `src\components`：存放单文件组件，即.vue文件。

-  `src\components\HelloWorld.vue`：一个名称为HelloWorld的单文件组件。

-  `src\App.vue`：项目的根组件。

-  `src\main.js`：项目的入口文件，用于创建Vue应用实例。

-  `src\style.css`：项目的全局样式表文件。

-  `gitignore`：向Git仓库上传代码时需要忽略的文件列表。

-  `index. html`：默认的主渲染页面文件，同时也是页面的入口文件。

-  `package.json`:包配置文件。

-  `README.md`：项目使用说明文件。

-  `vite.config.js`：存放Vite的相关配置。

-  `yarn.lock`：存储每一个依赖项的安装版本，在使用yarn安装、升级、卸载依赖时，会自动更新yarn.lock文件。

此外，当执行了yarn build命令后，在项目目录中会出现dist目录，该目录中保存的是项目构建后的文件。

---

## Vue 3项目的运行过程

使用Vite构建Vue 3项目后，当执行yarn dev命令启动服务时，项目就会运行起来，该项目会通过src\main.js文件将`src\App.vue`组件渲染到`index.html`文件的指定区域。

`src\App.vue`文件

Vue 3项目是由各种组件组成的，`src\App.vue`文件是项目的根组件。在根组件中可以引用其他组件，从而显示其他组件的内容。

`index.html`文件

`index.html`文件是页面的入口文件，该文件中预留了用于被`src\main.js`文件中的Vue应用实例所控制的区域。

`src\main.js`文件

`src\main.js`文件是项目的入口文件，该文件创建了Vue应用实例。Vue应用实例是Vue项目工作的基础，每个Vue项目都是从创建Vue应用实例开始的。
