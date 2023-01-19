<template>
  <el-form ref="form" class="form" @submit.native.prevent="addTask">
    <el-input class="input" placeholder="Please input task" v-model="form.input" type="text"></el-input>
    <el-button plain class="button" type="success" @click="addTask">Add</el-button>
  </el-form>
</template>

<script>
export default {
  name: 'AppAddTask',
  props: {
    tasks: []
  },
  data() {
    return {
      form: {
        input: ''
      }
    }
  },
  methods: {
    addTask() {
      if (this.form.input.trim() === "") return;
      let task = {
        id: Date.now(),
        taskContent: this.form.input,
        isCompleted: false,
      };
      this.$emit('add-task', task);
      this.form.input = "";
    }
  }
}
</script>

<style scoped>
.form {
  display: flex;
}

.input {
  flex: 1 1 auto;
  width: auto;
}

.button {
  margin-left: 16px;
  flex: 0 1 128px;
}
</style>