<template>
  <button @click="clearCompleted">Clear Completed</button>
</template>

<script>
export default {
  name: 'todo-clear-completed',
  methods: {
    clearCompleted() {
      eventBus.$emit('clearCompleted')
    }
  }
}
</script>

<style>
</style>


