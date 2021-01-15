<template>
  <div>
    <div class="d-flex justify-content-around align-items-center">
      <a
        class="nav-link"
        v-bind:id="page == 'register' ? 'activate' : ''"
        aria-current="page"
        @click="changePage('register')"
        >Register</a
      >
      <a
        class="nav-link"
        v-bind:id="page == 'login' ? 'activate' : ''"
        aria-current="page"
        @click="changePage('login')"
        >Login</a
      >
    </div>
  </div>
</template>

<script>
export default {
    name: "NavUnLogged",
    props: ['page', 'changePage']
};
</script>

<style>
</style>