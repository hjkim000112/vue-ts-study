<template>
  <div>
    <header>
      <h1>Vue Todo with Typescript</h1>
    </header>
    <main>
      <TodoInput :item="todoText" @input="updateTodoText" @add="addTodoItem"/>
    </main>
    <div>
      <ul>
        <TodoListItem />
        <!-- <li>item 1</li>
        <li>item 2</li>
        <li>item 3</li> -->
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
  import Vue from 'vue'
  import TodoInput from "@/components/TodoInput.vue";
import TodoListItem from './components/TodoListItem.vue';
const STORAGE_KEY = 'vue-todo-ts-v1';
const storage = { //LocalStorage 
  fetch(){
    const todoItems = localStorage.getItem(STORAGE_KEY) || [];
    // const result = JSON.parse(todoItems);
    //데이터 조회 API 설계부터 봐야함
    // return result;
  }
}
  export default Vue.extend({
    components:{TodoInput, TodoListItem},
    data(){
      return{
        todoText:''
      }
    },
    methods:{
      updateTodoText(value:string){
        this.todoText=value;
      }
      ,
      addTodoItem(){
        const value = this.todoText;
        localStorage.setItem(value,value);
        this.initTodoText();

      },
      initTodoText(){
        this.todoText='';
      }
    }
  })
</script>

<style scoped>

</style>
