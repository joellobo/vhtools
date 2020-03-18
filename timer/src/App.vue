<template>
  <div id="app">
    <task-list :tasks="tasks"></task-list>
  </div>
</template>

<script>
import TaskList from "./components/task-list";

export default {
  name: "app",
  components: { TaskList }
};
</script>

<style>
</style>
