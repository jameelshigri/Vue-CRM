<template lang="">
  <v-container h-screen fluid="">
    <v-row no-gutters class="justify-center">
      <v-col lg="12" md="6" sm="12" align="center">
        <v-card elevation="6" class="pa-5">
          <v-card-title class="bg-grey-lighten-2 pa-5 mb-5 rounded">
            Add New User
          </v-card-title>
          <v-form fast-fail @submit.prevent="AddUser">
            <v-text-field
              v-model="user.name"
              label="Name"
              :rules="[rules.required]"
              prepend-icon="mdi-account"
              hint="Enter User Name"
              variant="solo"
            ></v-text-field>
            <v-text-field
              v-model="user.email"
              label="User Email"
              :rules="[rules.required]"
              prepend-icon="mdi-email"
              hint="Enter User Email"
              variant="solo"
            ></v-text-field>
            <v-text-field
              v-model="user.password"
              :rules="[rules.required, rules.min]"
              :append-inner-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
              :type="show ? 'text' : 'password'"
              name="input-10-1"
              prepend-icon="mdi-lock"
              label="Password"
              hint="At least 8 characters"
              variant="solo"
              @click:append-inner="show = !show"
            ></v-text-field>
            <v-radio-group v-model="user.isAdmin" inline>
              <v-radio label="Admin" :value="true"></v-radio>
              <v-radio label="User" :value="false"></v-radio>
            </v-radio-group>

            <v-btn type="submit" class="ma-2 bg-success" rounded="xl"
              >Add User</v-btn
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
      user: {
        name: "",
        email: "",
        password: "",
        isAdmin: "",
      },
      show: true,
      rules: {
        required: (value) => !!value || "Required.",
        min: (v) => v.length >= 8 || "Min 8 characters",
      },
    };
  },
  methods: {
    async AddUser() {
      let res = await axios.post("http://localhost:3000/user", this.user);
      if (res.status === 201) {
        this.user.name = "";
        this.user.email = "";
        this.user.password = "";
        this.user.isAdmin = "";
      }
    },
  },
};
</script>
<style lang=""></style>
