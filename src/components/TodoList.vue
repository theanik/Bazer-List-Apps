<template>
    <div>
        Todo App
        <div class="container">
            <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
        </div>
         <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
         <todo-item v-for="(todo, index) in FilterTodos" :key="todo.id" :todo="todo" :index="index"
         @removedTodo="removeTodo" @finishedEdit="finishedEdit" :checkedAll="!anyRemaining">
             
            <!-- <div class="todo-item-left">
                <input type="checkbox" v-model="todo.completed">
                <div v-if="!todo.editing" class="todo-item-label" :class="{ completed : todo.completed }" @dblclick="editTodo(todo)">{{ todo.title }}</div>
                <input v-else type="text" class="todo-item-edit" v-model="todo.title" @blur="doneEdit(todo)"
                @keyup.enter="doneEdit(todo)" v-focus @keyup.esc="cancleEdit(todo)">
            </div>
            <div class="remove-item" @click="removeTodo(index)">
                &times;
            </div> -->
        </todo-item>
        </transition-group>
        <div class="extra-container">
            <div>
              <label>
                <input type="checkbox" @change="checkAll" :checked="!anyRemaining"> Check All</label>
              </div>
            <todo-remaining :remaining="remaining"></todo-remaining>
        </div>
        <div class="extra-container">
            <div>
              <filter-todo></filter-todo>
                
            </div>
             <div>
                 <transition name="fade">
                <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
               </transition>
            </div>

        </div>
    </div>

    
</template>

<script>
import TodoItem from './TodoItem'
import TodoRemaining from './TodoRemaining'
import FilterTodo from './FilterTodo'
export default {
    components:{
        TodoItem,
        TodoRemaining,
        FilterTodo
    },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      newTodo : '',
      idTodo : 1,
      titlecache : '',
      filter : 'all',
      todos :[

      ]

    }
  },
  created(){
    eventBus.$on('changeFilter',(filter)=>this.filter = filter)
  },
  directives: {
  focus: {
    // directive definition
    inserted: function (el) {
      el.focus()
    }
  }
},
computed:{
    remaining(){
        return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining(){
        return this.remaining != 0
    },
    FilterTodos(){
        if(this.filter == 'all'){
            return this.todos
        }else if(this.filter == 'active'){
            return this.todos.filter(todo => !todo.completed)
        }else if(this.filter == 'completed'){
            return this.todos.filter(todo => todo.completed)
            
        }
        return this.todos
    },
    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0
    }
},
  methods :{
      addTodo(){
          if(this.newTodo.trim().length == 0){
              return
          }

          this.todos.push({
              id : this.idTodo,
              title : this.newTodo,
              completed : false,
              editing : false
          })

          this.newTodo = ''
          this.idTodo++
      },
      removeTodo(index){
          this.todos.splice(index,1)
      },
      cancleEdit(todo){
          todo.title = this.titlecache
          todo.editing = false
      },
      checkAll(){
          this.todos.forEach((todo) =>{
              todo.completed = event.target.checked
          })
      },
      clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed)
    },
    finishedEdit(data){
        this.todos.splice(data.index,1,data.todo)
    }
  }
}
</script>

<style>
  @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");
  .todo-input {
    width: 100%;
    padding: 10px 18px;
    font-size: 18px;
    margin-bottom: 16px;
  }
  .todo-item {
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    animation-duration: 0.3s;
  }
  .remove-item {
    cursor: pointer;
    margin-left: 14px;
    font-style: bold;
    color: red;
    font-size: 25px;
  }
  .remove-item:hover{
      color: black;
  }
  .todo-item-left {
    display: flex;
    align-items: center;
  }
  .todo-item-label {
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
  }
  .todo-item-edit {
    font-size: 24px;
    color: #2c3e50;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc; 
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    /* &:focus {
      outline: none;
    } */
  }
  .completed {
    text-decoration: line-through;
    color: grey;
  }
  .extra-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 14px;
    margin-bottom: 14px;
  }
  /* button {
    font-size: 14px;
    background-color: white;
    appearance: none;
    &:hover {
      background: lightgreen;
    }
    &:focus {
      outline: none;
    }
  } */
  .active {
    background: lightgreen;
  }
  .fade-enter-active, .fade-leave-active {
    transition: opacity .2s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
</style>