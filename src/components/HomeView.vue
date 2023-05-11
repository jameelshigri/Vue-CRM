<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      class="bg-deep-purple"
      theme="dark"
      permanent
    >
      <v-list color="transparent" v-if="isAdmin">
        <v-list-item
          v-for="item in adminItems"
          :prepend-icon="item.icon"
          :title="item.title"
          @click="loadComponent(item.component)"
        >
        </v-list-item>
      </v-list>

      <v-list color="transparent" v-else>
        <v-list-item
          v-for="item in userItems"
          :prepend-icon="item.icon"
          :title="item.title"
          @click="loadComponent(item.component)"
        >
        </v-list-item>
      </v-list>
      <div class="ma-5">
        <v-btn
          block
          variant="flat"
          elevation="4"
          color="blue"
          rounded="xl"
          prepend-icon="mdi-lock"
          absolute
          @click="logout()"
        >
          Logout
        </v-btn>
      </div>
    </v-navigation-drawer>
    <v-app-bar>
      <v-app-bar-nav-icon @click="drawer = !drawer"> </v-app-bar-nav-icon>
      <v-toolbar-title>Simple CRM</v-toolbar-title>
      <v-avatar size="30" color="red">
        <img src="https://i.pravatar.cc/60" alt="alt" />
      </v-avatar>
      <p class="pa-5">{{ user_name }}</p>
    </v-app-bar>

    <v-main @click="drawer = false">
      <v-container v-if="isAdmin">
        <h1 class="text-center">Welcome Admin</h1>
        <component :is="currentComponent"></component>
      </v-container>
      <v-container v-else>
        <h1 class="text-center">Welcome User</h1>
        <component :is="currentComponent"></component>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import { defineComponent } from "vue";
import CreateUser from "../components/Admin/CreateUser.vue";
import ViewUser from "../components/Admin/ViewUser.vue";
import Tasks from "../components/User/Tasks.vue";
import UpdateTask from "../components/User/UpdateTask.vue";
import Test from "./Test.vue";
// Components

export default defineComponent({
  name: "HomeView",
  data: () => ({
    drawer: false,
    isAdmin: false,
    user_name: "",
    currentComponent: Test,
    adminItems: [
      {
        title: "Add New User",
        icon: "mdi-view-dashboard",
        component: "CreateUser",
      },
      {
        title: "View Users",
        icon: "mdi-account",
        component: "ViewUser",
      },
    ],
    userItems: [
      { title: "My Tasks", icon: "mdi-view-list", component: "Tasks" },
      {
        title: "Profile",
        icon: "mdi-account",
        component: "UpdateTask",
      },
    ],
  }),
  components: {},
  methods: {
    logout() {
      localStorage.clear();
      this.$router.push({ name: "login" });
    },

    loadComponent(component) {
      if (component === "CreateUser") {
        this.currentComponent = CreateUser;
      } else if (component === "ViewUser") {
        this.currentComponent = ViewUser;
      } else if (component === "Tasks") {
        this.currentComponent = Tasks;
      } else if (component === "UpdateTask") {
        this.currentComponent = UpdateTask;
      }
    },
  },
  mounted() {
    const userData = JSON.parse(localStorage.getItem("user"));
    if (userData) {
      this.isAdmin = userData.isAdmin;
      console.log(this.isAdmin);
      console.log(userData);
      this.user_name = userData.name;
    } else {
      this.$router.push({ name: "login" });
    }
  },
});
</script>
