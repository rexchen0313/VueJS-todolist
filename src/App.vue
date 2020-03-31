<template>
  <div id="app">
    <TodoForm v-on:todoTask="getTodoTask"/>
    <List v-bind:todoList="items" />
  </div>
</template>

<script>
import List from './components/List';
import TodoForm from './components/TodoForm';

export default {
  name: 'App',
  components: {
    List,
    TodoForm
  },
  data() {
    return {
      items: []
    }
  },
  methods: {
    getTodoTask(todoTask) {
      this.items.unshift(todoTask);
    }
  }
}
</script>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
  font-family: Roboto-Medium;
}
html, body {
  background-color: #E1E1E1;;
  width: 100%;
  height: 100%;
}

</style>
