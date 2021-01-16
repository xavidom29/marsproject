<template>
  <div id="instructions-commands">
    <v-card style="min-height: calc(100vh - 240px)">
      <v-row class="px-5" >
          <v-col cols="12" lg="5" style="text-align: start;" >
            <b>Instructions</b> <br />
            Plan the route and type the commands in the textfield below. Press the button and watch it happen! <br>
             * Don't worry about the boundaries, if the rover reaches a border it will not fall out of the map!
          </v-col>

          <v-col cols="12" lg="6" style="text-align: start;">
            <b>Commands</b>
            <ul >
              <li><b>F:</b> moves the rover forwards one position</li>
              <li> <b>R:</b> turns the rover right and moves the rover forwards one position</li>
              <li> <b>L:</b> turns the rover left and moves the rover forwards one  position</li>
            </ul>
          </v-col>
          <v-col cols="12" class="pl-7 mt-lg-5">
            <v-text-field
              v-model="roverMovements"
              append-icon="mdi-arrow"
              label="eg: RRFFFL"
              single-line
              hide-details
              outlined
              dense
              :append-outer-icon="'mdi-rocket'"
              @click:append-outer="moveRover"
            ></v-text-field
          ></v-col>

          <v-col class="px-7">
            <v-card class="mt-lg-10">
              <v-col cols="12">
                <b>At the moment the rover is facing {{ direction }}.</b>
                <v-icon class="pl-2">mdi-compass-rose</v-icon>
              </v-col>
              <v-col cols="12">
                <b>Actual position:</b> <br />
                {{roverPosition}}
              </v-col>
            </v-card>
          </v-col>
        </v-row>
        </v-card>
  </div>
</template>

<script>

export default {
  name: "InstructionsAndCommands",
  props: {
    direction: String,
    roverPosition: Object
    
  },
  data() {
    return {
      roverMovements: "",
    }
  },
  computed: {
 
  },
  methods: {
   closeDrawer() {
    //  console.log(event)
      this.$emit("close-drawer");
    },
    // eslint-disable-next-line no-unused-vars
    moveRover(event){
      this.closeDrawer()
      this.$emit("move-rover", this.roverMovements);
      this.roverMovements = ""
      
    }
  },
  mounted() {
  },
  async created() {},
};
</script>
<style scoped lang="css">

</style>
