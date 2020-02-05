<template>
  <div>
    <template v-if="isEmpty">
      <div class="list-item" v-for="(item, index) in tasksDoneList" :key="index">
        <a class="circle__done" @click="addItemToNotDone(item.id)"></a>
        <span class="task__done-title">{{ item.name }}</span>
        <button @click="deleteItem(item.id)">🗑</button>
      </div>
    </template>
    <div v-else class="no__tasks">You have no completed tasks.</div>
  </div>
</template>

<script>
export default {
  name: 'TasksDone',
  props: {
    tasksDoneList: {
      type: Array,
      default: () => ({}),
    },
  },
  computed: {
    isEmpty() {
      return Object.keys(this.tasksDoneList).length;
    },
  },
  methods: {
    deleteItem(id) {
      this.$emit('deleteItem', id);
    },
    addItemToNotDone(id) {
      this.$emit('addItemToNotDone', id);
    },
  },
};
</script>

<style>
.tasksDone {
  margin: 0 auto;
  width: 45%;
  padding: 20px;
  border-radius: 5px;
  background-color: #fff;
  opacity: 0.95;
}

h2 {
  margin: 10px auto 40px;
  text-align: center;
  font-size: 1.7em;
  font-weight: 700;
}

.circle__done {
  display: block;
  width: 25px;
  height: 25px;
  border-radius: 25px;
  background: aquamarine;
  border: 1px solid #6cdcb6;
  transition: 0.2s background;
}

.circle__done:hover {
  background: rgb(86, 255, 199);
}

.task__done-title {
  text-decoration: line-through;
  font-size: 18px;
}
</style>
