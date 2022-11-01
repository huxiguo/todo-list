<template>
  <div class="todo-footer" v-if="todo.length">
    <label>
      <input type="checkbox" :checked="isAll" @change="checkAll" />
    </label>
    <span>
      <span>已完成{{ doneTotal }}</span> / 全部{{ todo.length }}
    </span>
    <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
  </div>
</template>

<script>
import { set } from 'vue'
export default {
  name: 'Footer',
  props: ['todo'],
  methods: {
    checkAll(e) {
      // this.checkAllTodo(e.target.checked)
      this.$emit('checkAllTodo', e.target.checked)
    },
    clearAll() {
      // this.clearAllTodo()
      this.$emit('clearAllTodo')
    }
  },
  computed: {
    doneTotal() {
      return this.todo.reduce((pre, current) => {
        return pre + (current.done ? 1 : 0)
      }, 0)
    },
    isAll: {
      get() {
        return this.doneTotal === this.todo.length && this.todo.length > 0
      },
      set(value) {
        this.$emit('checkAllTodo', value)
      }
    }
  }
}
</script>

<style scoped>
/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>
