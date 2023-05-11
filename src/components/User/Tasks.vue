<template lang="">
  <v-container h-screen fluid="">
    <v-row no-gutters class="justify-center">
      <v-col lg="6" md="6" sm="12" align="center">
        <v-card elevation="6" class="pa-5 ma-1">
          <v-card-title class="bg-grey-lighten-2 pa-5 mb-5 rounded">
            Add New Task
          </v-card-title>
          <v-form fast-fail @submit.prevent="AddTask">
            <v-text-field
              v-model="task.task"
              label="Enter Task...."
              prepend-icon="mdi-view-list"
              hint="Enter New Task"
              variant="solo"
            ></v-text-field>
            <v-btn type="submit" class="ma-2 bg-success" rounded="xl"
              >Add Task</v-btn
            >
          </v-form>
        </v-card>
      </v-col>
      <v-col lg="6" md="6" sm="12" align="center">
        <v-card elevation="6" class="pa-5 ma-1">
          <v-card-title class="bg-grey-lighten-2 pa-5 mb-5 rounded">
            Your Tasks
          </v-card-title>
          <v-table>
            <thead>
              <tr>
                <th class="text-center bg-light-blue-lighten-3">
                  Task
                </th>
                <th class="text-center bg-light-green-accent-2">
                  Actions
                </th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in StoredTasks" :key="item.id">
                <td class="text-center">{{ item.task }}</td>
                <td class="text-center">
                  <v-btn
                    icon="mdi-pencil"
                    size="small"
                    color="success"
                    class="ma-1"
                    variant="outline"
                    @click="EditTask(item.id)"
                  ></v-btn>
                  <v-btn
                    icon="mdi-delete"
                    size="small"
                    color="primary"
                    class="ma-1"
                    variant="outline"
                    @click="DeleteTask(item.id)"
                  ></v-btn>
                </td>
              </tr>
            </tbody>
          </v-table>
        </v-card>
      </v-col>
    </v-row>
    <v-row class="justify-center" v-if="showEditTask">
      <v-col lg="6" md="6" sm="12" align="center">
        <v-card elevation="6" class="pa-5 ma-1">
          <v-form fast-fail @submit.prevent="UpdateTask">
            <v-text-field
              v-model="TaskToEdit.task"
              label=""
              prepend-icon="mdi-pencil"
              hint="Enter New Task"
              variant="solo"
            ></v-text-field>
            <v-btn type="submit" class="ma-2 bg-success" rounded="xl"
              >Update Task</v-btn
            >
            <v-btn
              @click="showEditTask = false"
              color="red"
              class="ma-2"
              rounded="xl"
              >Cancel</v-btn
            >
          </v-form>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      task: { task: "", user_id: null },
      LogedInUser: null,
      StoredTasks: [],
      showEditTask: false,
      TaskToEdit: [],
    };
  },
  methods: {
    AddTask: async function () {
      this.task.user_id = this.LogedInUser.id;
      await axios
        .post("http://localhost:3000/task", this.task)
        .then((this.task.task = ""));
      this.ShowTasks();
    },

    ShowTasks: async function () {
      let User = JSON.parse(localStorage.getItem("user"));
      this.LogedInUser = User;
      let Stored = await axios.get(
        "http://localhost:3000/task?user_id=" + this.LogedInUser.id
      );
      this.StoredTasks = Stored.data;
    },
    DeleteTask: async function (id) {
      let result = await axios.delete("http://localhost:3000/task/" + id);
      this.ShowTasks();
    },
    EditTask: async function (id) {
      this.showEditTask = true;
      let result = await axios.get("http://localhost:3000/task/" + id);
      this.TaskToEdit = result.data;
    },
    UpdateTask: async function () {
      await axios.put(
        "http://localhost:3000/task/" + this.TaskToEdit.id,
        this.TaskToEdit
      );
      this.ShowTasks();
      this.showEditTask = false;
    },
  },
  mounted() {
    this.ShowTasks();
  },
};
</script>
<style lang=""></style>
