<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/Twitter_Logo_Blue.svg" class="logo">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}
body {
  background-color: hsl(34, 78%, 91%);
  line-height: 1.5rem;
  font-family: Arial;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
#app .logo {
  width: 200px;
}
.btn {
  background-color: hsl(202, 88%, 53%);
  color: white;
  padding: .25em 1em;
  text-decoration: none;
  border: none;
  cursor: pointer;
  font-size: inherit;
  border-radius: .2em;
}
.btn:focus, .btn:hover {
  background-color: hsl(200, 100%, 40%);
}
.btn:disabled {
  background-color: hsl(200, 100%, 90%);;
}
</style>
