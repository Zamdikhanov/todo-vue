<template>

  <ul class="list" v-if="tasks.length > 0">
    <li v-for="task in tasks" class="list__item" :key="task.id">
      <div class="container">
        <el-checkbox v-model="task.isCompleted" @change="toggleTask(task,task.isCompleted)"></el-checkbox>
        <div v-if="task.id!==editedTaskID" class="task-text" @click="changeEditTaskID(task.id)">{{ task.taskContent }}
        </div>
        <el-form v-else ref="form" class="form" @submit.native.prevent="editTask(task)" >
          <el-input v-model="task.taskContent" type="text" class="input"/>
        </el-form>
      </div>
      <button class="el-icon-close button-delete" @click="deleteTask(task.id)"></button>

    </li>
  </ul>
  <h3 v-else>У вас нет задач</h3>
</template>

<script>
export default {
  name: 'AppList',
  props: {
    tasks: []
  },
  data() {
    return {
      editedTaskID: String,
    };
  },
  methods: {
    deleteTask: function (id) {
      this.$emit('delete-task', id);
    },
    toggleTask: function (task, isCompleted) {
      this.$emit('toggle-task', {...task, isCompleted: isCompleted});
    },
    changeEditTaskID: function (id) {
      this.editedTaskID = id;
    },
    editTask: function (task) {
      this.$emit('edit-task', task);
      this.editedTaskID='';
    }
  }
}
</script>

<style scoped>
.list {
  padding: 16px;
}

.list__item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  list-style-type: none;
  margin-bottom: 16px;
}

.container {
  display: flex;
  flex: 1 1 auto;
  align-items: center;
  text-align: center;
  margin-right: 16px;
}

.task-text {
  display: flex;
  align-items: center;
  margin: 0 16px;
  height: 40px;
}
.form {
  width: 100%;
  display: flex;
}
.input {
  margin: 0 0 0 16px;
  flex: 1 0 calc(100% - 16px);
}

.button-delete {
  padding: 0;
  background-color: transparent;
  border: none;
  color: red;
  cursor: pointer;
  font-size: 16px;
  transform: scale(1);
  transition: all 0.3s ease-in-out;
}

.button-delete:hover {
  transform: scale(1.2);
}
</style>