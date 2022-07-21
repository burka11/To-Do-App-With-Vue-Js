<template>
  <div class="container">
    <h2 class="text-center mt-5">My vue Todo App</h2>

    <!-- Input -->
    <div class="d-flex">
      <form @submit.prevent="submitTask">
        <input v-model="task" type="text" placeholder="Enter task" class="form-control" style="width:1100px"> 
        <button style="margin:5px" @click="handleClick('to-do')" class="btn btn-warning rounded-20">TO DO</button>
        <button style="margin:5px" @click="handleClick('in-progress')" class="btn btn-warning rounded-20">IN PROGRESS</button>
        <button style="margin:5px" @click="handleClick('finished')" class="btn btn-warning rounded-20">FINISHED</button>
        <!-- <br><br> -->
        <button style="margin-left:50px" class="btn btn-success rounded-20">SUBMIT</button> 
        <button style="margin-left:15px" @click="GetAll()" class="btn btn-danger rounded-20">CLEAR FILTER</button> 
        
      </form>

    </div>

    <!--Task Table -->
<hr>
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" classes="text-center">#</th>
          <th scope="col" classes="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in filteredTasks" :key="index">
          <td>
            <span
              :class="{ 'finished': task.status === 'finished', 'in-progress': task.status === 'in-progress' }">{{ task.name }}</span>
          </td>
          <td style="width:120px"><span @click="changeStatus(index)" class="pointer"
              :class="{ 'text-danger': task.status === 'to-do', 'text-success': task.status === 'finished', 'text-warning': task.status === 'in-progress' }">{{ firstCharUpper(task.status) }}</span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)"><span class="fa fa-pen"></span></div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>

          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'TodoApp',
  props: {
    msg: String,
  },

  data() {
    return {
      task: '',
      editedTask: null,
      filteredStatus: '',
      availableStatuses: ['to-do', 'in-progress', 'finished'],
      tasks: [
        {
          name: 'Steel bananas from the store.',
          status: 'to-do'
        },
        {
          name: 'Eat 1kg chocolate in 1 hour.',
          status: 'to-do'
        },
        {
          name: 'Eat 1kg chocolate in 1 hour.',
          status: 'in-progress'
        },
        {
          name: 'Eat 1kg chocolate in 1 hour.',
          status: 'in-progress'
        },
        {
          name: 'Eat 1kg chocolate in 1 hour.',
          status: 'in-progress'
        },
        {
          name: 'Eat 1kg chocolate in 1 hour.',
          status: 'finished'
        },
      ],
      filteredTasks: []
    }
  },
  mounted(){
    this.filteredTasks = this.tasks;
  },  
  methods: {
    handleClick(status){
      this.filteredTasks = this.tasks.filter(item => item.status === status);
      // console.log(this.filteredTasks);
    },  
    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        });
      }
      else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = '';
    },


    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
    GetAll() {
    this.filteredTasks = this.tasks;

    }

  }
};
</script>


<style scoped>
.pointer {
  cursor: pointer;

}

.finished {
  text-decoration: line-through;
  text-decoration-color: blue;
}



td {
  transition: 300ms all;
}

td:hover {
  cursor: pointer;
  background-color: burlywood;
}

div {
  background-color: beige;
}

h2 {
  background-color: white;
}
</style>
