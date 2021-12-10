<template>
    <header>
        <h1>
          {{ title }}  
        </h1>
        <Button @toggle-add-task="$emit('toggle-add-task')" :color="addtaskbtntext ? 'red' : 'green' " :text="addtaskbtntext ? 'Close' : 'Add Task'" />
        
    </header>
</template>

<script>
import Button from './Button.vue'
export default {
    name:'Header',
    props :{
      title: String,
      addtaskbtntext:Boolean
    },
    components: {
        Button
    }, 
    emit:["toggle-add-task"] 
}
</script>

<style>
    header{
        display: flex;
        justify-content: space-around;
        align-items: center;
        margin-bottom: 20px;    
    }

</style>