<template>
    <div>
        <label for="todo-input">오늘 할 일 : </label>
        <input id="todo-input" type="text" :value="item" @input="handleInput">
        <button @click="addTodo" type="button">추가</button>
    </div>
</template>

<script lang="ts">
    import Vue from 'vue'

    export default Vue.extend({
        props:{
            item:{
                type:String,
                required:true
            }
        },
        methods:{
            handleInput(event: InputEvent){
            
            //event.target.value :이렇게 하면 타입 에러가 나기떄문에 
            //이것은 HTML Input 객체라는것을 미리 알려줌.
            const eventTartget =event.target as HTMLInputElement
            this.$emit("input",eventTartget.value);
            },
            addTodo(){
                this.$emit("add");
            }
        }
    })
</script>

<style scoped>

</style>