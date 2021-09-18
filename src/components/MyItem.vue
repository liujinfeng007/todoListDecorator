<template>
  <li>
    <label>
      <input type="checkbox" @change="handleCheck(mytodo.id)" :checked="mytodo.done"/>
      <span v-show="!mytodo.isEdit">{{ mytodo.title }}</span>
      <input
          type="text"
          v-show="mytodo.isEdit"
          :value="mytodo.title"
          @blur='handleBlur(mytodo,$event)'
          ref="inputTitle"
      />
    </label>
    <button class="btn btn-danger" @click="handleDelete(mytodo.id)">删除</button>
    <button class="btn btn-edit" @click="handleEdit(mytodo)" v-show="!mytodo.isEdit">编辑</button>
  </li>

</template>

<script lang="ts">
import {Component, Emit, Prop, Vue} from 'vue-property-decorator';
import pubsub   from 'pubsub-js'
import {Type} from "@/api/type";
@Component
export default class MyItem extends Vue {
  @Prop(Object)
  mytodo!:{};


  handleCheck(id:string) {
    this.$bus.$emit('checkToDo', id)
  }

  handleDelete(id:string) {
    if (confirm("确定删除吗？")) {
      // this.deleteToDo(id)
      // this.$bus.$emit('deleteToDo', id)
      pubsub.publish("deleteToDo", id)
    }
  }

  handleEdit(todo:Type) {
    if (Object.prototype.hasOwnProperty.call(todo, 'isEdit')) {
      todo.isEdit = true;
    } else {
      this.$set(todo, "isEdit", true)
    }
    //在下一次DOM更新后执行该代码
    this.$nextTick(function () {
      // console.log('this.$refs.inputTitle',this.$refs.inputTitle)
      this.$refs.inputTitle.focus()
    })
  }

  handleBlur(todo, e) {
    todo.isEdit = false
    if (!e.target.value.trim()) return alert('输入不能为空！')
    this.$bus.$emit('updateToDo', todo.id, e.target.value)
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
  background-color: #ddd;
}

li:hover button {
  display: block;
}
</style>
