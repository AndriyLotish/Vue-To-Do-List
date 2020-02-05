<template>
  <div>
    <template v-if="!isEmpty">
      <div class="list-item" v-for="(item, key) in list" :key="key">
        <a class="circle" @click="addItemToDone(item.id)"></a>
        <span class="textItem" contenteditable="true">{{ item.name }}</span>
        <button @click="deleteItem(item.id)">🗑</button>
      </div>
    </template>
    <div v-else class="all__done">
      <div>🎊</div>
      <div class="all__done-done">All done!</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Tasks',
  props: {
    list: {
      type: Array,
      default: () => ({}),
    },
  },
  computed: {
    isEmpty() {
      return !Object.keys(this.list).length;
    },
  },
  methods: {
    deleteItem(id) {
      this.$emit('deleteItem', id);
    },
    addItemToDone(id) {
      this.$emit('addItemToDone', id);
    },
  },
};
</script>


<style>
.task-list {
  margin: 10px auto 40px;
  text-align: center;
  font-size: 1.7em;
  font-weight: 700;
}

.list-item {
  text-align: left;
  display: grid;
  grid-template-columns: 40px auto 40px;
  grid-gap: 15px;
  align-items: center;
  margin-bottom: 20px;
}

.circle {
  display: block;
  width: 25px;
  height: 25px;
  border: 1px solid aquamarine;
  border-radius: 25px;
  transition: 0.2s background;
}

.circle:hover,
.circle:focus {
  background: #7fffd440;
}

.textItem {
  width: max-content;
}

button {
  display: block;
  border: 1px solid rgb(255, 182, 193);
  width: 25px;
  height: 25px;
  padding: 0;
  padding-left: 3px;
  border-radius: 50%;
  transition: 0.2s box-shadow;
}

button:hover {
  border: 1px solid rgb(252, 169, 181);
  box-shadow: 0px 2px 20px rgba(28, 37, 73, 0.151);
}

.all__done {
  text-align: center;
  display: grid;
  font-size: 1.2em;
}

.all__done-done {
  margin-top: 10px;
  color: gray;
  text-align: center;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: 700;
  font-size: 1em;
}
</style>
