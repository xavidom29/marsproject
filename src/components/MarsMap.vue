<template>
  <div class="py-0" id="leaflet-map" style="max-height: calc(100vh - 150px)">
    <v-row>
      <v-col cols="12" md="8" lg="7" class="px-5 py-0 d-flex justify-center">
        <span style="font-weight: bold" class="pr-2"
          >Welcome to the red planet! Enjoy a virtual tour!</span
        >
      </v-col>

      <v-col class="hidden-md-and-up d-flex justify-center">
        <v-btn
          color="primary"
          class="hidden-md-and-up"
          @click="drawer = !drawer"
        >
          Let's do it!
        </v-btn>
      </v-col>

      <v-navigation-drawer
        class="hidden-md-and-up"
        right
        style="width: 500px"
        v-model="drawer"
        absolute
        bottom
        temporary
      >
        <v-row>
          <v-col>
            <InstructionsAndCommands
              :roverPosition="rover.position"
              :direction="direction"
              @move-rover="moveRover"
              @close-drawer="closeDrawer()"
            />
          </v-col>
        </v-row>
      </v-navigation-drawer>
    </v-row>

    <v-row class="px-xs-12 px-md-0">
      <v-col cols="12" md="8" lg="7" class="px-0 d-flex justify-center">
        <l-map
          ref="map"
          :crs="crs"
          style="height: 550px; width: 98%; background-color: black"
          :center="center"
          :options="{
            zoomControl: false,
            scrollWheelZoom: false,
            touchZoom: false,
            tap: false,
          }"
        >
          <l-image-overlay :url="url" :bounds="bounds"></l-image-overlay>
          <l-rectangle
            :bounds="rectangle.bounds"
            :l-style="rectangle.style"
          ></l-rectangle>

          <l-marker
            :draggable="true"
            :key="roverKey"
            :lat-lng="rover.position"
            @update:lat-lng="updatePosition"
          >
            <l-icon>
              <div class="background-icon">
                <v-icon large
                  >{{ rover.icon }} {{ `mdi-rotate-${rover.facing}` }}</v-icon
                >
              </div>
            </l-icon>

            <l-popup>
              <v-row>
                <v-col
                  class="d-flex justify-center pt-1"
                  cols="12"
                  style="font-size: 14px"
                >
                  {{ rover.position }}
                </v-col>
              </v-row>
            </l-popup>
          </l-marker>
        </l-map>
      </v-col>

      <v-col cols="12" md="4" lg="5" class="hidden-sm-and-down">
        <InstructionsAndCommands
          :roverPosition="rover.position"
          :direction="direction"
          @move-rover="moveRover"
          @close-drawer="closeDrawer()"
        />
      </v-col>
    </v-row>

    <v-dialog v-model="dialog" max-width="490">
      <DialogAlert
        v-if="dialogBorder"
        title="Oops! we have discovered that Mars has a very unusual flat and squared shape!!"
        text1="Fortunately our rover has a boundary detector which prevents it to fall out of the planet!"
        text2="Your planned route has been cancelled, please plan a new route."
        :loading="loading"
        @close-dialog="closeDialogBorder()"
      ></DialogAlert>

      <DialogAlert
        v-if="dialogObstacle"
        title="Oops! Yoy have bumped into a CRATER!!"
        text1="Our rober can't climb, so it has stopped here."
        text2="Your planned route has been cancelled, please plan a new route."
        :loading="loading"
        @close-dialog="closeDialogObstacle()"
      ></DialogAlert>
    </v-dialog>
  </div>
</template>

<script>
import InstructionsAndCommands from "./InstructionsAndCommands.vue";
import DialogAlert from "./DialogAlert.vue";
import { CRS } from "leaflet";
import { latLng } from "leaflet";
import {
  LMap,
  LImageOverlay,
  LMarker,
  LPopup,
  LIcon,
  LRectangle,
} from "vue2-leaflet";

