<template>
  <div id="app">
    <Calc></Calc>
  </div>
</template>

<script>
import Calc from './components/Calc.vue';

export default {
  name: 'app',
  data() {
    return {
      todos: ['first', 'second']
    }
  },
  components: {
    Calc,
  },
  methods: {
    deleteTodo(index) {
      this.todos.splice(index, 1);
    }
  }
}
</script>

<style>

</style>
