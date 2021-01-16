<template >
  <v-container style="height: calc(100vh - 115px); overflow: auto;" >

    <v-row  v-if="home" class="text-center mt-lg-12" style="margin-top: 10vh;">

      <v-col cols="12" class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3" style="color:white;">Welcome to Mars</h1>

        <p class="subheading font-weight-regular" style="color:white;">
          Are you ready to discover life in the red planet?
        </p>
      </v-col>

      <v-col cols="12">
        <v-btn @click="home = false" dark color="orange">take me there!</v-btn>
      </v-col>
       <v-col cols="12">
        <v-btn @click="Weather = true" dark color="orange">take me there!3</v-btn>
      </v-col>
    </v-row>

    <v-row class="text-center" v-if="!home">
      <v-col>
        <MarsMap />
      </v-col>
    </v-row>

    <v-row class="text-center" v-if="Weather">
      <v-col>
        <Weather />
      </v-col>
    </v-row>
    

  </v-container>
</template>

<script>
import MarsMap from "./MarsMap.vue";
import Weather from './Weather.vue';


export default {
  name: "Home",
  components: {
    MarsMap,
    Weather,
  },
  data: () => ({
    home: true,
    Weather: false,
  }),
};
</script>
