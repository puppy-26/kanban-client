<template>
  <div class="container col-md-5" id="register-form">
    <h2>Register</h2>
    <form @submit.prevent="$emit('postRegister', user)">
      <label for="validationDefaultUsername" class="form-label">Username</label>
      <input
        v-model="user.username"
        type="text"
        class="form-control"
        id="validationDefaultUsername"
        aria-describedby="inputGroupPrepend2"
        required
      />
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
      <button type="submit" class="btn btn-primary">Sign Up</button>
    </form>
  </div>
</template>

<script>
export default {
    data() {
        return {
          user: {
            username: "",
            email: "",
            password: ""
          }
        }
    },
    name: "RegisterPage",
    methods: {
        
    }
};
</script>

<style>
</style>