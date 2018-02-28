<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: "App",
  beforeRouteEnter(to, from, next) {
    console.log("组件路由勾子：beforeRouteEnter, 下一行打印自身this");
    console.log(this); //undefined，不能用this来获取vue实例
    next(vm => {
      console.log("组件路由勾子beforeRouteEnter的next");
      console.log(vm); //vm为vue的实例
    });
  },
  beforeCreate() {
    console.log("beforeCreate");
  },
  created() {
    this.$nextTick(() => {
      console.log("nextTick");
    });
    console.log("created");
  },
  beforeMount() {
    console.log("beforeMount");
  },
  mounted() {
    console.log("mounted");
  },
  beforeUpdate() {
    console.log("beforeUpdate");
  },
  updated() {
    console.log("updated");
  },
  beforeDestroy: function() {
    console.log("beforeDestroy 销毁前状态===============》");
  },
  destroyed: function() {
    console.log("destroyed 销毁完成状态===============》");
  },
  beforeRouteLeave(to, from, next) {
    console.log("组件路由勾子：beforeRouteLeave, 下一行打印自身this");
    console.log(this); //可以访问vue实例
    next();
  }
};
</script>

<style>
  #app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
