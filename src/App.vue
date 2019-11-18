<template>
  <div>
    <div class="flex justify-between">
      <div class="ml-2">SAMPLING POINTS</div>
      <div class="m-1">
        <button class="rounded border-2 p-2" @click="save">GUARDAR</button>
      </div>
    </div>
    <l-map ref="myMap" style="height: 900px" :zoom="zoom" :center="center">
      <v-tilelayer-googlemutant :apikey="apikey" :options="options"></v-tilelayer-googlemutant>
      <l-geo-json @click="onMapClick" :geojson="poligon.geojson" :options="poligon.options"></l-geo-json>

      <div v-for="(marker, index) in markers" :key="index">
        <v-editablecirclemarker
          :latLng="getLatiLong(marker.position.latitude, marker.position.longitude)"
          :rad="parseInt(marker.radio)"
          :options="optionsMarker"
        ></v-editablecirclemarker>
        <l-marker :lat-lng="getLatiLong(marker.position.latitude, marker.position.longitude)">
          <l-popup>
            <div class="flex">
              <div class="w-10/12 ml-1 border-r-2 block">
                <div class="flex pr-3">
                  <label class="mr-1" for>Muestras</label>
                  <input
                    v-model="marker.num_samples"
                    type="number"
                    pattern="[0-9]{10}"
                    class="rounded border-2"
                    style="width: 100px"
                  />
                </div>
                <div class="flex mt-1">
                  <label class="mr-1" for>Radio (m)</label>
                  <input
                    v-model="marker.radio"
                    type="number"
                    pattern="[0-9]{10}"
                    class="rounded border-2"
                    style="width: 100px"
                  />
                </div>
              </div>
              <div class="w-2/12 ml-1 mr-1">
                <a class="mt-1 pl-2 cursor-pointer" @click="eliminar(marker)">
                  <svg
                    class="fill-current"
                    viewBox="0 0 20 20"
                    width="20"
                    height="20"
                    version="1.1"
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                  >
                    <path
                      d="M2,2 L18,2 L18,4 L2,4 L2,2 Z M8,0 L12,0 L14,2 L6,2 L8,0 Z M3,6 L17,6 L16,20 L4,20 L3,6 Z M8,8 L9,8 L9,18 L8,18 L8,8 Z M11,8 L12,8 L12,18 L11,18 L11,8 Z"
                      id="Combined-Shape"
                    />
                  </svg>
                </a>
              </div>
            </div>
          </l-popup>
        </l-marker>
      </div>
    </l-map>
    <button @click="save">Guardar</button>
  </div>
</template>

<script>
import L from "leaflet";
import { latLng } from "leaflet";
import { LMap, LGeoJson, LMarker, LPopup } from "vue2-leaflet";
import Vue2LeafletGoogleMutant from "vue2-leaflet-googlemutant/Vue2LeafletGoogleMutant";
import Vue2LeafletEditablecirclemarker from "./components/Vue2LeafletCircleMarker";

import data from "./assets/geojson/data.json";

export default {
  name: "App",
  mounted() {},
  components: {
    LMap,
    LGeoJson,
    LMarker,
    LPopup,
    "v-tilelayer-googlemutant": Vue2LeafletGoogleMutant,
    "v-editablecirclemarker": Vue2LeafletEditablecirclemarker
  },
  data() {
    return {
      markers: [],
      optionsMarker: {
        icon: L.divIcon({
          className: "fake"
        }),
        draggable: false
      },
      poligon: {
        geojson: data.features[0],
        options: {}
      },

      options: {
        type: "hybrid"
      },
      apikey: "AIzaSyDmBqC4lCt-LWj0b4PYJlHPBaTn9YtXgrA",
      zoom: 17,
      center: latLng(-14.010508830892729, -75.8031879258449),
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
    };
  },
  methods: {
    onMapClick(e) {
      let marker = {
        position: {
          latitude: e.latlng.lat,
          longitude: e.latlng.lng
        },
        radio: 10,
        num_samples: 1
      };
      this.markers.push(marker);
    },
    save() {
      console.log(JSON.stringify(this.markers));
    },
    getLatiLong(lat, long) {
      return latLng(lat, long);
    },
    eliminar(marker) {
      let index = this.markers.indexOf(marker);
      console.log(index);
      if (index > -1) {
        this.markers.splice(index, 1);
      }
    }
  }
};
</script>
<style src="tailwindcss/dist/tailwind.min.css">
</style>