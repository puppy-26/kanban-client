<template>
  <div style="font-family: 'Poppins'; cursor: pointer; overflow-x: hidden; height: 100vh;">
    <header>
      <Navbar 
        :orgs="orgs"
        :page="page" 
        :postOrg="postOrg"
        :changePage="changePage"
        :loadOrg="loadOrg"
        :getTask="getTask"
        @logOut="logOut"></Navbar>
    </header>

    <AuthPage
      v-if="page == 'login' || page == 'register'"
      :page="page"
      :postRegister="postRegister"
      :postLogin="postLogin"
    ></AuthPage>
    <KanbanBoard 
      v-if="page == 'home'"
      :tasks="tasks"
      ></KanbanBoard>
      <KanbanBoardOrg
        v-if="page == 'org'"
        :loadUsers="loadUsers"
        :postCategory="postCategory"
        :orgData="orgId"
        :postMember="postMember"
        :users="users"
        :deleteMember="deleteMember"
        :deleteCategory="deleteCategory"
        @putOrg="putOrg"
        @patchOrg="patchOrg"
        :deleteOrg="deleteOrg"
        :putCategory="putCategory"
        :postTask="postTask"
        :deleteTask="deleteTask"
        :putTask="putTask"
        :patchTask="patchTask"
        ></KanbanBoardOrg>
  </div>
</template>

<script>
import Navbar from "./src/components/Navbar";
import AuthPage from "./src/components/AuthPage";
import KanbanBoard from "./src/components/KanbanBoard"
import KanbanBoardOrg from "./src/components/KanbanBoardOrg"
const url = 'https://kanban-server-puppy-26.herokuapp.com'

