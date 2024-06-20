<template>
  <v-container>
    <v-row class="top-row" align="center">
      <v-col cols="auto" class="logo-container">
        <v-img class="logo" src="../assets/image.png" alt="logo" />
      </v-col>
      <v-col>
        <h1 class="text-h5">Kanban Board</h1>
      </v-col>
      <v-col cols="auto">
        <v-btn color="#5f4b66" @click="dialog = true">Add Task</v-btn>
      </v-col>
    </v-row>
    <v-row>
      <v-col v-for="status in statuslists" :key="status" cols="3">
        <h3>{{ status }}</h3>
        <draggable class="list-group" :list="tasks[status]" group="tasks" @change="log" itemKey="id">
          <template #item="{ element }">
            <v-card class="mb-2 semi-rounded-card">
              <v-card-title>{{ element.title }}</v-card-title>
              <v-card-subtitle>{{ element.description }}</v-card-subtitle>
            </v-card>
          </template>
        </draggable>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="3" v-for="(taskList, status) in tasks" :key="status">
        <rawDisplayer :value="taskList" :title="status" />
      </v-col>
    </v-row>

    <!-- Modal for adding task. could use some tweaking -->
    <v-dialog v-model="dialog" max-width="500px">
      <v-card class="dialog-card">
        <v-card-title>
          <span class="text-h5">Add Task to backlog</span>
        </v-card-title>
        <v-card-text>
          <v-form ref="form">
            <v-text-field v-model="newTask.title" label="Title" required></v-text-field>
            <v-textarea v-model="newTask.description" label="Description" required></v-textarea>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn @click="addTask">Add</v-btn>
          <v-btn @click="dialog = false">Cancel</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
import draggable from "vuedraggable";

export default {
  name: "KanbanBoard",
  components: {
    draggable,
  },
  data() {
    return {
      dialog: false,
      newTask: {
        title: "",
        description: "",
      },
      statuslists: ["Backlog", "In Progress", "In Review", "Done"],
      tasks: {
        "Backlog": [
          {
        id: 5,
        title: "Refactor codebase",
        description: "Improve code structure and readability",
          },
          {
        id: 6,
        title: "Implement authentication",
        description: "Add user authentication functionality",
          },
          {
        id: 7,
        title: "Optimize database queries",
        description: "Improve performance by optimizing database queries",
          },
        ],
        "In Progress": [
          {
        id: 8,
        title: "Write unit tests",
        description: "Create unit tests for critical components",
          },
          {
        id: 9,
        title: "Integrate third-party API",
        description: "Integrate external API for additional functionality",
          },
        ],
        "In Review": [
          {
        id: 10,
        title: "Fix bugs",
        description: "Address and resolve reported bugs",
          },
          {
        id: 11,
        title: "Improve UI/UX",
        description: "Enhance user interface and user experience",
          },
        ],
        "Done": [
          {
        id: 12,
        title: "Deploy to production",
        description: "Release the application to production environment",
          },
          {
        id: 13,
        title: "Document API endpoints",
        description: "Create documentation for API endpoints",
          },
        ],
      },
    };
  },
  methods: {
    addTask() {
      if (this.newTask.title && this.newTask.description) {
        const newTask = {
          id: this.statuslists.length + 1, title: this.newTask.title,
          description: this.newTask.description,
        };
        this.tasks.Backlog.push(newTask);
        this.newTask.title = "";
        this.newTask.description = "";
        this.dialog = false;
      }
    },
  },
};
</script>

<style scoped>
.top-row {
  display: flex;
  align-items: center;
}

.logo-container {
  display: flex;
  align-items: center;
}

.logo {
  width: 50px;
  height: 50px;
  margin-right: 10px;
}

.list-group {
  min-height: 500px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 12px;
  background-color: #ffe5e0;
}

.semi-rounded-card {
  border-radius: 12px;
  background-color: #fff;
}

.v-card-title,
.v-card-subtitle {
  color: #333;
}

h3 {
  color: #5f4b66;
  padding-left: 3px;
}

body {
  background-color: #ffe5e0;
}

.v-container {
  background-color: #fff7e6;
  padding: 20px;
  border-radius: 12px;
}

.text-h5 {
  color: #5f4b66;
  font-weight: 700;
}

.dialog-card {
  background: #fff;
}

.dialog-card .v-card-title {
  color: #5f4b66;
  margin-left: 10px;
}

.dialog-card button {
  background-color: #5f4b66;
  color: #fff;
}

.v-form {
  background-color: #fff7e6;
}
</style>
