<template>
   <li>
       <span class="item" 
       :class="todoItemClass" 
       @click="toggleItem"
       >{{todoItem.title}}</span>
       <button @click="removeItem">삭제</button> 
       <!-- @click="$emit('remove')" 이런식으로 바로 올려주는 행위는
        가능은 하지만 테스트할 때 추천하지 않는 방법으로 웬만하면 사용하지 않는것이 좋음
        (테스트할 때 mock 함수를 사용하기 번거로워짐?)-->
    </li>
</template>

<script lang="ts">
    import { Todo } from '@/App.vue'
import Vue, { PropType } from 'vue'

    export default Vue.extend({
        props: {
            todoItem: Object as PropType<Todo>, //이 객체의 타입은 title,done이 들어가는 Todo 라는 뜻
            //ctrl+space : 자동완성 
            index:Number 
        },
        computed:{
            todoItemClass():string|null{ // 반환타입 꼭 정의해줘야함.
                return this.todoItem.done? 'complete':null
            }
        },
        methods:{
            toggleItem(){
                this.$emit("toggle",this.todoItem,this.index);
            },
            removeItem(){
                this.$emit('remove',this.index);
            },

        }
    })
</script>

<style scoped>
.item{
    cursor:pointer;
}

.complete{
    text-decoration:line-through;

}

</style>