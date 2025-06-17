<template>
  <div>
    <l-map style="height: 450px" :zoom="zoom" :center="currentPosition">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <l-marker :lat-lng="currentPosition">
        <l-popup>
          <div>Votre position actuelle</div>
        </l-popup>
      </l-marker>
    </l-map>
  </div>
</template>

<script>
import * as L from "leaflet";

import { LMap, LTileLayer, LMarker, LPopup } from "vue2-leaflet";

export default {
  name: "app",
  components: { LMap, LTileLayer, LMarker, LPopup },
  data: function () {
    return {
      zoom: 17,
      currentPosition: [21.14631, 79.08491],
      url: "http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
    };
  },
  mounted() {
    this.askPermissionForCurrentPosition();
  },
  methods: {
    askPermissionForCurrentPosition() {
      const vm = this;
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition((position) => {
          vm.currentPosition = L.latLng(
            position.coords.latitude,
            position.coords.longitude
          );
        });
      } else {
        alert("Geolocation is not supported by this browser.");
      }
      return vm.currentPosition;
    },
  },
};
</script>