export default {
  name: "App",
  data() {
    return {
      page: "",
      tasks: [],
      orgs: [],
      orgId: [],
      users: []
    };
  },
  methods: {
    checkAuth() {
      if (localStorage.getItem("access_token")) {
        this.page = "home";
        this.getTask();
        this.getOrg();
        this.getTask();
      } else {
        this.page = "register";
      }
    },
    changePage(str) {
      this.page = str;
    },
    postRegister(data) {
      axios({
        method: 'post',
        url: `${url}/register`,
        data: data
      })
        .then(() => {
          this.page = "login"
        })
        .catch((err) => {
          swal(err.response.data.name, err.response.data.msg.join(','), "error");
        });
    },
    postLogin(data) {
      axios({
        method: 'post',
        url: `${url}/login`,
        data: data
      })
        .then((response) => {
          localStorage.setItem("access_token", response.data.access_token);
          this.checkAuth();
        })
        .catch((err) => {
          swal(err.response.data.msg.join(','), '', "error");
        });
    },
    logOut() {
      localStorage.clear();
      this.checkAuth();
      this.signOut();
    },
    signOut() {
      var auth2 = gapi.auth2.getAuthInstance();
      auth2.signOut().then(function () {
        console.log('User signed out.');
      });
    },
    getTask() {
      axios({
        method: 'get',
        url: `${url}/tasks`,
        headers: {
          access_token: localStorage.access_token
        }
      })
        .then((response) => {
          this.tasks = response.data;
        })
        .catch((err) => {
          console.log(err.response);
        });
    },
    getOrg() {
      axios({
        method: 'get',
        url: `${url}/organizations`,
        headers: {
          access_token: localStorage.access_token
        }
      })
        .then((response) => {
          this.orgs = response.data;
        })
        .catch((err) => {
          throw err;
        });
    },
    postOrg(data) {
      axios({
        method: 'post',
        url: `${url}/organizations`,
        headers: {
          access_token: localStorage.access_token
        },
        data: {
          name: data
        }
      })
        .then(() => {
          swal("Good job!", "Create organization success", "success");
          this.checkAuth();
        })
        .catch((err) => {
          throw err;
        });
    },
    loadOrg(data) {
      axios({
        method: 'get',
        url: `${url}/organizations/${data.id}`,
        headers: {
          access_token: localStorage.access_token
        }
      })
        .then((response) => {
          this.orgId = response.data;
          this.changePage('org');
        })
        .catch((err) => {
          throw err;
        });
    },
    loadUsers (data) {
      axios({
          method: 'get',
          url: `${url}/organizations/${data}/users`,
          headers: {
            access_token: localStorage.access_token
          }
        })
          .then((response) => {
            this.users = response.data;
          })
          .catch((err) => {
            throw err;
          });
    },
    postMember(data) {
      axios({
        method: 'post',
        url: `${url}/organizations/${this.orgId.id}/members`,
        headers: {
          access_token: localStorage.access_token
        },
        data: {
          userId: data
        }
      })
        .then((response) => {
          let id = this.orgId.id;
          swal("Good job!", "Add member success!", "success");
          this.loadUsers(id);
          this.loadOrg(this.orgId);
        })
        .catch((err) => {
          if (typeof err.response.data.msg == 'string') {
            swal(err.response.data.msg, '', "error");
          } else {
            swal(err.response.data.msg.join(','), '', "error");
          }
        })
    },
    deleteMember(data) {
      axios({
        method: 'delete',
        url: `${url}/organizations/${this.orgId.id}/members/${data}`,
        headers: {
          access_token: localStorage.access_token
        }
      })
        .then(() => {
          swal("Good job!", "Remove member success!", "success");
          this.loadUsers(this.orgId.id);
          this.loadOrg(this.orgId);
        })
        .catch((err) => {
          if (typeof err.response.data.msg == 'string') {
            swal(err.response.data.msg, '', "error");
          } else {
            swal(err.response.data.msg.join(','), '', "error");
          }
        });
    },
    postCategory(data) {
      axios({
        method: 'post',
        url: `${url}/categories/${this.orgId.id}`,
        headers: {
          access_token: localStorage.access_token
        },
        data: {
          name: data,
          orgId: this.orgId.id
        }
      })
        .then(() => {
          this.loadOrg(this.orgId);
        })
        .catch((err) => {
          throw err
        });
    },
    deleteCategory (data) {
      swal({
        title: "Are you sure?",
        text: "Once deleted, you will not be able to recover this imaginary file!",
        icon: "warning",
        buttons: true,
        dangerMode: true,
      })
      .then((willDelete) => {
        if (willDelete) {
          axios({
            method: 'delete',
            url: `${url}/categories/${this.orgId.id}/${data}`,
            headers: {
              access_token: localStorage.access_token
            }
          })
            .then(() => {
              this.loadOrg(this.orgId);
              swal("Category has been deleted!", {
                icon: "success",
              });
            })
            .catch((err) => {
              throw err
            });
        }
      });
    },
    putOrg(data) {
      axios({
        method: 'put',
        url: `${url}/organizations/${this.orgId.id}`,
        headers: {
          access_token: localStorage.access_token
        },
        data: {
          name: data
        }
      })
        .then(() => {
          swal("Good job!", "Update organization name success!", "success");
          this.loadUsers(this.orgId.id);
          this.loadOrg(this.orgId);
        })
        .catch(() => {
          if (typeof err.response.data.msg == 'string') {
            swal(err.response.data.msg, '', "error");
          } else {
            swal(err.response.data.msg.join(','), '', "error");
          }
        });
    },
    patchOrg(data) {
      axios({
        method: 'patch',
        url: `${url}/organizations/${this.orgId.id}`,
        headers: {
          access_token: localStorage.access_token
        },
        data: {
          userId: data
        }
      })
        .then(() => {
          swal("Good job!", "Admin has been change!", "success");
          this.loadUsers(this.orgId.id);
          this.loadOrg(this.orgId);
        })
        .catch((err) => {
          if (typeof err.response.data.msg == 'string') {
            swal(err.response.data.msg, '', "error");
          } else {
            swal(err.response.data.msg.join(','), '', "error");
          }
        });
    },
    deleteOrg() {
      axios({
        method: 'delete',
        url: `${url}/organizations/${this.orgId.id}`,
        headers: {
          access_token: localStorage.access_token
        }
      })
        .then(() => {
          swal("Good job!", "Organization deleted!", "success");
          this.checkAuth();
        })
        .catch((err) => {
          if (typeof err.response.data.msg == 'string') {
            swal(err.response.data.msg, '', "error");
          } else {
            swal(err.response.data.msg.join(','), '', "error");
          }
        });
    },
    putCategory (id, data) {
      axios({
        method: 'put',
        url: `${url}/categories/${this.orgId.id}/${id}`,
        headers: {
          access_token: localStorage.access_token
        },
        data: {
          name: data
        }
      })
        .then(() => {
          swal("Good job!", "Category updated!", "success");
          this.loadUsers(this.orgId.id);
          this.loadOrg(this.orgId);
        })
        .catch((err) => {
          throw err;
        });
    },
    postTask(catId, data) {
      axios({
        method: 'post',
        url: `${url}/tasks/${this.orgId.id}`,
        headers: {
          access_token: localStorage.access_token
        },
        data: {
          title: data,
          categoryId: catId
        }
      })
        .then(() => {
          swal("Good job!", "Task created", "success");
          this.loadUsers(this.orgId.id);
          this.loadOrg(this.orgId);
        })
        .catch((err) => {
          throw err;
        });
    },
    deleteTask(id) {
      axios({
        method: 'delete',
        url: `${url}/tasks/${this.orgId.id}/${id}`,
        headers: {
          access_token: localStorage.access_token
        }
      })
        .then(() => {
          swal("Good job!", "Task deleted", "success");
          this.loadUsers(this.orgId.id);
          this.loadOrg(this.orgId);
        })
        .catch((err) => {
          throw err;
        });
    },
    putTask(taskId, data) {
      axios({
        method: 'put',
        url: `${url}/tasks/${this.orgId.id}/${taskId}`,
        headers: {
          access_token: localStorage.access_token
        },
        data: {
          title: data
        }
      })
        .then(() => {
          swal("Good job!", "Task updated", "success");
          this.loadUsers(this.orgId.id);
          this.loadOrg(this.orgId);
        })
        .catch((err) => {
          throw err;
        });
    },
    patchTask(catId, taskId) {
      axios({
        method: 'patch',
        url: `${url}/tasks/${this.orgId.id}/${taskId}`,
        headers: {
          access_token: localStorage.access_token
        },
        data: {
          categoryId: catId
        }
      })
        .then(() => {
          swal("Good job!", "Task updated", "success");
          this.loadUsers(this.orgId.id);
          this.loadOrg(this.orgId);
        })
        .catch((err) => {
          throw err;
        });
    }
  },
  mounted() {
    this.checkAuth();
  },
  components: {
    Navbar,
    AuthPage,
    KanbanBoard,
    KanbanBoardOrg
  },
};
</script>

<style>
</style>