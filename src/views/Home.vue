<template>
  <div class="home">
    <v-card class="mx-auto" max-width="900" v-if="fetched">
      <NewTaskForm v-on:submitted="createTask" :form="form"></NewTaskForm>
      <v-divider></v-divider>
      <v-list subheader two-line flat>
        <v-subheader> {{ user.UserName }}'s Tasks: </v-subheader>

        <TaskList
          v-on:deleted="deleteTask"
          v-on:checked="updateTask"
          :tasks="tasks"
        ></TaskList>
      </v-list>
    </v-card>
  </div>
</template>

<script>
import { getCurrentDate, formatDate } from "@/util";
import AppBar from "@/components/AppBar.vue";
import NewTaskForm from "@/components/NewTaskForm.vue";
import TaskList from "@/components/TaskList.vue";

export default {
  name: "Home",
  components: {
    AppBar,
    NewTaskForm,
    TaskList,
  },
  props: {
    user: Object,
  },
  data: () => ({
    fetched: false, // This keeps us from getting an error when the page loads, but there's no data
    tasks: [], // This will hold the list of tasks you get from your API
    form: {
      Text: "",
      Date: getCurrentDate(),
    },
  }),
  mounted() {
    this.readTasks();
  },
  methods: {
    createTask(form) {
      debugger;
      fetch(`${process.env.VUE_APP_API_ORIGIN}/api/v1/tasks/`, {
        method: "POST",
        credentials: "include",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(form),
      }).then((response) => {
        if (response.ok) {
          this.readTasks();
          this.form.Text = "";
          this.form.Date = "";
        }
      });
    },
    readTasks() {
      fetch(`${process.env.VUE_APP_API_ORIGIN}/api/v1/tasks`, {
        credentials: "include",
      })
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          this.tasks = data;
          this.fetched = true;
        });
    },
    updateTask(task) {
      let checked = false;
      if (task.Done == true) {
        checked = false;
      } else {
        checked = true;
      }

      fetch(`${process.env.VUE_APP_API_ORIGIN}/api/v1/tasks/${task._id}`, {
        method: "PUT",
        credentials: "include",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          Done: checked,
        }),
      }).then((response) => {
        if (response.ok) {
          this.readTasks();
        }
      });
    },

    deleteTask(task) {
      fetch(
        // The first parameter is a string that contains the full URL to your endpoint
        `${process.env.VUE_APP_API_ORIGIN}/api/v1/tasks/${task._id}`,
        // The second parameter is an object with options. You can include request
        // headers here, options for credentials, which method, which mode, etc.
        {
          method: `DELETE`,
          credentials: `include`,
        }
        // Note: The default for method is GET, so you don't need to include the
        // method on any GET requests.
      ).then((response) => {
        // Here we're just checking if the response was successful or not before
        // trying to do anything about it.
        if (response.ok) {
          // If it is successful, we want to update the task list.
          this.readTasks();
        }
      });
    },
  },
};
</script>

<style scoped>
.form {
  padding: 0 1rem;
}
</style>
