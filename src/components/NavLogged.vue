<template>
  <div>
    <div class="d-flex justify-content-around align-items-center">
      <a class="nav-link text-light" aria-current="page" @click="changePage('home'); $emit('getTask')">Home</a>
      <div class="nav-link btn-group">
        <button
          id="dropdown-nav"
          type="button"
          class="btn dropdown-toggle text-light"
          data-bs-toggle="dropdown"
          aria-expanded="false"
        >
          Account
        </button>
        <ul class="dropdown-menu">
          <li>
            <h6 class="dropdown-header">Organizations:</h6>
          </li>
          <li v-for="org in orgs" :key="org.id">
            <a class="dropdown-item" @click="$emit('pageOrg', org)">{{ org.name }}</a>
          </li>
          <li>
            <a class="dropdown-item">
              <button
                type="button"
                class="btn"
                data-bs-toggle="modal"
                data-bs-target="#exampleModal"
                style="color: gray"
              >
                Add
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="16"
                  height="16"
                  fill="currentColor"
                  class="bi bi-plus-circle"
                  viewBox="0 0 16 16"
                >
                  <path
                    d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"
                  />
                  <path
                    d="M8 4a.5.5 0 0 1 .5.5v3h3a.5.5 0 0 1 0 1h-3v3a.5.5 0 0 1-1 0v-3h-3a.5.5 0 0 1 0-1h3v-3A.5.5 0 0 1 8 4z"
                  />
                </svg></button
            ></a>
          </li>
          <li>
            <hr class="dropdown-divider" />
          </li>
          <li>
            <a class="dropdown-item" @click="$emit('logOut')"
              >Log Out
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                fill="currentColor"
                class="bi bi-box-arrow-right"
                viewBox="0 0 16 16"
              >
                <path
                  fill-rule="evenodd"
                  d="M10 12.5a.5.5 0 0 1-.5.5h-8a.5.5 0 0 1-.5-.5v-9a.5.5 0 0 1 .5-.5h8a.5.5 0 0 1 .5.5v2a.5.5 0 0 0 1 0v-2A1.5 1.5 0 0 0 9.5 2h-8A1.5 1.5 0 0 0 0 3.5v9A1.5 1.5 0 0 0 1.5 14h8a1.5 1.5 0 0 0 1.5-1.5v-2a.5.5 0 0 0-1 0v2z"
                />
                <path
                  fill-rule="evenodd"
                  d="M15.854 8.354a.5.5 0 0 0 0-.708l-3-3a.5.5 0 0 0-.708.708L14.293 7.5H5.5a.5.5 0 0 0 0 1h8.793l-2.147 2.146a.5.5 0 0 0 .708.708l3-3z"
                /></svg
            ></a>
          </li>
        </ul>
      </div>
    </div>
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-body">
            <form class="row g-3" style="color: black; padding: 10px;" @submit.prevent="$emit('postOrg', name)">
              <label for="name" class="form-label">Add your new organization name:</label>
              <input type="text" class="form-control" required v-model="name">
              <button type="submit" class="btn btn-primary"
                style="margin-top: 10px;"
                data-bs-toggle="modal"
                data-bs-target="#exampleModal"
                >Add</button>
            </form>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Close
            </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: ""
    }
  },
  name: "NavLogged",
  props: ["orgs", 'changePage'],
};
</script>

<style>
</style>