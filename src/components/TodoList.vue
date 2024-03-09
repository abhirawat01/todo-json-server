<template>
  <div class="container">
    <div class="row">
       <div class="col-9 mx-auto my-4">
        <input class="form-control rounded-pill p-2 px-4" placeholder="Enter New Todo..." type="text" v-model="newTask" @keyup.enter="addTask">
       </div>
    </div>
    <div class="row">
        <h2>Tasks</h2>
        <ul>
          <li class="d-flex align-items-center list-group-item gap-4 justify-content-center p-2" v-for="task in openTasks" :key="task.id">
            <input type="checkbox" v-model="task.completed" @change="updateTask(task)">
            <span :class="{ 'completed': task.completed }">{{ task.text }}</span>
            <span class="date">{{ task.createdAt }}</span>
            <button class="btn btn-success" @click="editTask(task)">Edit</button>
            <button class="btn btn-danger" @click="deleteTask(task)">Delete</button>
          </li>
        </ul>
    </div>
    <hr>
    <div class="row">
        <h2>Completed Tasks</h2>
    </div>
    <div class="row">
        <ul>
        <li class="d-flex align-items-center list-group-item gap-4 justify-content-center p-2" v-for="task in completedTasks" :key="task.id">
            <input type="checkbox" v-model="task.completed" @change="updateTask(task)">
            <span :class="{ 'completed': task.completed }">{{ task.text }}</span>
            <span class="date">{{ task.createdAt }}</span>
        </li>
    </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      tasks: [],
      newTask: ''
    };
  },
  computed: {
    openTasks() {
      return this.tasks.filter(task => !task.completed);
    },
    completedTasks() {
      return this.tasks.filter(task => task.completed);
    }
  },
  mounted() {
    console.log('inside mounted');
    this.getTasks();
  },
  methods: {
    async getTasks() {
      const response = await axios.get('http://localhost:3000/tasks');
      this.tasks = response.data;
    },
    async addTask() {
      if (this.newTask.trim() === '') return;
      const newTask = {
        text: this.newTask,
        completed: false,
        createdAt: new Date().toLocaleString()
      };
      const response = await axios.post('http://localhost:3000/tasks', newTask);
      this.tasks.push(response.data);
      this.newTask = '';
    },
    async updateTask(task) {
      await axios.put(`http://localhost:3000/tasks/${task.id}`, task);
    },
    async editTask(task) {
      const newText = prompt('Edit task:', task.text);
      if (newText !== null) {
        task.text = newText;
        await this.updateTask(task);
      }
    },
    async deleteTask(task) {
      const confirmed = confirm(`Are you sure you want to delete "${task.text}"?`);
      if (confirmed) {
        await axios.delete(`http://localhost:3000/tasks/${task.id}`);
        this.tasks = this.tasks.filter(t => t.id !== task.id);
      }
    }
  }
};
</script>

<style>
.completed {
  text-decoration: line-through;
}
</style>
