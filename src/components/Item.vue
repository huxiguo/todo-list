<template>
  <transition
    name="animate__animated animate__bounce"
    appear
    enter-active-class="animate__bounceInLeft"
    leave-active-class="animate__hinge"
  >
    <li>
      <label>
        <input
          type="checkbox"
          :checked="todo.done"
          @change="handleCheck(todo.id)"
        />
        <span>{{ todo.title }}</span>
      </label>
      <button class="btn btn-danger" @click="handleDelete(todo.id)">
        删除
      </button>
    </li>
  </transition>
</template>

<script>
import 'animate.css'
export default {
  name: 'Item',
  data() {
    return {}
  },
  props: ['todo'],
  methods: {
    handleCheck(id) {
      // this.checkTodo(id)
      this.$bus.$emit('checkTodo', id)
    },
    handleDelete(id) {
      if (confirm('确定删除？')) {
        // this.deleteTodo(id)
        this.$bus.$emit('deleteTodo', id)
      }
    }
  }
}
</script>

<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
  background-color: orange;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}
li:hover {
  background-color: #fff;
}
li:hover button {
  display: block;
}
</style>
