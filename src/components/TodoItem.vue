<template>
    <div class="todo-item">
        <div class="todo-item-left">
                <input type="checkbox" v-model="completed" @change="doneEdit">
                <div v-if="!editing" class="todo-item-label" :class="{ completed : completed }" @dblclick="editTodo()">{{ title }}</div>
                <input v-else type="text" class="todo-item-edit" v-model="title" @blur="doneEdit()"
                @keyup.enter="doneEdit()" v-focus @keyup.esc="cancleEdit()">
            </div>
            <div class="remove-item" @click="removeTodo(index)">
                &times;
            </div>
    </div>
</template>
<script>
export default {
    name:'todo-item',
    props :{
        todo:{
            type : Object,
            required : true
        },
        index :{
            type : Number,
            required : true
        },
        checkedAll:{
            type :Boolean,
            required : true
        }
    },
    data(){
        return{
            id : this.todo.id,
            title : this.todo.title,
            completed : this.todo.completed,
            editing : this.todo.editing,
            titlecache : ''
        }
    },
    watch :{
        checkedAll(){
            if(this.checkedAll){
                this.completed = true
            }else{
                this.completed = this.todo.completed
            }
        }
    },
    methods:{
        removeTodo(index){
            this.$emit('removedTodo',index)
        },
        editTodo(){
          this.titlecache = this.title
          this.editing = true
        },
        doneEdit(){
            if(this.title.trim() == ''){
                return
            }
            this.editing = false
            this.$emit('finishedEdit',{
                'index' : this.index,
                'todo' :{
                     id : this.id,
                    title : this.title,
                    completed : this.completed,
                    editing : this.editing,
                }
            })
        },
        cancleEdit(todo){
            this.title = this.titlecache
            this.editing = false
        },
    }
}
</script>