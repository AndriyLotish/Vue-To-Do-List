<template>
  <div class="isEmpty">
    <div class="emptyToDo">
      <h1 class="task-list">Task list</h1>
      <Input @submit="onSubmitData" />
    </div>
    <div class="span"></div>
  </div>
</template>

<script>
import Input from './Input.vue';

export default {
  name: 'isEmptyTaskList',
  components: {
    Input,
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

.span {
  background-image: url("../assets/Write tasks for yourself.png");
  background-repeat: repeat;
}
</style>
