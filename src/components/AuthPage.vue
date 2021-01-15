<template>
  <div>
    <div class="d-flex row" id="first-page">
      <div class="container col-md-7" id="about">
        <p>
          Kanban is a workflow management method for defining, managing and
          improving services that deliver knowledge work. It aims to help you
          visualize your work, maximize efficiency, and improve continuously.
        </p>
      </div>
      <RegisterPage v-if="page == 'register'" @postRegister="register"></RegisterPage>
      <LoginPage v-if="page == 'login'" @postLogin="login"></LoginPage>
    </div>
  </div>
</template>

<script>
import LoginPage from "./LoginPage";
import RegisterPage from "./RegisterPage"

export default {
  name: "AuthPage",
  props: ["page", "postRegister", "postLogin"],
  methods: {
    register(data) {
      this.postRegister(data);
    },
    login(data) {
      this.postLogin(data);
    }
  },
  components: {
    LoginPage,
    RegisterPage
  },
};
</script>

<style>
</style>