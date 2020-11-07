<template>
  <v-app>
    <v-main>
      <MealGenerator/>
    </v-main>
  </v-app>
</template>

<script>
import MealGenerator from './components/MealGenerator';

export default {
  name: 'App',

  components: {
    MealGenerator,
  },

  data: () => ({
    //
  }),
};
</script>
