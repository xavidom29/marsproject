<template>
  <div id="footer">
    <v-footer
      style="background-color: white"
      absolute
      class="font-weight-medium pt-0 pb-0 align-center"
      elevation="4"
    >
      <v-row align="end" justify="center" style="padding-right: 20px">
        <v-col v-for="link in links" :key="link.url" class="d-flex justify-center">
          <v-btn :href="link.url" target="_blank" text>
            <v-icon>{{ link.icon }}</v-icon>
          </v-btn>
        </v-col>
      </v-row>
    </v-footer>
  </div>
</template>

<script>
export default {
  name: "Footer",

  data() {
    return {};
  },
  computed: {
    links() {
      let myLinks = [
        { icon: "mdi-github", url: "https://github.com/constanza101" },
        { icon: "mdi-linkedin", url: "https://linkedin.com/in/constanzadi" },
        { icon: "mdi-card-account-details-star", url: "https://github.com/constanza101/cv_2021/raw/main/Constanza%20Diaz%202021%20CV.pdf" },
        
      ];
      return myLinks;
    },
  },
  methods: {},
  mounted() {},
  async created() {},
};
</script>
<style scoped lang="css">
</style>
