<template>
<div class="container">
  <Header @toggle-add-task="showAddTaskfun" :addtaskbtntext="showaddtask" title="Task Tracker" />
  <div v-show="showaddtask">
    <AddTask @add-task="addTask" />
  </div>
  <Tasks @toggle-reminder="toggleReminder"  @delete-task="deleteTask" :tasks="tasks" />
</div>

</template>

<script>
 import Header from '../src/components/Header.vue'
 import Tasks from '../src/components/Tasks.vue'
 import AddTask from '../src/components/AddTask.vue'
export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask
  },
  methods:{
   async deleteTask(id){
      if(confirm("Are you sure?"))
      {
        const res = await fetch(`http://localhost:5000/tasks/${id}`,{
          method:"DELETE"
        });

        res.status === 200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert("Error while deleting");

      
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.getTaskid(id)
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder }
      const res = await fetch(`http://localhost:5000/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updTask),
      })
      const data = await res.json()
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      )
    },
   async addTask(newtask){
      const res = await fetch("http://localhost:5000/tasks",{
        method:"POST",
        headers:{
          "Content-type":"application/json"
        },
        body:JSON.stringify(newtask),
      });

      const data = await res.json();
      this.tasks = [...this.tasks,data]
    },
    showAddTaskfun(){
        this.showaddtask = !this.showaddtask;
    },
    async getTask(){
      const res = await fetch("http://localhost:5000/tasks");
      const data = await res.json();
      return data
    },
    async getTaskid(id){
      const res = await fetch(`http://localhost:5000/tasks/${id}`);
      const data = await res.json();
      return data
    }

  },
  data() {
    return{
      tasks:[],
      showaddtask:false,
    }  
  },
  async created(){
    this.tasks = await this.getTask();
  },
  
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
