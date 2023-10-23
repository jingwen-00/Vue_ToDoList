<template>
  <div class="home">
    <v-text-field
      v-model="newTasktitle"
      @click:append="addTask"
      @keyup.enter="addTask"
      class="pa-6"
      outlined
      label="Add New Task"
      :append-icon="newTasktitle ? 'mdi-plus' : ''"
      hide-details="auto"
      clearable
    ></v-text-field>

    <v-list v-if="tasks.length" flat class="pt-0">
      <h2 class="px-6">Upcoming Task</h2>
      <div
        v-for="(task, i) in tasks.filter((x) => x.done === false)"
        :key="task.id"
      >
        <v-list-item
          @click="doneTask(task.id)"
          :class="{ 'green lighten-5': task.done }"
          class="px-6"
        >
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox :input-value="task.done" color="green"></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title
                :class="{ 'text-decoration-line-through': task.done }"
                >{{ task.title }}</v-list-item-title
              >
            </v-list-item-content>
            <v-list-item-action>
              <v-btn
                icon
                @click="snackbar = true"
                @click.stop="deleteTask(task.id)"
              >
                <v-icon color="red lighten-1">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider
          v-if="i !== tasks.filter((x) => x.done === false).length - 1"
        ></v-divider>
      </div>

      <v-divider class="my-4" color="grey"></v-divider>

      <h2 class="px-6 pb-4">Completed Task</h2>
      <div
        v-for="(task, i) in tasks.filter((x) => x.done === true)"
        :key="task.id"
      >
        <v-list-item
          @click="doneTask(task.id)"
          :class="{ 'green lighten-5': task.done }"
          class="px-6"
        >
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox :input-value="task.done" color="green"></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title
                :class="{ 'text-decoration-line-through': task.done }"
                >{{ task.title }}</v-list-item-title
              >
            </v-list-item-content>
            <v-list-item-action>
              <v-btn
                icon
                @click="snackbar = true"
                @click.stop="deleteTask(task.id)"
              >
                <v-icon color="red lighten-1">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider
          v-if="i !== tasks.filter((x) => x.done === true).length - 1"
        ></v-divider>
      </div>
    </v-list>
    <div v-else class="no-tasks">
      <v-icon color="primary" size="100" class="pb-2">mdi-help</v-icon>
      <div class="text-h5 primary--text">No Tasks</div>
    </div>
  </div>
</template>

<script>
import Swal from "sweetalert2";

export default {
  name: "Home",
  
  data() {
    return {
      snackbar: false,
      snackbartext: "Task have been deleted!",
      newTasktitle: "",
      tasks: [],
    };
  },

  methods: {
    addTask() {
      //check input value
      if (this.newTasktitle) {
        let newTask = {
          id: Date.now(),
          title: this.newTasktitle,
          done: false,
        };
        this.tasks.push(newTask);
        this.newTasktitle = "";
      }
    },

    doneTask(id) {
      //check id
      //console.log("id: ", id);
      let task = this.tasks.filter((task) => task.id === id)[0];
      task.done = !task.done;
    },

    deleteTask(id) {
      Swal.fire({
        title: "Are you sure ?",
        text: "Once deleted, it cannot be recover.",
        type: "warning",
        icon: "warning",
        showCancelButton: true,
        cancelButtonColor: "#aab8b4",
        cancelButtonText: "Cancel",
        confirmButtonColor: "#d33",
        confirmButtonText: "Yes, Delete",
        // reverseButtons: true,
        returnFocus: true,
        customClass: {
          actions: "my-actions",
          confirmButton: "left-gap",
        },
      }).then((result) => {
        if (result.isConfirmed) {
          this.tasks = this.tasks.filter((task) => task.id !== id);
        } else {
        }
      });
    },
  },

  metaInfo() {
    return {
      title: "Vue_To Do List",
      meta: [
        {
          name: "description",
          content:
            "Allows users to manage their tasks and create to-do lists",
        },
        {
          name: "keywords",
          content: "",
        },
        { property: "og:title", content: "Vue_To Do List" },
        { property: "og:site_name", content: "Vue_To Do List" },
        {
          property: "og:description",
          content:
            "Allows users to manage their tasks and create to-do lists",
        },
        {
          property: "og:type",
          content: "website",
        },
      ],
    };
  },
};
</script>

<style>
.no-tasks {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  opacity: 0.5;
}

.swal2-popup {
  font-family: "Roboto", sans-serif !important;
}
</style>
