<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-toolbar-title class="headline mr-4">Dreamcatcher</v-toolbar-title>
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/henkelmax/dreamcatcher"
        target="_blank"
        text
      >
        <span class="mr-2">GitHub</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <Canvas/>
    </v-main>
  </v-app>
</template>

<script>
import Canvas from './components/Canvas';

export default {
  name: 'App',
  components: {
    Canvas,
  },
};
</script>