export default {
  name: "Example",
  components: {
    InstructionsAndCommands,
    DialogAlert,
    LMap,
    LRectangle,
    LMarker,
    LImageOverlay,
    LPopup,
    LIcon,
  },
  data() {
    return {
      stop: false,
      drawer: false,
      dialog: false,
      dialogBorder: false,
      dialogObstacle: false,
      popupMessage: "",
      degrees: 0,
      roverKey: 0,
      rover: {
        icon: "mdi-space-station",
        position: latLng(41.26713, 1.974814, 17),
        // position: LatLng(34.811548, -26.768394),
        facing: 0,
      },
      latitudude: 41.26713,
      longitude: 1.974814,
      rectangle: {
        bounds: [
          [140, 180],
          [240, 70],
        ],
        style: { color: "black", weight: 3 },
      },
      loading: false,
      zoom: -10,
      center: [141.26713, 111.974814, 17],
      url: "https://photojournal.jpl.nasa.gov/jpeg/PIA00411.jpg",
      bounds: [
        [-100, -100],
        [400, 400],
      ],
      crs: CRS.Simple,
    };
  },
  computed: {
    direction() {
      let direction = "";
      if (this.rover.facing === 0) {
        direction = "north";
      } else if (this.rover.facing === 90) {
        direction = "east";
      } else if (this.rover.facing === 180) {
        direction = "south";
      } else if (this.rover.facing === 90) {
        direction = "west";
      }
      return direction;
    },
  },
  methods: {
    closeDrawer() {
      this.drawer = false;
    },
    closeDialogBorder() {
      this.dialog = false;
      this.dialogBorder = false;
    },
    closeDialogObstacle() {
      this.dialog = false;
      this.dialogObstacle = false;
    },
    foundBorder() {
      this.dialogBorder = true;
      this.dialog = true;
      this.stopMoving();
    },
    foundObstacle() {
      this.dialogObstacle = true;
      this.dialog = true;
      this.stopMoving();
    },
    moveFwd() {
      let degrees = this.rover.facing;
      degrees === 0
        ? (this.rover.position.lat += 1)
        : degrees === 90
        ? (this.rover.position.lng += 1)
        : degrees === 180
        ? (this.rover.position.lat -= 1)
        : degrees === 270
        ? (this.rover.position.lng -= 1)
        : degrees;

      this.roverKey++;
    },
    stopMoving() {
      this.stop = true;
      let degrees = this.rover.facing;
      degrees === 0
        ? (this.rover.position.lat -= 1)
        : degrees === 90
        ? (this.rover.position.lng -= 1)
        : degrees === 180
        ? (this.rover.position.lat += 1)
        : degrees === 270
        ? (this.rover.position.lng += 1)
        : degrees;
    },
    updatePosition(position) {
      this.rover.position = position;
    },
    async moveRover(item) {
      this.stop = false;
      this.loading = true;
      for (const char of item) {
        const actionChars = ["f", "l", "r"];
        if (actionChars.includes(char) && !this.dialog) {
          await this.action(char.toLowerCase());
        }
      }
      this.loading = false;
    },
    async action(char) {
      try {
        await this.waitPromise(2000).then(
          this.turnRover(char).then(this.moveForward())
        );
      } catch (e) {
        console.log(e);
      }
    },
    async turnRover(char) {
      char === "r"
        ? (this.rover.facing += 90)
        : char === "l"
        ? (this.rover.facing -= 90)
        : char;
      //we make sure that degrees are always 0 to 270
      this.rover.facing ** 2 === 129600
        ? (this.rover.facing = 0)
        : this.rover.facing == -90
        ? (this.rover.facing = 270)
        : this.rover.facing;
    },
    async moveForward() {
      for (let x = 0; x < 100; x++) {
        setTimeout(() => {
          if (this.stop === false) {
            const northBound = this.rover.position.lat > 366;
            const eastBound = this.rover.position.lng > 370;
            const southBound = this.rover.position.lat < -86;
            const westBound = this.rover.position.lng < -82;
            const borders = northBound || eastBound || southBound || westBound;
            const obstacle =
              this.rover.position.lat < 256 &&
              this.rover.position.lat > 104 &&
              this.rover.position.lng > 36 &&
              this.rover.position.lng < 197;

            borders
              ? this.foundBorder()
              : obstacle
              ? this.foundObstacle()
              : this.moveFwd();
          }
        }, 100);
      }
    },

    async waitPromise(ms) {
      return new Promise((resolve) => setTimeout(resolve, ms));
    },
  },
  mounted() {
    this.$refs.map.mapObject.setView([150, 150], -1);
  },
  async created() {},
};
</script>
<style scoped lang="css">
.background-icon {
  margin-top: -27px;
  margin-left: -10px;
  width: 46px;
  height: 46px;
  background-color: white;
  border-radius: 50%;
  padding: 5px;
  box-shadow: 1px 1px 5px grey;
}
a {
  color: black;
}
>>> .leaflet-popup {
  margin-bottom: 40px;
}
>>> .leaflet-top {
  z-index: 400;
}
>>> .leaflet-bottom {
  z-index: 400;
}

>>> .v-input--hide-details {
  padding-top: 0;
}

>>> .leaflet-map-pane {
  z-index: 0;
}
</style>
