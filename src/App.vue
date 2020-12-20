<template>
  <v-app>
    <v-app-bar app color="#F25021" dark dense>
      <v-spacer> </v-spacer>
      <!-- <img
        class="mt-7"
        src="../src/assets/spark.png"
        height="60px"
        width="35px"
      /> -->
      <h3>SPARK©</h3>
      <v-spacer> </v-spacer>
    </v-app-bar>

    <v-main>
      <HelloWorld />
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",

  components: {
    HelloWorld,
  },

  data: () => ({
    //
  }),
};
</script>
<style  >
/* @import url("https://media.made.com/mws-assets/d4f1179df87879f6dfe8afa64011a25a.ttf");
* {
  font-family: "FS Meridian Medium", "Roboto";
} */
</style>