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
        <TodoListItem 
        v-for="(todoItem,index) in todoItems" 
        :key="index"
        :index="index"
        :todoItem="todoItem"
        @remove="removeTodoItem"
        @toggle="toggleTodoItemComplete"
        />
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
  save(todoItems : Todo[] ){
    const parsed = JSON.stringify(todoItems);
    localStorage.setItem(STORAGE_KEY , parsed);
  },
  fetch():Todo[]{
    const todoItems = localStorage.getItem(STORAGE_KEY) || "[]";
    const result = JSON.parse(todoItems); // type 에러 부분 
    // 데이터 조회 API 설계부터 봐야함
    return result;
  }
}

export interface Todo{ //객체를 위한 타입
    title:string;
    done: boolean;
}
  export default Vue.extend({
    components:{TodoInput, TodoListItem},
    data(){
      return{
        todoText:'',
        todoItems:[] as Todo[]
      }
    },
    methods:{
      updateTodoText(value:string){
        this.todoText =value;
      }
      ,
      addTodoItem(){
        const value = this.todoText;
        const todo:Todo ={
          title:value,
          done:false
        }
        this.todoItems.push(todo);
        storage.save(this.todoItems);
        // localStorage.setItem(value,value);
        this.initTodoText();

      },
      initTodoText(){
        this.todoText='';
      },
      fetchTodoItems(){
       this.todoItems =  storage.fetch().sort((a,b)=>{
        if(a.title<b.title){
          return -1;
        }
        if(a.title > b.title){
          return 1;
        }
        return 0;
       });
      },
      removeTodoItem(index:number){
        this.todoItems.splice(index,1);
        storage.save(this.todoItems);
      },
      toggleTodoItemComplete(todoItem:Todo,index:number){
        this.todoItems.splice(index,1,{
          ...todoItem, //todoItem 에 있는 값을 다 가져옴
          // title: todoItem.title,
          done: !todoItem.done //가져온 뒤 바꿔야하는 속성만 지정해서 바꿈
        });
        storage.save(this.todoItems);
      }
    },
    created(){
      this.fetchTodoItems()
    }
  })
</script>

<style scoped>

</style>
