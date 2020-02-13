<template>
  <div id="app">
    <div class="bcgClr" />
    <div class="bcgImg" />
    <template v-if="isNotEmpty">
      <EmptyTaskList @addItem="onSubmitData" />
    </template>
    <div v-else>
      <div
        class="sidebar"
        :style="{ top: sidebar + '%' }"
      />
      <div class="wrapper">
        <h1>Task list</h1>
        <TasksList
          :list="list"
          @addItemToDone="onAddItemToDone"
          @modalOpened="onModalOpened"
          @editName="onEditName"
          @editNameOnLocalStorage="onEditNameOnLocalStorage"
        />
      </div>
      <InputTask @submit="onSubmitData" />
      <div class="tasksDone">
        <h2>Tasks Done</h2>
        <TasksDone
          :tasks-done-list="tasksDoneList"
          @addItemToNotDone="onAddItemToNotDone"
          @modalOpened="onModalOpened"
        />
      </div>
      <PercentDoneWork
        :list="list"
        :tasks-done-list="tasksDoneList"
        :count="count"
        :complete="complete"
        :percent-done="percentDone"
        :modal-opened="modalOpened"
        :item-for-delete="itemForDelete"
        @deleteItem="onDeleteItem"
        @modalClose="onModalClose"
      />
    </div>
    <footer class="copyright">
      <p>
        &copy; 2019
        <a
          href="https://digitalidea.studio/"
          target="_blank"
        >Digital Idea</a>
      </p>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {
    TasksList: () => import('./components/TasksList.vue'),
    InputTask: () => import('./components/InputTask.vue'),
    TasksDone: () => import('./components/TasksDone.vue'),
    EmptyTaskList: () => import('./components/EmptyTaskList.vue'),
    PercentDoneWork: () => import('./components/PercentDoneWork.vue'),
  },
  data: () => ({
    formData: {
      comment: '',
    },
    list: [],
    tasksDoneList: [],
    percent: '',
    modalOpened: false,
    itemForDelete: {},
    nameEdit: '',
  }),
  computed: {
    count() {
      return this.list.length + this.tasksDoneList.length;
    },
    complete() {
      return this.count - this.list.length;
    },
    percentDone() {
      if (this.complete === this.count && this.count !== 0) {
        return 100;
      }
      if (this.complete < this.count && this.complete !== 0) {
        return Math.round((this.complete / this.count) * 100);
      }
      return 0;
    },
    sidebar() {
      return `${100 - this.percentDone}`;
    },
    isNotEmpty() {
      return !(this.list.length + this.tasksDoneList.length);
    },
  },
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
    onDeleteItem() {
      this.list = this.list.filter(task => task.id !== this.itemForDelete.id);
      this.tasksDoneList = this.tasksDoneList.filter(task => task.id !== this.itemForDelete.id);
      const tasks = this.list.concat(this.tasksDoneList);
      localStorage.setItem('tasks', JSON.stringify(tasks));
    },
    onAddItemToDone(id) {
      const tasks = JSON.parse(localStorage.getItem('tasks'));
      tasks.forEach((task) => {
        if (task.id === id) {
          const taskItem = task;
          taskItem.checked = true;
          this.tasksDoneList.unshift(task);
          this.list = this.list.filter(taskDone => taskDone.id !== id);
        }
      });
      localStorage.setItem('tasks', JSON.stringify(tasks));
    },
    onAddItemToNotDone(id) {
      const tasks = JSON.parse(localStorage.getItem('tasks'));
      tasks.forEach((task) => {
        const taskItem = task;

        if (task.id === id) {
          taskItem.checked = false;
          this.list.push(task);
          this.tasksDoneList = this.tasksDoneList.filter(taskDone => taskDone.id !== id);
        }
      });
      localStorage.setItem('tasks', JSON.stringify(tasks));
    },
    onSubmitData({ task }) {
      this.list.push(task);
    },
    onModalOpened(item) {
      this.itemForDelete = item;
      this.modalOpened = true;
    },
    onModalClose() {
      this.modalOpened = false;
    },
    onEditName(e) {
      this.nameEdit = e;
    },
    onEditNameOnLocalStorage(item) {
      const tasks = JSON.parse(localStorage.getItem('tasks'));
      const thisName = this;
      tasks.forEach((task) => {
        if (task.id === item.id) {
          const taskItem = task;
          taskItem.name = thisName.nameEdit;
        }
      });
      this.list.forEach((task) => {
        if (task.id === item.id) {
          const taskItem = task;
          taskItem.name = thisName.nameEdit;
        }
      });
      localStorage.setItem('tasks', JSON.stringify(tasks));
    },
  },
};
</script>

<style scoped>
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

.bcgClr {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  background-color: #2c3e50;
  z-index: -2;
}

.bcgImg {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
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

.tasksDone {
  margin: 0 auto;
  width: 45%;
  padding: 20px;
  border-radius: 5px;
  background-color: #fff;
  opacity: 0.95;
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
