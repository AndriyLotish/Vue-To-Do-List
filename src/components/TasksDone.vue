<template>
  <div>
    <template v-if="isNotEmpty">
      <template v-for="(item, index) in tasksDoneList">
        <div
          v-if="index < indexEl"
          :key="index"
          class="list-item"
        >
          <a
            class="circle__done"
            @click="addItemToNotDone(item.id)"
          />
          <span class="task__done-title">{{ item.name }}</span>
          <button @click="deleteItem(item)">
            🗑
          </button>
        </div>
        <div
          v-if="index === indexEl"
          :key="index"
          class="show__more"
        >
          <a @click="showMore">Show more...</a>
        </div>
      </template>
    </template>
    <div v-else>
      You have no completed tasks.
    </div>
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
  data: () => ({
    indexEl: 5,
  }),
  computed: {
    isNotEmpty() {
      return Object.keys(this.tasksDoneList).length;
    },
  },
  methods: {
    deleteItem(item) {
      this.$emit('modalOpened', item);
    },
    addItemToNotDone(id) {
      this.$emit('addItemToNotDone', id);
    },
    showMore() {
      this.indexEl += 5;
    },
  },
};
</script>

<style>
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

.show__more {
  display: grid;
  justify-content: center;
}

.show__more a:hover {
  text-decoration: underline;
}
</style>
