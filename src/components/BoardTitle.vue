<template>
  <div
    class="container d-flex flex-column flex-sm-row justify-content-between"
    style="color: gray; padding: 20px"
  >
    <h3>{{ orgData.name }}</h3>
    <div class="d-flex">
      <div class="dropdown">
        <button
          class="btn dropdown-toggle"
          type="button"
          id="dropdownMenuButton"
          data-bs-toggle="dropdown"
          aria-expanded="false"
        >
          Member
        </button>
        <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton">
          <li v-for="user in orgData.Users" :key="user.id">
            <a class="dropdown-item"
              >{{ user.username }}
              <svg
                @click="$emit('deleteMember', user.id)"
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                fill="currentColor"
                class="bi bi-person-dash-fill"
                viewBox="0 0 16 16"
              >
                <path
                  fill-rule="evenodd"
                  d="M11 7.5a.5.5 0 0 1 .5-.5h4a.5.5 0 0 1 0 1h-4a.5.5 0 0 1-.5-.5z"
                />
                <path
                  d="M1 14s-1 0-1-1 1-4 6-4 6 3 6 4-1 1-1 1H1zm5-6a3 3 0 1 0 0-6 3 3 0 0 0 0 6z"
                /></svg
            ></a>
          </li>
          <li>
            <a
              class="dropdown-item"
              style="color: gray"
              @click="getUsers(orgData.id)"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal2"
              >Add
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                fill="currentColor"
                class="bi bi-person-plus-fill"
                viewBox="0 0 16 16"
              >
                <path
                  d="M1 14s-1 0-1-1 1-4 6-4 6 3 6 4-1 1-1 1H1zm5-6a3 3 0 1 0 0-6 3 3 0 0 0 0 6z"
                />
                <path
                  fill-rule="evenodd"
                  d="M13.5 5a.5.5 0 0 1 .5.5V7h1.5a.5.5 0 0 1 0 1H14v1.5a.5.5 0 0 1-1 0V8h-1.5a.5.5 0 0 1 0-1H13V5.5a.5.5 0 0 1 .5-.5z"
                /></svg
            ></a>
          </li>
        </ul>
      </div>
      <div class="dropdown">
        <button
          class="btn dropdown-toggle"
          type="button"
          id="dropdownMenuButton"
          data-bs-toggle="dropdown"
          aria-expanded="false"
        >
          Settings
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="currentColor"
            class="bi bi-gear-fill"
            viewBox="0 0 16 16"
          >
            <path
              d="M9.405 1.05c-.413-1.4-2.397-1.4-2.81 0l-.1.34a1.464 1.464 0 0 1-2.105.872l-.31-.17c-1.283-.698-2.686.705-1.987 1.987l.169.311c.446.82.023 1.841-.872 2.105l-.34.1c-1.4.413-1.4 2.397 0 2.81l.34.1a1.464 1.464 0 0 1 .872 2.105l-.17.31c-.698 1.283.705 2.686 1.987 1.987l.311-.169a1.464 1.464 0 0 1 2.105.872l.1.34c.413 1.4 2.397 1.4 2.81 0l.1-.34a1.464 1.464 0 0 1 2.105-.872l.31.17c1.283.698 2.686-.705 1.987-1.987l-.169-.311a1.464 1.464 0 0 1 .872-2.105l.34-.1c1.4-.413 1.4-2.397 0-2.81l-.34-.1a1.464 1.464 0 0 1-.872-2.105l.17-.31c.698-1.283-.705-2.686-1.987-1.987l-.311.169a1.464 1.464 0 0 1-2.105-.872l-.1-.34zM8 10.93a2.929 2.929 0 1 1 0-5.86 2.929 2.929 0 0 1 0 5.858z"
            />
          </svg>
        </button>
        <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton">
          <li><a class="dropdown-item" @click="catForm">Add Category</a></li>
          <li><a class="dropdown-item" @click="$emit('getForm', 'admin')">Change Admin</a></li>
          <li><a class="dropdown-item" @click="$emit('getForm', 'edit')">Edit</a></li>
          <li><a class="dropdown-item" @click="$emit('deleteOrg')">Delete</a></li>
        </ul>
      </div>
    </div>
    <div
      class="modal fade"
      id="exampleModal2"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Add Member</h5>
          </div>
          <div class="modal-body">
            <form @submit.prevent="postMember">
              <label for="userId" class="form-label">Users: </label>
              <select class="form-select" aria-label="Default select example" v-model="userId">
                <option v-for="user in users" :key="user.id" :value="user.id">{{ user.username }}</option>
              </select>
              <button class="btn btn-primary" type="submit">Add</button>
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
  </div>
</template>

<script>
export default {
  data() {
      return {
          userId: null
      }
  },
  name: "BoardTitle",
  props: ["orgData", "getUsers", "users", 'add'],
  methods: {
      postMember() {
          this.add(this.userId);
          this.userId = null;
      },
      catForm() {
        swal({
            text: "Category Name:",
            content: "input",
            button: {
                text: "Save"
            }
        })
            .then((data) => {
                if (data == '') {
                    swal("Category name required!", "", "error");
                    this.catForm();
                } else if (!data) {
                    console.log('cancel');
                } else {
                    this.$emit('postCategory', data);
                    swal("Good job!", "Success create category!", "success");
                }
            });
      }
  },
};
</script>

<style>
</style>