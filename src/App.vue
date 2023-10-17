
<template>
  <div id="app">
    <KanbanBoard />
  </div>
</template>

<script>
import KanbanBoard from './components/KanbanBoard.vue';

export default {
  name: 'App',
  components: {
    KanbanBoard,
  },
};
</script>

<style>
.title {
  text-align: center;
  margin-top: 2rem;
}
body{
  background-color: #ddd!important;
}
</style>