<template>
  <div class="">
    <h1 class="text-center bg-primary text-white p-3 mb-5">{{ title }}'s todo list</h1>

    <div class="container bg-info p-5 rounded">
      <div class="row my-5">
        <div class="col">
          <input type="text" class="form-control" v-model="newTask" @keyup.enter="addTask"/>
        </div>
        <div class="col">
          <input type="button" class="btn btn-secondary" value="Add" @click="addTask">
        </div>
        <div class="col">
          <button class="btn btn-danger" @click="deleteTask">Delete done tasks</button>
        </div>
      </div>

      <div class="">
        <div class="row">
          <div class="col fs-3 mb-4">Tasks</div>
          <div class="col-2 fs-3 mb-4">Done</div>
        </div>

        <div class="row" v-for="(task, index) in filterTask" :key="index">
          <div class="col" :class="task.done ? 'delete' : ''">{{ task.action }}</div>
          <div class="col-2">
            <input type="checkbox" v-model="task.done"/>
          </div>
        </div>
      </div>

      <div class="alert alert-warning text-center" v-if="tasks.length == 0">There is no data</div>

      <div class="bg-secondary text-center text-white py-2 row mt-3">
        <h5 class="col">Hide Completed Tasks</h5>
        <input type="checkbox" :class="'hide-check'" class="col" v-model="hideCompleted" />
      </div>
    </div>
  </div>

</template>

<script>
export default{
  name : 'App',
  data: () => ({
    title: "HPM",
    hideCompleted: false,
    newTask: '',
    tasks: []
  }),
  computed: {
    filterTask() {
      return this.hideCompleted ? this.tasks.filter( (v) => !v.done) : this.tasks;
    }
  },
  methods: {
    addTask() {

      if (this.newTask !== '') {
        this.tasks.push({
        action: this.newTask,
        done: false
      } 
      );

      this.storeData();

      this.newTask = '';
      } else {
        alert ("Please add a task!");
      }
    },
    deleteTask() {
      this.tasks = this.tasks.filter((v) => !v.done);

      this.storeData();
    },
    storeData() {
      localStorage.setItem("myLocalTasks", JSON.stringify(this.tasks));
    }
  },
  mounted() {
    let data = localStorage.getItem("myLocalTasks");
    
    if (data !== null) {
      this.tasks = JSON.parse(data);
    }
  }

}
</script>

<style>
  .delete {
    text-decoration: line-through;
  }

  input.hide-check {
  
  }
</style>