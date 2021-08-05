<template>
  <div id="app">
    <timer-container :currentColor="currentColor">
    </timer-container>
  </div>
</template>

<script>
/* import HelloWorld from './components/HelloWorld.vue' */
import TimerContainer from './components/timerContainer.vue'

export default {
  name: 'App',
  components: {
    TimerContainer
  },
  methods: {
    currentColor(color){
        let ourBody = document.querySelector('body');
        ourBody.style.background = color;
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Staatliches';
  text-shadow: 1px 1px black;
}
body{
  transition: 1s ease all;
  background:#3A4750;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  min-height: 100vh;

  display:flex;
  justify-content: center;
  align-items: center;
}
</style>
