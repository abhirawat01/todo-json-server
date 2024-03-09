<template>
  <div id="app">
    <p class="display-3">APTCODER Assignment</p>
    <TodoList />
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue';

export default {
  name: 'App',
  components: {
    TodoList
  }
};
</script>

<style>
body{
  background-color: rgb(66, 66, 66);
  color:white
}
#app {
  text-align: center;
  margin-top: 30px;
}
</style>
