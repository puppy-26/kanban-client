<template>
  <div class="d-flex flex-column align-items-stretch">
    <div class="container">
      <TaskList
        :tasks="tasks"
        ></TaskList>
    </div>
  </div>
</template>

<script>
import TaskList from "./TaskList"

export default {
  name: "KanbanBoard",
  props: ["tasks"],
  components: {
      TaskList
  }
};
</script>

<style>
</style>