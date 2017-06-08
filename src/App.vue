<template>
  <div id="app">
    <h1>{{title}}</h1>
    <h2 v-html="title_two"></h2>
    <!-- v-model双向绑定 -->
    <input v-model="newItem" v-on:keyup.enter="addNew">
      <p v-for="item in items" :class="{finished: item.isFinished}" v-on:click="toggleFinish(item)">
        {{item.label}}
      </p>
    <h3>chlid tells me: {{ childWords }}</h3>
    <!-- 自定义事件child-msg 用于监听emit -->
    <comp1 msgfromfather='孩子，你好(父传子)' v-on:child-msg='listenToMyChild'></comp1>
  </div>
</template>

<script>

import Store from './localstorage'
import Comp1 from './components/comp1'

export default {
  data() {
    return {
      title: 'TODO LIST',
      title_two: 'BY WZZ',
      newItem: '',
      items: Store.fetch(),
      childWords: '等待接收孩子的消息'
    }
  },
  components: {
    Comp1
  },
  // 监听 
  watch: {
    //监听items的变化自动触发更新
    items: {
      handler: function(items){
        //保存到localstorage
        Store.save(items)
      },
      deep: true 
    }
  },
  // 使用$dispatch需要使用events监听
  //方法放在methods
  methods: {
    toggleFinish: function(item) {
      // console.log(item)
      item.isFinished = !item.isFinished
    }, // es6方法写法
    dbclick: function (item) {
      item.remove();
    },
    //es6语法
    addNew() {
      this.items.push({
        label: this.newItem,
        isFinished: false
      })
      this.newItem = ''
      this.$broadcast('onAddnew', this.items) //向子级广播
    },
    //监听子级传来的参数msg
    listenToMyChild: function(message){
      this.childWords = message;
    }
  }
}
</script>

<style>
html {
  height: 100%;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}

#app {
  color: #2c3e50;
  margin-top: -100px;
  max-width: 600px;
  font-family: Source Sans Pro, Helvetica, sans-serif;
  text-align: center;
}

p{
  line-height: 10px;
}

#app a {
  color: #42b983;
  text-decoration: none;
}

.finished{
  color: #42b983;
  font-size: 20px;
}
.logo {
  width: 100px;
  height: 100px
}
</style>
