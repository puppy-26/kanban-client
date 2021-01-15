<template>
  <div class="container col-md-5" id="register-form">
    <h2>Login</h2>
    <form @submit.prevent="$emit('postLogin', user)">
      <label for="exampleInputEmail1" class="form-label">Email address</label>
      <input
        v-model="user.email"
        type="email"
        class="form-control"
        id="exampleInputEmail1"
        aria-describedby="emailHelp"
        required
      />
      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">Password</label>
        <input
          v-model="user.password"
          type="password"
          class="form-control"
          id="exampleInputPassword1"
          required
        />
      </div>
      <button type="submit" class="btn btn-primary">Sign In</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "LoginPage",
  data() {
    return {
      user: {
        email: "",
        password: ""
      }
    }
  }
};
</script>

<style>
</style>