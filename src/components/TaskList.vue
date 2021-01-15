<template>
  <div id="task-list">
    <h5>Universal Board</h5>
    <ul class="list-group list-group-flush">
      <li v-for="task in tasks" :key="task.id" class="list-group-item d-flex flex-column flex-sm-row align-items-baseline justify-content-between" id="task-con">
          <h3>{{ task.title }}</h3>
          <p>By: {{ task.User.username }}</p>
          <p>Board: {{ task.Organization.name }}</p>
          <p>Category: {{ task.Category.name }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "TaskList",
  props: ['tasks']
};
</script>

<style>
#task-list {
    display: flex;
    flex-direction: column;
    align-items: stretch;
    width: 100%;
}

h5 {
    padding: 20px;
    padding-top: 30px;
}
</style>