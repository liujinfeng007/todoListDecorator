<template>
  <div class="todo-footer" v-show="total">
    <label>
      <input type="checkbox" v-model="All"/>
    </label>
    <span>
			<span>已完成{{ doneTotal }}</span> / 全部{{ total }}
		</span>
    <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
  </div>
</template>

<script lang="ts">
import {Component, Emit, Prop, Vue} from 'vue-property-decorator';
import {Type} from "@/api/type";

@Component
export default class MyFooter extends Vue {
  @Prop(Array)
  todos!: [];



  get total(): number {
    return this.todos.length
  }

  get doneTotal(): number {
    return this.todos.reduce((pre, todo: Type) => pre + (todo.done ? 1 : 0), 0)
  }

  get All() {
    return this.total === this.doneTotal && this.total > 0
  }

  set  All(value){
    this.$emit('checkAllToDo',value)

  }

  @Emit('clearAllToDo')
  clearAll() :void{
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
</style> 1