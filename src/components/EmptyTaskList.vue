<template>
  <div class="isEmpty">
    <div class="emptyToDo">
      <h1 class="task-list">
        Task list
      </h1>
      <InputTask @submit="onSubmitData" />
    </div>
  </div>
</template>

<script>
import InputTask from './InputTask.vue';

export default {
  name: 'IsEmptyTaskList',
  components: {
    InputTask,
  },
  methods: {
    onSubmitData({ task }) {
      this.$emit('addItem', { task });
    },
  },
};
</script>

<style>
.isEmpty {
  position: relative;
  height: 100%;
}

.emptyToDo {
  position: relative;
  margin: 180px;
  background-color: #fff;
  opacity: 0.95;
  width: 45%;
  margin: 60px auto 10px;
  padding: 20px;
  font-family: Arial, Helvetica, sans-serif;
  border-radius: 5px;
}

.span {
  position: absolute;
  display: flex;
  flex-direction: column;
  z-index: -1;
  height: 100vh;
  top: -60px;
  left: 0;
  right: 0;
  bottom: 0;
}
</style>
