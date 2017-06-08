<template>
  <div class="hello">
    <h3>{{ msg }}</h3>
    <h1>{{ msgfromfather }}</h1>
   <button v-on:click="onClickMe">CLICK</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Hello from component 1',
      childmsg: '孩子传来的信息'
    }
  },
  // props组件之间的通信，父级向子级传递信息
  // props: ['msgfromfather'],
  props:['msgfromfather'],
  events: {
    'onAddnew': function(items) {
      console.log(items)
    }
  },
  methods: {
    onClickMe() {
      //console.log(this.msgfromfather)
      // 每个 Vue 实例都是一个事件触发器：
      // 使用 $on() 监听事件；
      // 使用 $emit() 在它上面触发事件；
      //this.$emit('child-msg', this.childmsg);
      this.$emit('child-msg', this.childmsg);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  color: #42b983;
}
</style>
