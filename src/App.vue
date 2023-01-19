<template>
  <div id="app">
    <AppLayout>
      <AppAddTask :tasks="tasks" @add-task="postToDo"/>
      <AppList :tasks="tasks" @delete-task="deleteToDo" @toggle-task="patchToDo" @edit-task="patchToDo"/>
    </AppLayout>
  </div>
</template>

<script>
import axios from 'axios';
import AppLayout from "@/components/AppLayout";
import AppAddTask from "@/components/AppAddTask";
import AppList from "@/components/AppList";

export default {
  name: 'App',
  data() {
    return {
      tasks: [],
      BASE_URL: String,
    }
  },
  created() {
    this.BASE_URL = process.env.VUE_APP_BASE_URL;
  },
  mounted() {
    this.getToDo();
  },
  methods: {
    addToDoLocal(task) {
      this.tasks.push(task);
    },
    deleteToDoLocal(id) {
      this.tasks = this.tasks.filter(el => el.id !== id);
    },
    replaceToDoLocal(task) {
      let index = this.tasks.findIndex(el => el.id === task.id);
      this.tasks[index] = task;
    },
    async getToDo() {
      try {
        const response = await axios.get(`${this.BASE_URL}/tasks`);
        this.tasks = response.data;
      } catch (e) {
        console.log('Ошибка запроса');
      }
    },
    async postToDo(data) {
      try {
        await axios.post(`${this.BASE_URL}/tasks`, data);
        // await this.getToDo();
        this.addToDoLocal(data);
      } catch (e) {
        console.log('Ошибка отправки');
      }
    },
    async deleteToDo(id) {
      try {
        await axios.delete(`${this.BASE_URL}/tasks/${id}`);
        // await this.getToDo();
        this.deleteToDoLocal(id);
      } catch (e) {
        console.log('Ошибка удаления');
      }
    },
    async patchToDo(task) {
      try {
        await axios.patch(`${this.BASE_URL}/tasks/${task.id}`, task);
        // await this.getToDo();
        this.replaceToDoLocal(task);
      } catch (e) {
        console.log('Ошибка изменения');
      }
    },
  },
  components: {
    AppList,
    AppLayout,
    AppAddTask,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  box-sizing: border-box;
}
</style>
