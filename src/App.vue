/* eslint-disable consistent-return */
<template>
  <div id="app">
    <div class="bcgImg"></div>
    <template v-if="isEmpty">
      <isEmptyTaskList @addItem="onSubmitData" />
    </template>
    <div v-else>
      <div class="sidebar" :style="{top: sidebar + '%'}"></div>
      <div class="wrapper">
        <h1 class="task-list">Task list</h1>
        <Tasks :list="list" @addItemToDone="onAddItemToDone" @deleteItem="onDeleteItem" />
      </div>
      <Input @submit="onSubmitData" />
      <div class="tasksDone">
        <h2>Tasks Done</h2>
        <TasksDone
          :tasksDoneList="tasksDoneList"
          @addItemToNotDone="onAddItemToNotDone"
          @deleteItem="onDeleteDoneItem"
        />
      </div>
      <percentDoneWork
        :list="list"
        :tasksDoneList="tasksDoneList"
        :count="count"
        :incomplete="incomplete"
        :percentDone="percentDone"
      />
    </div>
    <footer class="copyright">
      <p>
        &copy; 2019
        <a href="https://digitalidea.studio/" target="_blank">Digital Idea</a>
      </p>
    </footer>
  </div>
</template>

<script>
import Tasks from './components/Tasks.vue';
import Input from './components/Input.vue';
import TasksDone from './components/TasksDone.vue';
import isEmptyTaskList from './components/isEmptyTaskList.vue';
import percentDoneWork from './components/percentDoneWork.vue';

export default {
  name: 'app',
  components: {
    Tasks,
    Input,
    TasksDone,
    isEmptyTaskList,
    percentDoneWork,
  },
  data: () => ({
    formData: {
      comment: '',
    },
    list: [],
    tasksDoneList: [],
    percent: '',
  }),
  created() {
    const tasks = JSON.parse(localStorage.getItem('tasks'));
    if (tasks !== null) {
      tasks.forEach((task) => {
        if (task.checked === false) {
          this.list.push(task);
        } else {
          this.tasksDoneList.push(task);
        }
      });
    }
  },
  methods: {
    onDeleteItem(id) {
      this.list = this.list.filter(task => task.id !== id);
      localStorage.setItem('tasks', JSON.stringify(this.list));
    },
    onDeleteDoneItem(id) {
      this.tasksDoneList = this.tasksDoneList.filter(task => task.id !== id);
      localStorage.setItem('tasks', JSON.stringify(this.tasksDoneList));
    },
    onAddItemToDone(id) {
      const tasks = JSON.parse(localStorage.getItem('tasks'));
      tasks.forEach((task) => {
        if (task.id === id) {
          // eslint-disable-next-line no-param-reassign
          task.checked = true;
          console.log(task);
          this.tasksDoneList.push(task);
          this.list = this.list.filter(taskDone => taskDone.id !== id);
        }
      });
      localStorage.setItem('tasks', JSON.stringify(tasks));
    },
    onAddItemToNotDone(id) {
      const tasks = JSON.parse(localStorage.getItem('tasks'));
      tasks.forEach((task) => {
        if (task.id === id) {
          // eslint-disable-next-line no-param-reassign
          task.checked = false;
          this.list.push(task);
          this.tasksDoneList = this.tasksDoneList.filter(
            taskDone => taskDone.id !== id,
          );
        }
      });
      localStorage.setItem('tasks', JSON.stringify(tasks));
    },
    onSubmitData({ task }) {
      this.list.push(task);
    },
  },
  computed: {
    count() {
      return this.list.length + this.tasksDoneList.length;
    },
    incomplete() {
      return +this.count - this.list.length;
    },
    // eslint-disable-next-line vue/return-in-computed-property
    percentDone() {
      if (this.incomplete === this.count && this.count !== 0) {
        return 100;
      }
      if (this.incomplete < this.count) {
        return Math.round((this.incomplete / this.count) * 100);
      }
      return 0;
    },
    sidebar() {
      return 100 - this.percentDone;
    },
    isEmpty() {
      if (this.list.length + this.tasksDoneList.length === 0) {
        return true;
      }
      return false;
    },
  },
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
  transition: all 0.5s;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  box-sizing: border-box;
}

:root {
  background-color: #2c3e50;
}

.bcgImg {
  position: absolute;
  left: 0;top: 0;right: 0;bottom: 0;
  background-image: url("assets/Write tasks for yourself.png");
  background-repeat: repeat;
  z-index: 0;
}

.sidebar {
  position: absolute;
  left: 0;
  /* top: 0; */
  right: 0;
  bottom: 0;
  background-color: #f5d76e;
  background-size: cover;
  transition: all 0.5s;
  z-index: -1;
}

.wrapper {
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

.copyright {
  position: absolute;
  right: 5px;
  bottom: 10px;
  color: #fff;
  font-size: 12px;
  font-family: Arial, Helvetica, sans-serif;
}

.copyright p a {
  color: #fff;
  font-size: 12px;
  font-family: Arial, Helvetica, sans-serif;
}
</style>
