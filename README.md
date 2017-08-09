**Vue基础认识**
### 搭建环境
- 全局安装
> 这里我推荐大家使用淘宝镜像装，也就是cnpm；
```
cnpm install -g vue-cli
```
- 使用“webpack”样板创建一个项目

```
vue init webpack my-first-vue-project
```
- 进入项目目录

```
cd my-first-vue-project
```
- 运行

```
npm run dev
```
### 编写程序
> 在src目录下创建components文件夹，然后在components里面创建componentA.vue
- componentA.vue

```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h1>{{ msgfromfather }}</h1>
    <button v-on:click="onClickMe">open mouse!</button>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data: function () {
    return {
      msg: 'hellow from cpmponent a '
    }
  },
  props: ['msgfromfather'],
  events: {
      'onAddnew': function (items) {
          console.log(items);
      }
  },
  methods: {
      onClickMe: function () {
          this.$emit('child-tell-me-something',this.msg);
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
</style>

```
- Hello.vue

```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    <ul>
      <li><a href="https://vuejs.org" target="_blank">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank">Forum</a></li>
      <li><a href="https://gitter.im/vuejs/vue" target="_blank">Gitter Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank">Twitter</a></li>
      <br>
      <li><a href="http://vuejs-templates.github.io/webpack/" target="_blank">Docs for This Template</a></li>
    </ul>
    <h2>Ecosystem</h2>
    <ul>
      <li><a href="http://router.vuejs.org/" target="_blank">vue-router</a></li>
      <li><a href="http://vuex.vuejs.org/" target="_blank">vuex</a></li>
      <li><a href="http://vue-loader.vuejs.org/" target="_blank">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank">awesome-vue</a></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>

```
> 在src目录下创建store.js文件；
- store.js

```
const STORAGE_KEY = 'todos-vuejs'
export default{
    fetch: function () {
        return JSON.parse(window.localStorage.getItem(STORAGE_KEY) || '[]')
    },
    save: function (items) {
        window.localStorage.setItem(STORAGE_KEY,JSON.stringify(items))
    }
}
```
> 修改App.vue；
- App.vue

```
<template>
  <div id="app">
      <h1 v-text="title"></h1>
      <input v-model="newItem" v-on:keyup.enter="addNew"/>
      <ul>
        <li v-for="item in items" v-bind:class="{finished: item.isFinished}" v-on:click="toggleFinish(item)">
            {{item.label}}
        </li>
      </ul>
      <p>child tells me: {{ childWorlds }}</p>
      <component-a msgfromfather='you die!' v-on:child-tell-me-something='listenToMyBoy'></component-a>
  </div>
</template>

<script>
import Store from './store';
import ComponentA from './components/ComponentA';

export default {
  name: 'hello',
  data: function () {
    return {
      title: 'this is a todo list',
      items: Store.fetch(),
      newItem: '',
      childWorlds: ''
    }
  },
  components: {ComponentA},
  watch: {
    items: {
      handler: function (items) {
          Store.save(items)
      },
      deep: true
    }
  },
  events: {
      'child-tell-me-something': function (msg) {
        this.childWorlds = msg;
      }
  },
  methods: {
    toggleFinish: function(item){
         item.isFinished = !item.isFinished
      },
    addNew: function (){
      this.items.push({
        label: this.newItem,
        isFinished: false
      })
      this.newItem=''
      this.$broadcast('onAddnew', this.items)
    },
    listenToMyBoy: function (msg) {
        this.childWorlds = msg;
    }
  }
}
</script>

<style>
.finished{
  text-decoration:underline;
}
#app {
  margin-left:50px;
}
</style>

```
- main.js

```
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  template: '<App/>',
  components: { App }
})

```

---
### 知识点总结
- new一个vue对象的时候你可以设置它的属性，其中最重要的包括三个，分别是data，methods，watch。
- 其中data代表vue对象的数据，methods代表vue对象的方法，watch设置了对象监听的方法。
- Vue对象里的设置通过html指令进行关联。
- 对象的指令包括

```
 -  v-text 渲染数据
-  v-if 控制显示
-  v-on 绑定事件
-  v-for 循环渲染 等
```
> 如何划分组件
- 功能模块 - select，pagenation....
- 页面区域 - header，footer，sidebar....

---
> 接下来就看看我们的效果吧：

   