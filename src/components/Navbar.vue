<template>
  <div class="navbar navbar-light" id="navbar" width="100%">
    <div class="container d-flex justify-content-between">
      <div class="d-flex align-items-center">
        <svg
          style="color: white; margin: 5px"
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="16"
          fill="currentColor"
          class="bi bi-list-task"
          viewBox="0 0 16 16"
        >
          <path
            fill-rule="evenodd"
            d="M2 2.5a.5.5 0 0 0-.5.5v1a.5.5 0 0 0 .5.5h1a.5.5 0 0 0 .5-.5V3a.5.5 0 0 0-.5-.5H2zM3 3H2v1h1V3z"
          />
          <path
            d="M5 3.5a.5.5 0 0 1 .5-.5h9a.5.5 0 0 1 0 1h-9a.5.5 0 0 1-.5-.5zM5.5 7a.5.5 0 0 0 0 1h9a.5.5 0 0 0 0-1h-9zm0 4a.5.5 0 0 0 0 1h9a.5.5 0 0 0 0-1h-9z"
          />
          <path
            fill-rule="evenodd"
            d="M1.5 7a.5.5 0 0 1 .5-.5h1a.5.5 0 0 1 .5.5v1a.5.5 0 0 1-.5.5H2a.5.5 0 0 1-.5-.5V7zM2 7h1v1H2V7zm0 3.5a.5.5 0 0 0-.5.5v1a.5.5 0 0 0 .5.5h1a.5.5 0 0 0 .5-.5v-1a.5.5 0 0 0-.5-.5H2zm1 .5H2v1h1v-1z"
          />
        </svg>
        <a href="#" class="navbar-brand d-flex align-items-center" id="logo">
          Kanban
        </a>
      </div>
      <NavLogged v-if="page != 'register' && page != 'login'"
        :changePage="changePage"
        @postOrg="org"
        @logOut="$emit('logOut')"
        @pageOrg="pageOrg"
        :orgs="orgs"
        @getTask="getTask"
        ></NavLogged>
      <NavUnLogged v-else :page="page" :changePage="changePage"></NavUnLogged>
    </div>
  </div>
</template>

<script>
import NavLogged from "./NavLogged"
import NavUnLogged from './NavUnLogged';

export default {
  name: "Navbar",
  props: ['page', 'changePage', 'orgs', 'postOrg', 'loadOrg', 'getTask'],
  methods: {
    org(data) {
      this.postOrg(data);
    },
    pageOrg(data) {
      this.loadOrg(data);
    }
  },
  components: {
      NavLogged,
      NavUnLogged
  }
};
</script>

<style>
</style>