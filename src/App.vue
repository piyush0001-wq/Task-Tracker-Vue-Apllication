<template>
<div class="container">
  <Header @show-add-task="showAdd" title="Task Tracker" :AddShowTask="AddShowTask" />
  <div v-show="AddShowTask">
    <AddTasks @add-task="addTask" />
  </div>
  <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />

<router-view></router-view>
  <Footer />
</div>
  
</template>

<script>
import Header from './components/Header' 
import Footer from './components/footer' 
import Tasks from './components/Tasks' 
import AddTasks from './components/AddTask' 



export default {
  name: 'App',
  components: {
    Header,
    Tasks,AddTasks,Footer
    
  },
  data(){
    return{
      tasks:[],
      AddShowTask: false,
    }
  },
  methods:  {
    
    showAdd(){
      this.AddShowTask = !this.AddShowTask
    },
    async addTask(task){
      const res = await fetch('api/tasks', {
        method:'POST',
        headers:{
          'Content-type':'application/json'
         
        },
         body:JSON.stringify(task)
      })
      const data = await res.json()
      this.tasks = [...this.tasks, data]
      
    },
    async deleteTask(id){
      if(confirm(`Are you sure for deleting id ${id} ?`)){
        const res = await fetch(`api/tasks/${id}`,{
          method:'DELETE'
        })
        res.status === 200?(this.tasks = this.tasks.filter((task)=> task.id !== id)) : alert(`Error in deleting task`)
          //filter is a high order array method and it takes a funtion that loop through the given arguments, we want everything back that is not equall to the id we passed!
      }
    },
    async toggleReminder(id){
      const taskToToggle = await this.fetchTask(id)
      const updTask = {...taskToToggle, reminder:!taskToToggle.reminder}

      const res = await fetch(`api/tasks/${id}`,{
        method: 'PUT',
        headers:{
          'Content-type':'application/json'
        }, 
         body:JSON.stringify(updTask)
      })

      const data = await res.json()

      this.tasks = this.tasks.map((task)=>(task.id ===id ? {...task, reminder: data.reminder}:task))
      
    },
    async fetchTasks(){
       
      const res = await fetch('api/tasks')
      const data = await res.json()
      return data
      },
    async fetchTask(id){
       
      const res = await fetch(`api/tasks/${id}`)
      const data = await res.json()
      return data
      },

    },

    async created(){
    this.tasks = await this.fetchTasks()

  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Cinzel+Decorative&display=swap');
*{
  font-family: Cinzel;
  box-sizing: border-box;
  margin: 0;
}
::-webkit-scrollbar{
  display: none;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  
}
.container{
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn{
  display: inline-block;
  width: 100px;
  background: #000;
  color: white;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  border: none;
  outline: none;
  cursor: pointer;
  font-size: 15px;

}
</style>
