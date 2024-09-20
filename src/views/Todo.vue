<template>
  <div class="home">
    <v-text-field
      v-model="newTaskTitle"
      @click:append="addTask"
      @keyup.enter="addTask"
      class="pa-3"
      outlined
      label="Add Task"
      append-icon="mdi-plus"
      hide-details
      clearable
    ></v-text-field>

    <v-list class="pt-0" flat>
      <div v-for="task in tasks" :key="task.id">
        <v-list-item :class="{'purple lighten-3': task.done}">
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox
                :input-value="task.done"
                @click.stop="doneTask(task.id)"
                color="purple darken-1"
              ></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <!-- Task editing: toggles between text and input field -->
              <v-list-item-title
                v-if="!task.isEditing"
                :class="{'text-decoration-line-through': task.done}"
              >
                {{ task.title }}
              </v-list-item-title>

              <v-text-field
                v-if="task.isEditing"
                v-model="task.title"
                @keyup.enter="saveTask(task)"
                class="pa-0"
                hide-details
              ></v-text-field>
            </v-list-item-content>

            <!-- Use d-flex and align-center to align buttons horizontally -->
            <v-list-item-action class="d-flex align-center">
              <v-btn icon @click.stop="editTask(task)">
                <v-icon color="purple darken-1">mdi-pencil</v-icon>
              </v-btn>
              <v-btn icon @click.stop="deleteTask(task.id)" class="ml-2">
                <v-icon color="purple darken-1">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider></v-divider>
      </div>
    </v-list>
  </div>
</template>

<script>
export default {
  name: 'Home',

  data() {
    return {
      newTaskTitle: '',
      tasks: []
    }
  },

  methods: {
    addTask() {
      if (this.newTaskTitle.trim() === '') return; // Prevent adding empty tasks
      let newTask = {
        id: Date.now(),
        title: this.newTaskTitle,
        done: false,
        isEditing: false  // Track if the task is being edited
      }
      this.tasks.push(newTask)
      this.newTaskTitle = '';  // Clear the input after adding the task
    },
    
    doneTask(id) {
      let task = this.tasks.find(task => task.id == id)
      task.done = !task.done
    },

    editTask(task) {
      task.isEditing = true;  // Set the task to editing mode
    },

    saveTask(task) {
      task.isEditing = false;  // Exit editing mode after saving
    },

    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id)
    }
  }
}
</script>
