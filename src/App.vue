<template>
  <div id="app">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addToDo="addToDo"/>
        <MyList :todo="todos"/>
        <MyFooter :todos="todos" @checkAllToDo="checkAllToDo" @clearAllToDo="clearAllToDo"/>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import {Component, Vue, Watch} from 'vue-property-decorator';
import {Type} from "@/api/type";
import pubsub from 'pubsub-js'
import MyHeader from "@/components/MyHeader.vue";
import MyList from "@/components/MyList.vue";
import MyFooter from "@/components/MyFooter.vue";

@Component({
  components: {
    MyHeader,
    MyList,
    MyFooter
  },
})
export default class App extends Vue {
  todos = JSON.parse(localStorage.getItem('todos')) || [];

  mounted() {
    this.$bus.$on('checkToDo', this.checkToDo)
    this.$bus.$on('updateToDo', this.updateToDo)

    // this.$bus.$on('deleteToDo',this.deleteToDo)
    this.pubId = pubsub.subscribe("deleteToDo", this.deleteToDo)
  }

  beforeDestroy() {
    this.$bus.$off('checkToDo')
    // this.$bus.$off('deleteToDo')
    this.$bus.$off('updateToDo')

    pubsub.unsubscribe('pubId')
  }

  addToDo(todoObj: Type) {
    this.todos.unshift(todoObj)
  }

  checkToDo(id: string) {
    this.todos.forEach((todo:Type) => {
      if (todo.id === id) {
        todo.done = !todo.done
      }
    })
  }

  deleteToDo(_, id: string) {
    this.todos = this.todos.filter(todo => {
      return todo.id !== id
    })
  }

  checkAllToDo(done: boolean) {
    this.todos.forEach((todo) => {
      todo.done = done
    })
  }

  clearAllToDo() {
    this.todos = this.todos.filter(todo => {
      return !todo.done
    })
  }

  updateToDo(id: string, title:string) {
    this.todos.forEach((todo) => {
      if (todo.id === id) {todo.title = title}
    })
  }

  @Watch("todos", {deep: true})
  onChangeValue(value) {
    localStorage.setItem('todos', JSON.stringify(value))
  };


}
</script>

<style>
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
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-edit {
  color: #fff;
  background-color: skyblue;
  border: 1px solid #6ba1b6;
  margin-right: 5px;
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

