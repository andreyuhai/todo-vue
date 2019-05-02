<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input type="checkbox" v-model="completed" @change="doneEdit">
      <div
        v-if="!editing"
        @dblclick="editTodo"
        class="todo-item-label"
        :class="{completed : completed }"
      >{{title}}</div>
      <input
        v-else
        class="todo-item-edit"
        type="text"
        v-model="title"
        @blur="doneEdit"
        @keyup.enter="doneEdit"
        v-focus
        @keyup.esc="cancelEdit"
      >
    </div>
    <div class="remove-item" @click="removeTodo(todo.id)">&times;</div>
  </div>
</template>

<script>
export default {
  name: 'todo-item',
  props: {
    todo: {
      type: Object,
      required: true
    },
    checkAll: {
      type: Boolean,
      required: true
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },
  data() {
    return {
      'id': this.todo.id,
      'title': this.todo.title,
      'completed': this.todo.completed,
      'editing': this.todo.editing,
      'beforeEditCache': ''
    }
  },
  watch: {
    checkAll() {
      this.completed = this.checkAll ? true : this.todo.completed
    }
  },
  methods: {
    removeTodo(id) {
      eventBus.$emit('removedTodo', id)
    },
    editTodo() {
      this.beforeEditCache = this.title
      this.editing = true
    },
    doneEdit() {
      if (this.title.trim().length == 0) {
        this.title = this.beforeEditCache
      }
      this.editing = false
      eventBus.$emit('finishedEdit', {
        'id': this.id,
        'title': this.title,
        'completed': this.completed,
        'editing': this.editing
      })
    },
    cancelEdit() {
      this.title = this.beforeEditCache
      this.editing = false
    }
  }
}
</script>


<style>
</style>
