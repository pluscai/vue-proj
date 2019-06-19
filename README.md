# 1小时Vue

>  [Vue Tutorial in 2018 - Learn Vue.js by Example](<https://coursetro.com/courses/23/Vue-Tutorial-in-2018---Learn-Vue.js-by-Example>)的笔记，深入浅出，通俗易懂。





## 安装Vue

​	安装vue有三种方式，本次使用Vue CLI

> Vue 提供了一个[官方的 CLI](https://github.com/vuejs/vue-cli)，为单页面应用 (SPA) 快速搭建繁杂的脚手架。它为现代前端工作流提供了 batteries-included 的构建设置。只需要几分钟的时间就可以运行起来并带有热重载、保存时 lint 校验，以及生产环境可用的构建版本

​	步骤：

- 安装vue cli

  ```html
  > npm install -g @vue/cli
  ```

- 开始一个新的Vue项目

  ```
  > vue create vue-proj
  ```

- 进入项目，开启服务，访问localhost:8080

  ```
  yarn serve
  ```

  ![](https://raw.githubusercontent.com/pluscai/imgrepo/master/img/Image.png)

  

## Vue组件

​	组件是组成Vue应用的基本单元，可以看下vue-pro的工程目录

![](https://raw.githubusercontent.com/pluscai/imgrepo/master/img/20190618192427.png)

​	这里的App.vue、Skill.vue就是组件，每个vue文件都是组件。

### 组件的结构

- template 中放置的是html
- script中是页面的逻辑
- style中即样式信息

```vue
<template>
  ...
</template>

<script>
  ...
</script>

<style>
 ...
</style>
```

### 引入其他的组件

​	如下所示：

```vue
<template>
  <!-- Other HTML removed for brevity -->
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  <!-- Other HTML removed for brevity -->
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  }
}
</script>

```

## Vue class和style绑定

### `scoped`

​	style元素上使用了scoped，那么该style下面的写的css或者引用的外部css，只对所在component中的元素起作用.如下：

<style scoped>
    body {
        background-color: #eeeeee;
    }
</style>

### 如何引入外部css资源

> 注意：加了scoped代表该css只在当前componet中元素生效

<style src="./Skills.css" scoped></style>

### class的绑定

```vue
<template>
  <div class="skills">
    <div class="holder">
    
      <!-- Add this -->
      <div v-bind:class="{ alert: showAlert}"></div>

    </div>
  </div>
</template>
<script>
export default {
  name: 'Skills',
    data() {
        return {
          showAlert: true  // Add this
        }
  	},
  }
}
</script>
```

如上，只有在showAlert为true时，才该div的class属性加上alert的值。

### class绑定多个值

```vue
<div v-bind:class="{ alert: showAlert, 'another-class': showClass }"></div>
```

### style绑定

```vue
<div v-bind:style="{ backgroundColor: bgColor, width: bgWidth, height: bgHeight }"></div>
<script>
export default {
  name: 'Skills',
    data() {
        return {
          bgColor: 'yellow',
      	  bgWidth: '100%',
      	  bgHeight: '30px'
        }
  	},
  }
}
</script>
```

> 为了模板中代码的简洁，你也可以如下写

```vue
<div v-bind:style="alertObject"></div>
<script>
export default {
  name: 'Skills',
    data() {
        return {
          alertObject:{
              backgroundColor: 'yellow',
   			  width: '100%',
   			  height: '30px'
          }
        }
  	},
  }
}
</script>
```

## Vue template

>  在vue的模板`<template></template>`里，可以写html，和使用vue特定的功能。

vue在模板里的功能分为两类：

* vue interpolation（vue的插入）  文本插值用{{}}  元素属性插入用v-on v-bind这些指令、也可以在`{{}}`中写表达式
* vue directives （vue的指令）  都是以v-开头的，每个指令都有自己的特定任务

下面的代码中.有用到上述的两种功能

```vue
<template>
    <div class="skills">
       文本插入: {{name}}
        <h1 v-once>{{name}}</h1>
       插入表达式：  {{ btnState ? 'The button is disabled' : 'The button is active'}}
        元素属性插入： <button v-on:click="changeName" v-bind:disabled="btnState">Change Name</button>
        
        v-for指令
        <ul>
            <li v-for="(data,index) in skills" :key='index'>{{index}}. {{data.skill}}</li>
        </ul>
        <p v-if="skills.length >= 1">you have >=1</p>
        <p v-else>you have 小于1</p>
     </div>
</template>
```

全部的vue指令如下

- v-text
- v-html
- v-show
- v-if
- v-else
- v-else-if
- v-for
- v-on
- v-bind
- v-model
- v-pre
- v-cloak
- v-once

## Vue 表单

### v-model的双向绑定

```vue
<!-- Add these two lines: -->
<input type="text" placeholder="Enter a skill you have.." v-model="skill">

{{ skill }}
```

上述代码运行后：在输入框里输入，输入框后面的文本也会显示相应内容

### 如何导入验证的第三方插件

**/src/main.js** 文件中做如下更改

```js
import Vue from 'vue'import App from './App.vue'

import VeeValidate from 'vee-validate'; // Add this
Vue.use(VeeValidate); // Add this

// Other code removed for brevity
```

验证的完整代码：

```vue
<form @submit.prevent="addSkill">
    <input type="text" placeholder="输入一个技能" v-model="skill" v-validate="'min:5'" name="skill">
    <p class="alert" v-if="errors.has('skill')">{{errors.first('skill')}}</p>
</form>
<script>
    export default {
        name: 'Skills',
        data: function() {
            return {
                skill: '',
                skills: [{
                        "skill": "vue.js"
                    },
                    {
                        "skill": "php"
                    }
                ]
            }
        },
        methods: {
            addSkill() {
                this.$validator.validateAll().then((result) => {
                    if(result) {
                        this.skills.push({
                            skill: this.skill
                        });
                        this.skill = "";
                    } else {
                        console.log("Not valid");
                    }
                })
            }
        }
    }
</script>
```

## Vue动画

> vue2中集成了进入离开动画、状态过渡效果

下面演示了一个进入离开动画写法，需要动画的元素被<transition name='value'></transition>包裹，然后利用value值做css选择器的前缀，书写css，如下：

```vue
<form @submit.prevent="addSkill">
    <input type="text" placeholder="Enter a skill you have.." v-model="skill" v-validate="'min:5'" name="skill" >
    <!-- Add these 3 lines -->
    <transition name="alert-in">
    <p class="alert" v-if="errors.has('skill')">{{ errors.first('skill') }}</p>
    </transition>
</form>
<style>
.alert-in-enter-active {
  animation: bounce-in .5s;
}
.alert-in-leave-active {
  animation: bounce-in .5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
</style>
```

上面演示了 **enter-active**、**leave-active**两个类名，实际上vue提供了6中用于过渡中切换的类名。

- **v-enter**
- **v-enter-active**
- **v-enter-to**
- **v-leave**
- **v-leave-active**
- **v-leave-to**

> 对于这些在过渡中切换的类名来说，如果你使用一个没有名字的 <transition>，则 v- 是这些类名的默认前缀。如果你使用了 <transition name="my-transition">，那么 v-enter 会替换为 my-transition-enter。

### vue如何使用动画库

拿Animate.css举例，引入了animate.css之后，直接加 enter-active-class="animated flipInx"

```vue
<transition name="alert-in" 
            enter-active-class="animated flipInX" 
            leave-active-class="animated flipOutX">
<p class="alert" 
   v-if="errors.has('skill')">{{ errors.first('skill') }}</p>
</transition>
```

## Vue路由

vue提供了vue-router,用于在vue组件中设置页面路径

1. 安装vue router

   ```
   > yarn add vue-router
   ```

2. 新增 /src/router.js 配置路由：

   ```js
   import Vue from 'vue'
   import Router from 'vue-router'
   import Skills from './components/Skills.vue'
   import About from './components/About.vue'
   Vue.use(Router)
   export default new Router({
     routes: [
       {
         path: '/',
         name: 'skills',
         component: Skills
       },
       {
         path: '/about',
         name: 'about',
         component: About
       }
     ]
   })
   ```

3. main.js中引入路由

   ```js
   // other imports removed for brevity
   import router from './router'
   new Vue({
     router,               // Add this line
     render: h => h(App)
   }).$mount('#app')
   
   ## 在App.vue中运用路由
   <template>
     <div id="app">
         <nav>
           <router-link to="/">Home</router-link>
           <router-link to="/about">About</router-link>
         </nav>
       <router-view/>
     </div>
   </template>
   ```

最终的切换效果：









