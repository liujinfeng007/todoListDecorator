<template>
  <ul class="todo-main">

    <transition-group name="animate__animated animate__bounce" appear
                      enter-active-class="animate__backInRight"
                      leave-active-class="animate__backOutDown"
    >
      <MyItem v-for="todoObj in todo" :key="todoObj.id" :mytodo="todoObj"></MyItem>
    </transition-group>


  </ul>

</template>

<script lang="ts">
import 'animate.css'
import {Component, Prop, Vue} from 'vue-property-decorator';
import MyItem from "@/components/MyItem.vue";
@Component({
  components: {
    MyItem
  },
})
export default class MyList extends Vue {
  @Prop(Array)
  todo!:[]

/*  mounted(){
    console.log('mylist',this.todo)
  }*/
}
</script>

<style scoped>
/*main*/
.todo-main {
  margin-left: 0;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0;
}
.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}

.todo-enter-active {
  animation: todo 0.5s linear;
}

.todo-leave-active {
  animation: todo 0.5s linear reverse;
}

@keyframes todo {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(0px);
  }

}
</style>
