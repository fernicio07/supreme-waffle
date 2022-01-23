<template>
  <v-app>
    <v-main>
      <AudioTable />
    </v-main>
  </v-app>
</template>

<script>
import AudioTable from "@/components/Audio/AudioTable";

export default {
  name: "App",

  components: {
    AudioTable,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap");

html,
body {
  font-family: "Cairo", sans-serif;
}

#app {
  font-family: "Cairo", sans-serif;
}
</style>
