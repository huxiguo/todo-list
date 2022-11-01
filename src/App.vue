<template>
  <div id="app">
    <div class="todo-container">
      <div class="todo-wrap">
        <!-- 头部 -->
        <Top @addTodo="addTodo"></Top>
        <List :todo="todos"></List>
        <MyFooter
          :todo="todos"
          @checkAllTodo="checkAllTodo"
          @clearAllTodo="clearAllTodo"
        ></MyFooter>
      </div>
    </div>
  </div>
</template>

<script>
import Top from '@/components/Top.vue'
import MyFooter from '@/components/MyFooter.vue'
import List from '@/components/List.vue'
export default {
  name: 'App',
  components: {
    Top,
    MyFooter,
    List
  },
  data() {
    return {
      todos: JSON.parse(localStorage.getItem('todos')) || []
    }
  },
  methods: {
    // 添加待办
    addTodo(todoObj) {
      this.todos.unshift(todoObj)
    },
    // 取消勾选
    checkTodo(id) {
      this.todos.forEach((todo) => {
        if (todo.id === id) todo.done = !todo.done
      })
    },
    deleteTodo(id) {
      this.todos = this.todos.filter((item) => {
        return item.id !== id
      })
    },
    checkAllTodo(done) {
      this.todos.forEach((item) => {
        item.done = done
      })
    },
    clearAllTodo() {
      this.todos = this.todos.filter((item) => {
        return !item.done
      })
    }
  },
  watch: {
    todos: {
      deep: true,
      handler(value) {
        localStorage.setItem('todos', JSON.stringify(value))
      }
    }
  },
  mounted() {
    this.$bus.$on('checkTodo', this.checkTodo)
    this.$bus.$on('deleteTodo', this.deleteTodo)
  },
  beforeDestroy() {
    this.$bus.$off(['checkTodo', 'deleteTodo'])
  }
}
</script>

<style lang="less">
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
