<template>
  <div>
    <div class="tasks__new">
      <input
        lable-position="bottom"
        id="new-task"
        type="text"
        @keyup.enter="onSubmit"
        placeholder="New task..."
        class="task__new-input"
        v-model="formData.comment"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Input',
  props: {
    list: {
      type: Object,
      default: () => ({}),
    },
  },
  data: () => ({
    formData: {
      comment: '',
    },
  }),
  methods: {
    onSubmit() {
      if (localStorage.getItem('tasks') === null) {
        localStorage.setItem('tasks', '[]');
      }
      const tasks = JSON.parse(localStorage.getItem('tasks'));
      let id;
      if (tasks.length === 0) {
        id = 1;
      } else {
        const taskLastEl = tasks.length;
        id = taskLastEl + 1;
      }
      const task = {
        id,
        name: this.formData.comment,
        checked: false,
        editing: false,
      };
      tasks.push(task);
      localStorage.setItem('tasks', JSON.stringify(tasks));

      this.$emit('submit', { task });
      this.formData.comment = '';
    },
  },
};
</script>

<style>
.tasks__new {
  display: flex;
  margin: 10px auto;
  width: 45%;
  padding: 20px;
  border-radius: 5px;
  background-color: #fff;
  opacity: 0.95;
}

.task__new-input {
  width: 95%;
  font-size: 18px;
  background: none;
  border: none;
  border-bottom: 1px dashed lightgray;
  color: #9a9999;
  padding: 1px 5px;
}

.task__new-input::placeholder {
  color: #d3d3d3;
}

.task__new-input:active,
.task__new-input:focus {
  border-bottom: 1px solid #9a9999;
  outline: none !important;
}
</style>
