<template>
  <div id="dialog-alert">
     <v-card dark>
        <v-col cols="12" class="d-flex justify-center pt-5">
          <v-btn x-large fab><v-icon x-large>mdi-space-station mdi-spin</v-icon></v-btn>
        </v-col>
        <v-card-title>
         {{title}}
        </v-card-title>

        <v-card-text>
          {{text1}}  <br>
          {{text2}}
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn :disabled="loading" color="primary" class="mb-2" @click="closeDialog">
            try again
          </v-btn>
        </v-card-actions>
      </v-card>
  </div>
</template>

<script>


export default {
  name: "DialogAlert",
  props: {
    title: String,
    text1: String,
    text2: String,
    loading: Boolean
  },
  data() {
    return {
    }
  },
  computed: {
 
  },
  methods: {
   closeDialog() {
    //  console.log(event)
      this.$emit("close-dialog");
    },
  },
  mounted() {
  },
  async created() {},
};
</script>
<style scoped lang="css">

</style>
