# Vue3学习

 Vue是前端优秀框架， 是一套用于构建用户界面的**渐进式框架**

## 1、Vue开发前准备

### 安装Vue工具

`Vue CLI` Vue.jS 开发的标准工具，`Vue CLI `是一个基于Vue.js进行快速开发的完整系统

 ``` shell
 npm install -g @vue/cli
 ```

安装之后，你就可以在命令行中访问 ` vue `命令。你可以通过简单运行` vue `，看看是否展示出了一份所有可用命令的帮助信息，来验证它是否安装成功。

```shell
vue --version
```

### 创建一个项目

运行一下命令创建一个新项目

``` shell
vue create vue-demo
```

>**温馨提示**
>
>在控制台中，可以用上下按键调整选择项
>
>在控制台中，可以用空格(spacebar)选择是否选中和取消选中

设置选择“手动选择特效” 来选取需要的特效。

​	选择3·13·1·1·N

### 运行项目

第一步：进入项目根目录` cd vue-demo`

第二步：运行`npm run server`启动项目

### 安装Vue高亮插件

VSCode中安装 `vetur` 或者 `volar` 都可，前者针对Vue2版本，后者针对Vue3版本



## 2、模板语法

### 文本

数据绑定最常见的形式就是使用“{{Mustache}}” (双大括号) 语法的文本插值

```  html
<span>Message: {{ msg }}</span>
```

一般配合 `js` 中的 `data()` 设置数据

``` javascript
export default {
  name: 'HelloWorld',
  data(){
    return{
      msg:"消息提示"
   }
 }
}
```

### 原始 HTML

双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 `v-html `指令

```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span vhtml="rawHtml"></span></p>
```

``` javascript
data(){
    return{
        rawHtml:"<a href='https://www.itbaizhan.com'>百战</a>"
   }
}
```

> **提示**
>
> *文本*相当于`innerText`
>
> *原始HTML*相当于`innerHtml`

### 属性 Attribute

Mustache 语法不能在 HTML 属性中使用，然而，可以使用`v-bind`指令

``` html
<div v-bind:id="dynamicId"></div>
```

``` javascript
data(){
    return{
        dynamicId:1001
   }
}
```

> **温馨提示**
>
> `v-bind:`可以简写成`:`

### 使用 JavaScript 表达式

在我们的模板中，我们一直都只绑定简单的 property 键值，Vue.js都提供了完全的 JavaScript 表达式支持

```
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}
```

这些表达式会在当前活动实例的数据作用域下作为 JavaScript 被解析。有个限制就是，每个绑定都只能包含**单个表达式**，所以下面的例子都**不会**生效。

```
<!-- 这是语句，不是表达式：-->
{{ var a = 1 }}

<!-- 流程控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```



## 3、条件渲染

### v-if

`v-if`指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回`true`值的时候被渲染。

``` html
<p v-if="flag">我是孙猴子</p>
```

``` javascript
data() {
    return {
        flag: true
   }
}
```

### v-else

你可以使用` v-else` 指令来表示` v-if `的“else 块”

``` html
<p v-if="flag">我是孙猴子</p>
<p v-else>你是傻猴子</p>
```

```javascript
data() {
    return {
        flag: false
   }
}
```

### v-show

另一个用于条件性展示元素的选项是 `v-show `指令

``` html
<h1 v-show="ok">Hello!</h1>
```

### v-if与v-show的区别

`v-if `是“真正”的条件渲染，因为它会确保在切换过程中，条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if `也是**惰性的**：如果在初始渲染时条件为假，则什么也不做，直到条件第一次变为真时，才会开始渲染条件块。

相比之下， `v-show `就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，` v-if `有更高的**切换开销**，而`v-show `有更高的**初始渲染开销**。因此，如果**需要非常频繁地切换，则使用 `v-show` 较好**；如果在**运行时条件很少改变，则使用` v-if` 较好**



## 4、列表渲染

## 5、事件处理

## 6、表单输入绑定

## 7、组件基础

## 8、Props组件交互

## 9、Vue引入第三方

## 10、Axios网络请求

### 网络请求跨域解决方案

## 11、Vue引入路由器配置

## 12、Vue状态管理(Vuex)

### Vue状态管理核心(Vuex)

## 13、Vue3新特性

## 14、Vue3加载Element-plus