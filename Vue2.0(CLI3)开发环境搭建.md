## Vue2.0(CLI3)开发环境搭建

### 一、创建项目并提交到github，方法一：

##### 1、创建项目

~~~powershell
vue create supermall
~~~

##### 2、创建名为supermall的github的仓库(repository，注：这里需要创建类型为MIT的License)

##### 3、暂时改项目名supermall为supermall1

##### 4、从github克隆新建的项目到本地

~~~powershell
git clone https://github.com/alanlou2020/supermall.git
~~~

##### 5、拷贝选中的内容到supermall中，注意：sumpermall中新生成的文件夹不能覆盖，node_modules文件夹无需复制，如下图：

![image-20210124105146335](../assets/Vue2.0(CLI3)开发环境搭建/image-20210124105146335.png)

##### 6、查看supermall项目git提交状态，执行命令如下：

~~~powershell
git status
~~~

##### 7、添加新增的内容到git

~~~powershell
git add .
~~~

##### 8、本地提交修改

~~~powershell
git commit -m '初始化项目'
~~~

##### 9、提交到github服务器

~~~powershell
git push
~~~

### 二 、项目直接和创建的github关联，方法二：

~~~powershell
git init
git remote add origin https://github.com/alanlou2020/supermall.git
git push -u origin master
~~~

### 三、安装npm(因为复制的时候并没有复制node_modules文件夹，使用方法二的无需执行)，命令如下：

~~~powershell
npm install
~~~

### 四、划分目录结构

##### 1、删除不需要的文件

* src\components\HelloWorld.vue
* src\assets\logo.png

##### 2、app.vue文件修改为如下：

~~~vue
<template>
  <div id="app">
    
  </div>
</template>

<script>

  export default {
    name: 'app',
    components: {
    }
  }

</script>

<style>

</style>

~~~

##### 3、创建资源文件夹

* src\assets\img
* src\assets\css
* src\views
* src\views\home
* src\views\category
* ...
* src\components\common
* src\components\content
* src\router
* src\store
* src\network
* src\common
* src\common\const.js（存放公共的js常量）
* src\common\utils.js（存放公共的js方法）
* src\common\mixin.js（混入）

### 五、引入CSS

##### 1、引入通用CSS：https://github.com/necolas/normalize.css，内容如下：

