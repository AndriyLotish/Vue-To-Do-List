<template>
  <div>
    <div class="percent__work">
      <div>
        <span>{{ complete }}</span>
        /
        <span>{{ count }}</span>
        (
        <span
          class="percent"
          :class="(percentDone === 0) ? 'black' :
            (percentDone > 0 && percentDone < 50) ? 'red' :
            (percentDone >= 50 && percentDone < 100) ? 'orange' :
            'green'"
        >{{ percentDone }}</span>
        <span>done</span>
        )
      </div>
    </div>
    <div>
      <div
        v-if="modalOpened"
        class="modal"
      >
        <div class="modal__wrapper">
          <h3 class="modal__header">
            Are you sure you want to delete task "{{ itemForDelete.name }}"?
          </h3>
          <div class="modal__footer">
            <button
              class="modal__yes"
              @click="deleteItem(itemForDelete.id)"
            >
              Yes
            </button>
            <button
              class="modal__no"
              @click="closeModal"
            >
              No
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PercentDoneWork',
  props: [
    'list',
    'tasksDoneList',
    'count',
    'complete',
    'percentDone',
    'modalOpened',
    'itemForDelete',
  ],
  methods: {
    closeModal() {
      // this.modalOpened = false;
      this.$emit('modalClose');
    },
    deleteItem() {
      this.$emit('deleteItem', this.itemForDelete.id);
      this.$emit('modalClose');
    },
  },
};
</script>

<style>
.modal {
  position: fixed;
  left: 0;top: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(10, 10, 10, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal__wrapper{
  background-color: #fff;
  width: 45%;
  padding: 20px;
  border-radius: 5px;
  opacity: 0.95;
}

.modal__footer {
  display: flex;
  justify-content: center;
}

.modal__yes, .modal__no{
  padding: 5px;
  margin: 5px;
  border: 1px solid #dfdfdf;
  border-radius: 2px;
  min-width: 50px;
  font-size: 16px;
  cursor: pointer;
}

.percent__work {
  margin: 10px auto;
  width: 45%;
  padding: 20px;
  border-radius: 5px;
  background-color: #fff;
  opacity: 0.95;
}

.percent::after {
  content: "% ";
}

.black {
  color: black;
}

.red {
  color: red;
}

.orange {
  color: orange;
}

.green {
  color: green;
}
</style>
