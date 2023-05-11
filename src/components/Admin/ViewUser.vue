<template lang="">
  <v-container h-screen fluid="">
    <v-row no-gutters class="justify-center">
      <v-col lg="12" md="6" sm="12" align="center">
        <v-card-title class="caps">
          List of Registered Users
        </v-card-title>
        <v-card elevation="6" class="pa-5">
          <v-table>
            <thead>
              <tr>
                <th class="text-left">
                  Name
                </th>
                <th class="text-left">
                  Email
                </th>
                <th class="text-left">
                  Role
                </th>
                <th class="text-left">
                  Actions
                </th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in users" :key="item.id">
                <td>{{ item.name }}</td>
                <td>{{ item.email }}</td>
                <td>
                  <v-chip :color="item.isAdmin ? 'red-darken-2' : 'blue'">
                    {{ item.isAdmin ? "Admin" : "User" }}
                  </v-chip>
                </td>
                <td>
                  <v-btn
                    density="compact"
                    icon="mdi-pencil"
                    size="large"
                    color="blue"
                    class="ma-2"
                    @click="EditUser(item.id)"
                  ></v-btn>
                  <v-btn
                    density="compact"
                    icon="mdi-delete"
                    color="red"
                    size="large"
                    class="ma-2"
                    @click="delUser(item.id)"
                  ></v-btn>
                </td>
              </tr>
            </tbody>
          </v-table>
        </v-card>
        <v-card elevation="6" class="pa-5 mt-5" v-if="showEditUser">
          <v-card-title>Edit user </v-card-title>
          <v-form fast-fail @submit.prevent="UpdateUser()">
            <v-text-field
              v-model="editUserDetail.name"
              label="Name"
              :rules="[rules.required]"
              prepend-icon="mdi-account"
              hint="Enter User Name"
              variant="solo"
            ></v-text-field>
            <v-text-field
              v-model="editUserDetail.email"
              label="User Email"
              :rules="[rules.required]"
              prepend-icon="mdi-email"
              hint="Enter User Email"
              variant="solo"
            ></v-text-field>
            <v-text-field
              v-model="editUserDetail.password"
              :rules="[rules.required, rules.min]"
              name="input-10-1"
              prepend-icon="mdi-lock"
              label="Password"
              hint="At least 8 characters"
              variant="solo"
            ></v-text-field>
            <v-radio-group v-model="editUserDetail.isAdmin" inline>
              <v-radio label="Admin" :value="true"></v-radio>
              <v-radio label="User" :value="false"></v-radio>
            </v-radio-group>
            <v-btn type="submit" color="blue">Update</v-btn>
            <v-btn @click="showEditUser = false" color="red" class="ml-3"
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
import { load } from "webfontloader";
export default {
  data() {
    return {
      users: [],
      showEditUser: false,
      editUserDetail: {
        name: "",
        email: "",
        password: "",
        isAdmin: "",
      },
      editUserId: null,
      rules: {
        required: (value) => !!value || "Required.",
        min: (v) => v.length >= 8 || "Min 8 characters",
      },
    };
  },
  methods: {
    async loadData() {
      let res = await axios.get("http://localhost:3000/user");
      this.users = res.data;
    },
    async delUser(id) {
      let result = await axios.delete("http://localhost:3000/user/" + id);
      if (result.status === 200) {
        this.loadData();
      }
    },
    async EditUser(id) {
      this.showEditUser = true;
      let res = await axios.get("http://localhost:3000/user/" + id);
      this.editUserDetail.name = res.data.name;
      this.editUserDetail.email = res.data.email;
      this.editUserDetail.password = res.data.password;
      this.editUserDetail.isAdmin = res.data.isAdmin;
      this.editUserId = id;
    },
    async UpdateUser() {
      await axios.put(
        "http://localhost:3000/user/" + this.editUserId,
        this.editUserDetail
      );
      this.loadData();
      this.showEditUser = false;
    },
  },
  mounted() {
    this.loadData();
  },
};
</script>
<style lang=""></style>
