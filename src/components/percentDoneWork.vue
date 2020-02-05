<template>
  <div class="percent__work">
    <div class="task__line">
      <span class="incomplete">{{ incomplete }}</span>
      /
      <span class="count">{{ count }}</span>
      (
      <span class="percent black red yellow green">{{ percentDone }}</span>
      <span class="done">done</span>
      )
    </div>
  </div>
</template>

<script>
export default {
  props: ['list', 'tasksDoneList', 'count', 'incomplete', 'percentDone'],
  name: 'percentDoneWork',
};
</script>

<style>
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
</style>
