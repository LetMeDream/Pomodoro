<template>
  <div id="app">
    <timer-container>
    </timer-container>
    <!-- <task-list></task-list> -->
  </div>
</template>

<script>
/* import HelloWorld from './components/HelloWorld.vue' */
import TimerContainer from './components/timerContainer.vue'
/* import TaskList from './components/taskList.vue' */

export default {
  name: 'App',
  components: {
    TimerContainer
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Staatliches';
}
body{
  transition: 1s ease all;
  background:#3A4750;
  min-height: 100vh;
  display:flex;
  align-items: center;
  justify-content: center;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

</style>
