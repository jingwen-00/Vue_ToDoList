<template>
  <div class="home">
    <v-text-field
      v-model="newTasktitle"
      @click:append="addTask"
      @keyup.enter="addTask"
      class="pa-6"
      outlined
      label=" Add Task"
      append-icon="mdi-plus"
      hide-details
      clearable
    ></v-text-field>
    <v-list v-if="tasks.length" flat class="pt-0">
      <div v-for="task in tasks" :key="task.id">
        <v-list-item @click="doneTask(task.id)" :class="{ 'blue lighten-5': task.done }">
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox :input-value="task.done"></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title
                :class="{ 'text-decoration-line-through': task.done }"
              >{{ task.title }}</v-list-item-title>
            </v-list-item-content>
            <v-list-item-action>
              <v-btn icon @click="snackbar = true" @click.stop="deleteTask(task.id)">
                <v-icon color="primary lighten-1">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider></v-divider>
      </div>
      <v-snackbar v-model="snackbar" color="red lighten">
        {{ snackbartext }}
        <template v-slot:action="{ attrs }">
          <v-btn color="black" text v-bind="attrs" @click="snackbar = false">Close</v-btn>
        </template>
      </v-snackbar>
    </v-list>
    <div v-else class="no-tasks">
      <v-icon color="primary" size="100">mdi-check</v-icon>
      <div class="text-h5 primary--text">No Tasks</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      snackbar: false,
      snackbartext: "Task have been deleted!",
      newTasktitle: "",
      tasks: [
        // {
        //   id: 1,
        //   title: "Wake Up",
        //   done: false,
        // },
        // {
        //   id: 2,
        //   title: "Reading Book",
        //   done: false,
        // },
        // {
        //   id: 3,
        //   title: "Revision Chapter 54",
        //   done: false,
        // },
      ]
    };
  },
  methods: {
    addTask() {
      //check input value
      //console.log('addTask');
      let newTask = {
        id: Date.now(),
        title: this.newTasktitle,
        done: false
      };
      this.tasks.push(newTask);
      this.newTasktitle = "";
    },
    doneTask(id) {
      //check id
      //console.log("id: ", id);
      let task = this.tasks.filter(task => task.id === id)[0];
      task.done = !task.done;
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id);
    }
  }
};
</script>
<style lang="sass">
.no-tasks
  position: absolute
  left: 50%
  top: 50%
  transform: translate(-50%,-50%)
  opacity: 0.5
</style>
