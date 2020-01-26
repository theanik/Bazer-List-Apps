<template>
    <div>
        <button :class="{ active: filter == 'all' }" @click="filterTodo('all')">All</button>
        <button :class="{ active: filter == 'active' }" @click="filterTodo('active')">Active</button>
        <button :class="{ active: filter == 'completed' }" @click="filterTodo('completed')">Completed</button>
    </div>
</template>
<script>

export default {
    name : 'filter-todo',
    data(){
        return{
            filter : 'all'
        }
    },
    methods:{
        filterTodo(filter){
            this.filter = filter
            eventBus.$emit('changeFilter',filter)
        }
    }
}
</script>