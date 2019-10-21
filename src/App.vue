<template>
  <div id="app" class="container">
    <img src="./assets/logo.png" class="logo">
    <ToDoList/>
    
    <!-- <HelloWorld/> -->
  </div>
</template>

<script>
import ToDoList from './components/ToDoList'
import HelloWorld from './components/HelloWorld'

export default {
  name: 'App',
  components: {
    HelloWorld,
    ToDoList
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
  font-size: 24px;
}
.container {
  max-width: 600px;
  margin: 0 auto;
}
</style>
