<template>
  <div
    class="d-flex flex-column flex-sm-row justify-content-center align-items-center align-items-sm-stretch flex-wrap"
  >
    <div
      v-for="category in orgData.Categories"
      class="flex-shrink-0"
      id="holder"
      :key="category.id"
      style="
        background-color: #2f4f4f;
        color: white;
        margin: 10px;
        border-radius: 5px;
        width: 300px;
      "
    >
      <div>
        <h5>
          {{ category.name }}
          <svg
            @click="getFormCat(category.id)"
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="currentColor"
            class="bi bi-pencil-fill"
            viewBox="0 0 16 16"
          >
            <path
              d="M12.854.146a.5.5 0 0 0-.707 0L10.5 1.793 14.207 5.5l1.647-1.646a.5.5 0 0 0 0-.708l-3-3zm.646 6.061L9.793 2.5 3.293 9H3.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.207l6.5-6.5zm-7.468 7.468A.5.5 0 0 1 6 13.5V13h-.5a.5.5 0 0 1-.5-.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.5-.5V10h-.5a.499.499 0 0 1-.175-.032l-.179.178a.5.5 0 0 0-.11.168l-2 5a.5.5 0 0 0 .65.65l5-2a.5.5 0 0 0 .168-.11l.178-.178z"
            />
          </svg>
          <svg
            @click="$emit('deleteCategory', category.id)"
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="currentColor"
            class="bi bi-trash-fill"
            viewBox="0 0 16 16"
          >
            <path
              d="M2.5 1a1 1 0 0 0-1 1v1a1 1 0 0 0 1 1H3v9a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2V4h.5a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1H10a1 1 0 0 0-1-1H7a1 1 0 0 0-1 1H2.5zm3 4a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 .5-.5zM8 5a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7A.5.5 0 0 1 8 5zm3 .5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 1 0z"
            />
          </svg>
        </h5>
      </div>
      <form
        v-if="pageType == category.id && editCat == true"
        @submit.prevent="
          $emit('putCategory', category.id, newCatName);
          newCatName = '';
          reset();
        "
        style="padding: 10px"
      >
        <div class="mb-3">
          <label for="exampleInputEmail1" class="form-label"
            >Category name</label
          >
          <input
            v-model="newCatName"
            type="text"
            class="form-control"
            required
          />
        </div>
        <div style="margin-top: 10px">
          <button type="submit" class="btn btn-primary">Update</button>
          <button class="btn btn-secondary" @click="reset">Cancel</button>
        </div>
      </form>
      <ul
        class="list-group"
        style="
          margin: 10px;
          height: 300px;
          overflow-y: scroll;
          color: darkslategray;
        "
      >
        <TaskCard
          v-for="task in category.Tasks"
          :key="task.id"
          :taskData="task"
          :categoryData="orgData.Categories"
          @deleteTask="deleteTask"
          @putTask="putTask"
          @patchTask="patchTask"
        ></TaskCard>
      </ul>
      <div class="text-light container">
        <a class="btn" @click="getFormTask(category.id)"> + Add Task</a>
        <form
          v-if="pageType == category.id && editTask == true"
          @submit.prevent="
            $emit('postTask', category.id, newTaskName);
            newCatName = '';
            reset();
          "
          style="padding: 10px"
        >
          <div class="mb-3">
            <label for="exampleInputEmail1" class="form-label">Title</label>
            <input
              v-model="newTaskName"
              type="text"
              class="form-control"
              required
            />
          </div>
          <div style="margin-top: 10px">
            <button type="submit" class="btn btn-primary">Update</button>
            <button class="btn btn-secondary" @click="reset">Cancel</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import TaskCard from "./TaskCard";

export default {
  data() {
    return {
      pageType: "",
      newCatName: "",
      newTaskName: "",
      editTask: false,
      editCat: false,
    };
  },
  name: "BoardList",
  props: ["orgData", "deleteTask", "putTask", "patchTask"],
  methods: {
    reset() {
      (this.pageType = ""),
        (this.editCat = false),
        (this.editTask = false),
        (this.newTaskName = "");
    },
    getFormCat(str) {
      this.reset();
      this.pageType = str;
      this.editCat = true;
    },
    getFormTask(str) {
      this.reset();
      this.pageType = str;
      this.editTask = true;
    },
  },
  components: {
    TaskCard,
  },
};
</script>

<style>
</style>