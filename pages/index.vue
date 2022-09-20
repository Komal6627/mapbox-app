<template>
  <!-- <input type="text" id="search" placeholder="Enter The Pin " /> -->
  <label for="" id="choose">Choose the satellite Mode:-</label>
  <select id="layer-change">
    <option selected value="mapbox://styles/mapbox/streets-v11">Street</option>
    <option value="mapbox://styles/mapbox/outdoors-v11">outdoors</option>
    <option value="mapbox://styles/mapbox/light-v10">light</option>
    <option value="mapbox://styles/mapbox/dark-v10">dark</option>
    <option value="mapbox://styles/mapbox/satellite-v9">satellite</option>
    <option value="mapbox://styles/mapbox/satellite-streets-v11">
      satellite-streets
    </option>
    <option value="mapbox://styles/mapbox/navigation-day-v1">
      navigation-day
    </option>
    <option value="mapbox://styles/mapbox/navigation-night-v1">
      navigation-night
    </option>
  </select>
  <main class="w-screen h-screen">
    <v-map class="w-full h-full" :options="state.map" @loaded="mapMark" />
  </main>
</template>

<script setup lang="ts">
import VMap from "v-mapbox";
import { reactive } from "vue";
import mapboxgl, { Marker } from "mapbox-gl";

import MapboxGeocoder from "@mapbox/mapbox-gl-geocoder";

// export default {
//   name: "App",
//   components: {
//     VMap,
//   },
mapboxgl.accessToken =
  "pk.eyJ1Ijoia29tYWxraWxsZWRhciIsImEiOiJjbDZnY3ZhY2ExdTJ4M2lxbGtid294ODljIn0._5sPomlX5lhlubBQTXVAww";
const state = reactive({
  map: {
    container: "map",
    // accessToken:
    //   "pk.eyJ1Ijoia29tYWxraWxsZWRhciIsImEiOiJjbDZnY3ZhY2ExdTJ4M2lxbGtid294ODljIn0._5sPomlX5lhlubBQTXVAww",
    style: "mapbox://styles/mapbox/streets-v11?optimize=true",
    // style: "https://basemaps.cartocdn.com/gl/dark-matter-gl-style/style.json",
    center: [73.8567, 18.5204],
    zoom: 11,
    maxZoom: 12,
    crossSourceCollisions: false,
    failIfMajorPerformanceCaveat: false,
    attributionControl: false,
    preserveDrawingBuffer: true,
    hash: false,
    minPitch: 0,
    maxPitch: 60,
  },
});

function mapMark(map) {
  map.on("click", (e) => {
    console.log("I am clicked");

    new mapboxgl.Marker({
      draggable: true,
      color: getRandomColor(),
    })
      .setLngLat([e.lngLat.lng, e.lngLat.lat])
      .addTo(map);
  });

  const setStyle: any = document.getElementById("layer-change");
  setStyle.addEventListener("change", (event) => {
    console.log(event);
    map.setStyle(event.target.value);
  });

  map.flyTo({
    center: [73.8567, 18.5204],
    zoom: 9,
    speed: 0.4,
    curve: 1,
    easing(t) {
      return t;
    },
  });

  map.addControl(
    new MapboxGeocoder({
      accessToken: mapboxgl.accessToken,
      mapboxgl: mapboxgl,
    })
  );
}

// map.on("load", () => {
//   console.log("loading");
// });

function getRandomColor() {
  var letters = "0123456789ABCDEF";
  var color = "#";
  for (var i = 0; i < 6; i++) {
    color += letters[Math.floor(Math.random() * 16)];
  }
  return color;
}
</script>

<style>
/* @import "~mapbox-gl/dist/mapbox-gl.css";
@import "~v-mapbox/dist/v-mapbox.css"; */
html,
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.w-screen {
  width: 100vw;
}
.h-screen {
  height: 100vh;
}
.h-full {
  height: 100%;
}
.w-full {
  width: 100%;
}

.marker {
  background-image: url("assets/images/marker.png");
  background-size: cover;
  width: 50px;
  height: 50px;
  border-radius: 50%;
}

#layer-change {
  padding: 1px;
  position: fixed;
  top: 25px;
  left: 30px;
  z-index: 1;
  margin: 1px;
}

#choose {
  position: fixed;
  top: 5px;
  left: 18px;
  z-index: 1;
  color: black;
}

#search {
  position: fixed;
  top: 35px;
  left: 20px;
  z-index: 1;
}
</style>
