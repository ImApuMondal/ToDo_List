<template>
  <div class="container">
    <h2 class="text-center mt-5">My To Do App</h2>

    <!-- Input -->
    <div class="d-flex">
      <input v-model="task" type="text" placeholder="Enter Your Task Here.." name="" id="" class="form-control">
      <button @click="submitTask" class="btn btn-warning rounded-1 ml">ADD</button>
    </div>

  <!-- Table -->
  <table class="table table-bordered mt-5">
  <thead>
    <tr>
      <th scope="col">Task Details</th>
      <th scope="col">Change Status</th>
      <th scope="col" class="text-center">Edit Task</th>
      <th scope="col" class="text-center">Remove Task</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(task, index) in tasks" :key="index">
      <td>
        <span :class="{'finished': task.status === 'finished' }">{{task.name}}</span>
      </td>
      <td style="width: 120px;">
        <span @click="changeStatus(index)" class="pointer"
        :class="{'text-danger': task.status === 'to-do',
        'text-warning': task.status === 'in-progress',
        'text-success': task.status === 'finished'
        }"
        >
          {{ firstCharUpper(task.status) }}
        </span>
      </td>
      <td style="width: 120px;">
        <div class="text-center" @click="editTask(index)">
          <span class="fa fa-pen pointer"></span>
        </div>
      </td>
      <td style="width: 120px;">
        <div class="text-center" @click="deleteTask(index)">
          <span class="fa fa-trash pointer"></span>
        </div>
      </td>
    </tr>
    
  </tbody>
</table>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  data(){
    return{
      task:'',
      editedTask: null,
      availableStatuses: ['to-do','in-progress','finished'],

      tasks:[
        {
          name: 'Task 1',
          status:'to-do'
        },
        {
          name:'Task 2',
          status:'in-progress'
        }
      ]
    }
  },

  methods:{
    submitTask(){
      // console.log(this.task)
      if(this.task.length === 0) return;

      if(this.editedTask == null){
        this.tasks.push({
        name: this.task,
        status: 'to-do'
        });  
      }else{
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      

      this.task = '';
    },
    deleteTask(index){
    this.tasks.splice(index, 1);
    },

    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },

    firstCharUpper(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }
};
</script>

<style scoped>
.pointer{
  cursor: pointer;
}
.finished{
  text-decoration: line-through;
}
.ml{
  margin-left: 10px;
}
</style>