```css
/*! normalize.css v8.0.1 | MIT License | github.com/necolas/normalize.css */

/* Document
   ========================================================================== */

/**
 * 1. Correct the line height in all browsers.
 * 2. Prevent adjustments of font size after orientation changes in iOS.
 */

html {
  line-height: 1.15; /* 1 */
  -webkit-text-size-adjust: 100%; /* 2 */
}

/* Sections
   ========================================================================== */

/**
 * Remove the margin in all browsers.
 */

body {
  margin: 0;
}

/**
 * Render the `main` element consistently in IE.
 */

main {
  display: block;
}

/**
 * Correct the font size and margin on `h1` elements within `section` and
 * `article` contexts in Chrome, Firefox, and Safari.
 */

h1 {
  font-size: 2em;
  margin: 0.67em 0;
}

/* Grouping content
   ========================================================================== */

/**
 * 1. Add the correct box sizing in Firefox.
 * 2. Show the overflow in Edge and IE.
 */

hr {
  box-sizing: content-box; /* 1 */
  height: 0; /* 1 */
  overflow: visible; /* 2 */
}

/**
 * 1. Correct the inheritance and scaling of font size in all browsers.
 * 2. Correct the odd `em` font sizing in all browsers.
 */

pre {
  font-family: monospace, monospace; /* 1 */
  font-size: 1em; /* 2 */
}

/* Text-level semantics
   ========================================================================== */

/**
 * Remove the gray background on active links in IE 10.
 */

a {
  background-color: transparent;
}

/**
 * 1. Remove the bottom border in Chrome 57-
 * 2. Add the correct text decoration in Chrome, Edge, IE, Opera, and Safari.
 */

abbr[title] {
  border-bottom: none; /* 1 */
  text-decoration: underline; /* 2 */
  text-decoration: underline dotted; /* 2 */
}

/**
 * Add the correct font weight in Chrome, Edge, and Safari.
 */

b,
strong {
  font-weight: bolder;
}

/**
 * 1. Correct the inheritance and scaling of font size in all browsers.
 * 2. Correct the odd `em` font sizing in all browsers.
 */

code,
kbd,
samp {
  font-family: monospace, monospace; /* 1 */
  font-size: 1em; /* 2 */
}

/**
 * Add the correct font size in all browsers.
 */

small {
  font-size: 80%;
}

/**
 * Prevent `sub` and `sup` elements from affecting the line height in
 * all browsers.
 */

sub,
sup {
  font-size: 75%;
  line-height: 0;
  position: relative;
  vertical-align: baseline;
}

sub {
  bottom: -0.25em;
}

sup {
  top: -0.5em;
}

/* Embedded content
   ========================================================================== */

/**
 * Remove the border on images inside links in IE 10.
 */

img {
  border-style: none;
}

/* Forms
   ========================================================================== */

/**
 * 1. Change the font styles in all browsers.
 * 2. Remove the margin in Firefox and Safari.
 */

button,
input,
optgroup,
select,
textarea {
  font-family: inherit; /* 1 */
  font-size: 100%; /* 1 */
  line-height: 1.15; /* 1 */
  margin: 0; /* 2 */
}

/**
 * Show the overflow in IE.
 * 1. Show the overflow in Edge.
 */

button,
input { /* 1 */
  overflow: visible;
}

/**
 * Remove the inheritance of text transform in Edge, Firefox, and IE.
 * 1. Remove the inheritance of text transform in Firefox.
 */

button,
select { /* 1 */
  text-transform: none;
}

/**
 * Correct the inability to style clickable types in iOS and Safari.
 */

button,
[type="button"],
[type="reset"],
[type="submit"] {
  -webkit-appearance: button;
}

/**
 * Remove the inner border and padding in Firefox.
 */

button::-moz-focus-inner,
[type="button"]::-moz-focus-inner,
[type="reset"]::-moz-focus-inner,
[type="submit"]::-moz-focus-inner {
  border-style: none;
  padding: 0;
}

/**
 * Restore the focus styles unset by the previous rule.
 */

button:-moz-focusring,
[type="button"]:-moz-focusring,
[type="reset"]:-moz-focusring,
[type="submit"]:-moz-focusring {
  outline: 1px dotted ButtonText;
}

/**
 * Correct the padding in Firefox.
 */

fieldset {
  padding: 0.35em 0.75em 0.625em;
}

/**
 * 1. Correct the text wrapping in Edge and IE.
 * 2. Correct the color inheritance from `fieldset` elements in IE.
 * 3. Remove the padding so developers are not caught out when they zero out
 *    `fieldset` elements in all browsers.
 */

legend {
  box-sizing: border-box; /* 1 */
  color: inherit; /* 2 */
  display: table; /* 1 */
  max-width: 100%; /* 1 */
  padding: 0; /* 3 */
  white-space: normal; /* 1 */
}

/**
 * Add the correct vertical alignment in Chrome, Firefox, and Opera.
 */

progress {
  vertical-align: baseline;
}

/**
 * Remove the default vertical scrollbar in IE 10+.
 */

textarea {
  overflow: auto;
}

/**
 * 1. Add the correct box sizing in IE 10.
 * 2. Remove the padding in IE 10.
 */

[type="checkbox"],
[type="radio"] {
  box-sizing: border-box; /* 1 */
  padding: 0; /* 2 */
}

/**
 * Correct the cursor style of increment and decrement buttons in Chrome.
 */

[type="number"]::-webkit-inner-spin-button,
[type="number"]::-webkit-outer-spin-button {
  height: auto;
}

/**
 * 1. Correct the odd appearance in Chrome and Safari.
 * 2. Correct the outline style in Safari.
 */

[type="search"] {
  -webkit-appearance: textfield; /* 1 */
  outline-offset: -2px; /* 2 */
}

/**
 * Remove the inner padding in Chrome and Safari on macOS.
 */

[type="search"]::-webkit-search-decoration {
  -webkit-appearance: none;
}

/**
 * 1. Correct the inability to style clickable types in iOS and Safari.
 * 2. Change font properties to `inherit` in Safari.
 */

::-webkit-file-upload-button {
  -webkit-appearance: button; /* 1 */
  font: inherit; /* 2 */
}

/* Interactive
   ========================================================================== */

/*
 * Add the correct display in Edge, IE 10+, and Firefox.
 */

details {
  display: block;
}

/*
 * Add the correct display in all browsers.
 */

summary {
  display: list-item;
}

/* Misc
   ========================================================================== */

/**
 * Add the correct display in IE 10+.
 */

template {
  display: none;
}

/**
 * Add the correct display in IE 10.
 */

[hidden] {
  display: none;
}
```

