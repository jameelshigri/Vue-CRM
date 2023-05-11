<template>
  <v-container h-screen fluid="">
    <v-row no-gutters class="justify-center pa-15">
      <v-col lg="6" md="6" sm="12" align="center">
        <v-card elevation="6" class="pa-5">
          <v-card-title class="bg-grey-lighten-2 pa-5 mb-5 rounded">
            Login
          </v-card-title>
          <v-form fast-fail @submit.prevent="singnin()">
            <v-text-field
              v-model="user.email"
              label="Email"
              :rules="[rules.required]"
              prepend-icon="mdi-account"
              hint="Enter valid email"
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

            <v-btn type="submit" class="ma-2 bg-info" rounded="xl">Login</v-btn>
          </v-form>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import axios from "axios";
export default {
  name: "Login",
  data: () => ({
    user: {
      email: "",
      password: "",
    },
    show: false,
    rules: {
      required: (value) => !!value || "Required.",
      min: (v) => v.length >= 8 || "Min 8 characters",
    },
  }),
  methods: {
    async singnin() {
      let res = await axios.get(
        `http://localhost:3000/user?email=${this.user.email}&password=${this.user.password}`
      );
      if (res.status == 200 && res.data.length > 0) {
        localStorage.setItem("user", JSON.stringify(res.data[0]));
        this.$router.push({ name: "home" });
      } else {
        alert("Invalid Credentials!");
        this.$router.push({ name: "login" });
      }
    },
  },
};
</script>
