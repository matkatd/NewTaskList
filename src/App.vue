<template>
  <v-app>
    <!-- Link to Material Icons-->
    <link
      href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700|Material+Icons"
      rel="stylesheet"
      type="text/css"
    />

    <AppBar :user="user"></AppBar>

    <v-main>
      <router-view :user="user"></router-view>
    </v-main>
  </v-app>
</template>

<script>
import AppBar from "@/components/AppBar.vue";
import { cookies } from "./util";
export default {
  name: "App",
  components: {
    AppBar,
  },
  asyncComputed: {
    user: {
      async get() {
        let response;
        // Use fetch() to get the user from your API
        // Make sure to include your credentials in the request
        // Also you might want to still return an object identical
        // to the `default` below if the fetch() fails
        response = await fetch(
          `${process.env.VUE_APP_API_ORIGIN}/api/v1/user`,
          {
            headers: {
              Cookie: cookies,
            },
            credentials: "include",
          }
        ).then((response) => {
          if (response.ok) {
            return response.json();
          } else {
            response.UserName = "";
          }
        });
        return response;
      },
      default: {
        UserName: "",
      },
    },
  },
};
</script>