##### 2、建立自己的CSS文件，内容如下：

~~~css
@import "./normalize.css";

/*:root -> 获取根元素html*/
:root {
  --color-text: #666;
  --color-high-text: #ff5777;
  --color-tint: #ff8198;
  --color-background: #fff;
  --font-size: 14px;
  --line-height: 1.5;
}

*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Helvetica Neue",Helvetica,"PingFang SC","Hiragino Sans GB","Microsoft YaHei","微软雅黑",Arial,sans-serif;
  user-select: none; /* 禁止用户鼠标在页面上选中文字/图片等 */
  -webkit-tap-highlight-color: transparent; /* webkit是苹果浏览器引擎，tap点击，highlight背景高亮，color颜色，颜色用数值调节 */
  background: var(--color-background);
  color: var(--color-text);
  /* rem vw/vh */
  width: 100vw;
}

a {
  color: var(--color-text);
  text-decoration: none;
}


.clear-fix::after {
  clear: both;
  content: '';
  display: block;
  width: 0;
  height: 0;
  visibility: hidden;
}

.clear-fix {
  zoom: 1;
}

.left {
  float: left;
}

.right {
  float: right;
}
~~~

##### 3、App.vue中引入base.css，如下：

```vue
<style>
  @import "./assets/css/base.css";
</style>
```

### 六、配置别名

##### 1、在项目根目录(supermall)创建vue.config.js文件，内容如下：

~~~vue
module.exports = {
    configureWebpack: {
        resolve: {
            alias: {
                'assets': '@/assets',
                'common': '@/common',
                'components': '@/components',
                'network': '@/network',
                'views': '@/views'
            }
        }
    }
}
~~~

注：router和store都可以通过`this.$router`和`this.$store`来获取，因此无需定义别名。

##### 2、在项目中使用别名，如：在"src\App.vue"中，修改'@import "./assets/css/base.css";'为'@import "assets/css/base.css";'。

### 七、创建项目统一风格配置文件“.editorconfig”，路径为项目根目录下(supermall)，内容如下(重要)：

~~~vue
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
~~~

### 八、搭建完成，启动项目，命令如下：

~~~powershell
npm run serve
~~~

### 九、配置路由：

##### 1、安装路由，命令如下：

~~~powershell
npm install vue-router --save
~~~

##### 2、创建文件src\router\index.js，内容如下：

```vue
import Vue from 'vue'
import VueRouter from 'vue-router'

const Home = () =>
    import ('views/home/Home')
const Category = () =>
    import ('views/category/Category')
const Cart = () =>
    import ('views/cart/Cart')
const Profile = () =>
    import ('views/profile/Profile')


//1、安装插件
Vue.use(VueRouter)


// 解决ElementUI导航栏中的vue-router在3.0版本以上重复点菜单报错问题
const originalPush = VueRouter.prototype.push;
VueRouter.prototype.push = function push(location) {
    return originalPush.call(this, location).catch(err => err)
};
// 解决ElementUI导航栏中的vue-router在3.0版本以上重复点菜单报错问题
const originalReplace = VueRouter.prototype.replace;
VueRouter.prototype.replace = function replace(location) {
    return originalReplace.call(this, location).catch(err => err);
};

//2、创建router
const routes = [{
        path: '',
        redirect: '/home'
    },
    {
        path: '/home',
        component: Home
    },
    {
        path: '/category',
        component: Category
    },
    {
        path: '/cart',
        component: Cart
    },
    {
        path: '/profile',
        component: Profile
    }
]

const router = new VueRouter({
    routes,
    mode: 'history'
})

//3、导出
export default router
```

##### 3、修改"src\main.js"文件，内容如下：

~~~vue
import Vue from 'vue'
import App from './App.vue'
import router from './router'

Vue.config.productionTip = false

new Vue({
    render: h => h(App),
    router
}).$mount('#app')
~~~

##### 4、修改文件"src\App.vue"，内容如下：

~~~vue
<template>
  <div id="app">
    <router-view />
  </div>
</template>

<script>
 
  export default {
    name: 'app',
    components: {
    }
  }

</script>

<style>
  @import "assets/css/base.css";
</style>
~~~





