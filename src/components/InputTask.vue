<template>
    <div class="tasks__new">
      <div :class="{ 'form-group--error': $v.formData.comment.$error }">
      <input
        id="new-task"
        type="text"
        placeholder="New task..."
        @keyup.enter="onSubmit"
        class="task__new-input"
        v-model="formData.comment"
      />
      </div>
      <div v-if="!$v.formData.comment.$error">
        <div class="error" v-if="$v.formData.comment.required">Field is required</div>
      </div>
    </div>
</template>

<script>
import { required } from 'vuelidate/lib/validators';

export default {
  name: 'InputTask',
  props: {
    list: {
      type: Object,
      default: () => ({}),
    },
  },
  data() {
    return {
      formData: {
        comment: '',
      },
    };
  },
  validations: {
    formData: {
      comment: { required },
    },
  },
  methods: {
    onSubmit() {
      if (this.$v.formData.$invalid === true) return;
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
  position: relative;
  margin: 10px auto;
  width: 45%;
  height: 1em;
  padding: 20px;
  border-radius: 5px;
  background-color: #fff;
  opacity: 0.95;
  overflow: hidden;
}

.task__new-input {
  width: 90%;
  margin: 0 10px 10px 0;
  position: absolute;
  justify-content: center;
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

.error {
  position: absolute;
  color: rgba(247, 157, 157, 0.671);
  font-size: 11px;
  bottom: 0;
}
</style>
