<template>
 <header>
     <h1>{{ title }}</h1>
     <Button  @show-add-task="$emit('show-add-task')" :text="AddShowTask ? 'Close' : 'Add Task'" :color="AddShowTask ? 'red':'green'" />
     <!-- <Button text="Update Task" color="yellow" /> -->
     <!-- <Button text="Delete Task" color="red" /> -->
 </header>
</template>

<script>
import Button from './Button'


export default{
    name: 'Header',
    props:{
        title:String,
        AddShowTask: Boolean
        
    },
    components:{
        Button,
    },
    methods:{
        onClick(){
           this.$emit('show-add-task')
        },
    },
    }
    

    
</script>

<style scoped>
header{
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px ;
}
</style>
