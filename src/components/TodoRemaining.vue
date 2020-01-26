<template>
    <div>{{ remaining }} items left</div>
</template>
<script>
name : 'todo-ramining'
export default {
    props:{
        remaining:{
            type:Number,
            required :true
        }
    }
}
</script>