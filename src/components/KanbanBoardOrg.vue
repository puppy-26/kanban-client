<template>
  <div class="d-flex flex-column align-items-center">
    <BoardTitle
      :orgData="orgData"
      :getUsers="getUsers"
      :users="users"
      :add="add"
      @deleteMember="deleteMember"
      @postCategory="postCategory"
      @getForm="getForm"
      @deleteOrg="deleteOrg"
    ></BoardTitle>
    <div
      v-if="formType"
      style="
        border: 1px solid gray;
        padding: 20px;
        margin: 10px;
        border-radius: 5px;
      "
    >
      <form
        v-if="formType == 'admin'"
        class="d-flex flex-column"
        @submit.prevent="
          $emit('patchOrg', newAdmin);
          newAdmin = null;
          getForm('');
        "
      >
        <label for="exampleInputEmail1" class="form-label">User</label>
        <select
          class="form-select"
          aria-label="Default select example"
          v-model="newAdmin"
          required
        >
          <option v-for="user in orgData.Users" :key="user.id" :value="user.id">
            {{ user.username }}
          </option>
        </select>
        <div style="margin-top: 10px">
          <button type="submit" class="btn btn-primary">Change</button>
          <button class="btn btn-secondary" @click="getForm('')">Cancel</button>
        </div>
      </form>
      <form
        v-else
        @submit.prevent="
          $emit('putOrg', newName);
          newName = '';
          getForm('');
        "
      >
        <div class="mb-3">
          <label for="exampleInputEmail1" class="form-label"
            >Organization name</label
          >
          <input v-model="newName" type="text" class="form-control" required />
        </div>
        <div style="margin-top: 10px">
          <button type="submit" class="btn btn-primary">Update</button>
          <button class="btn btn-secondary" @click="getForm('')">Cancel</button>
        </div>
      </form>
    </div>
    <BoardList
      :orgData="orgData"
      @deleteCategory="deleteCategory"
      @putCategory="putCategory"
      @postTask="postTask"
      :deleteTask="deleteTask"
      :putTask="putTask"
      :patchTask="patchTask"
    ></BoardList>
  </div>
</template>

<script>
import BoardTitle from "./BoardTitle";
import BoardList from "./BoardList";

export default {
  data() {
    return {
      formType: "",
      newAdmin: null,
      newName: "",
    };
  },
  name: "KanbanBoardOrg",
  props: [
    "orgData",
    "loadUsers",
    "users",
    "postMember",
    "deleteMember",
    "postCategory",
    "deleteCategory",
    "deleteOrg",
    "putCategory",
    "postTask",
    "deleteTask",
    "putTask",
    "patchTask"
  ],
  methods: {
    getUsers(data) {
      this.loadUsers(data);
    },
    add(data) {
      this.postMember(data);
    },
    getForm(data) {
      this.formType = data;
      this.newAdmin = null;
      this.newName = "";
    },
  },
  components: {
    BoardTitle,
    BoardList,
  },
};
</script>

<style>
</style>