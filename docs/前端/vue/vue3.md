main.js

```typescript
//程序的主入口文件,ts文件，是main.ts
//引入createApp函数，创建对应的应用，产生应用的实例对象
import { createApp } from 'vue'

//引入App组件(所有组件的父级组件)
import App from './App.vue'

//创建App应用返回对应的实例对象，调用mount方法进行挂载，所有组件产生完毕后，会在index.html中的div渲染
createApp(App).mount('#app')
```

index.html

```html
  <body>
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
```



App.vue

```typescript
<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <!-- 使用这个子级组件 -->
  <HelloWorld msg="Welcome to Your Vue.js + TypeScript App"/>
</template>

<script lang="ts">
// 这里可以使用ts代码

// defineComponent函数，目的是定义一个组件，内部可以传入一个配置对象
import { defineComponent } from 'vue';
// 引入一个子级组件
import HelloWorld from './components/HelloWorld.vue';

// 暴露一个定义好的组件
export default defineComponent({
  // 当前组件的名字
  name: 'App',
  // 注册组件
  components: {
    // 注册一个子级组件
    HelloWorld
  }
});
</script>

<style>

</style>
```







测试代码，setup是组合api第一个测试的函数

```typescript
<template>
  <div>哈哈，我是溪云</div>
  <h1>{{number}}</h1>
</template>

<script lang="ts">
// 这里可以使用ts代码

// defineComponent函数，目的是定义一个组件，内部可以传入一个配置对象
import { defineComponent } from 'vue';
// 引入一个子级组件
import HelloWorld from './components/HelloWorld.vue';

// 暴露一个定义好的组件
export default defineComponent({
  // 当前组件的名字
  name: 'App',
  setup(){
    const number = 10
    return {
      number
    }
  }

});
</script>
```

