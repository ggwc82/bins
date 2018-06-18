<template>
  <div>
    <div>
      <h2>Search for an address and mark your missed bin collection</h2>
      <label>
        <gmap-autocomplete
          @place_changed="setPlace">
        </gmap-autocomplete>
        <button @click="addMarker('blue')">Search</button>
        <div>
          <button @click="addMarker('blue')">Add Blue Bin</button>
          <button @click="addMarker('green')">Add Green Bin</button>
          <button @click="addMarker('brown')">Add Brown Bin</button>
        </div>

      </label>
      <br/>

    </div>
    <br>
    <gmap-map
      :center="center"
      :options="{scrollwheel: false, disableDefaultUI: true, mapStyle} "
      :zoom="16"
      style="width:80%;  height: 600px;"
    >
      <gmap-marker
        :key="index"
        v-for="(m, index) in markers"
        :position="m.position"
        :draggable="true"
        :icon="m.icon"
        @click="center=m.position"
      ></gmap-marker>
    </gmap-map>
  </div>
</template>

<script>
export default {
  name: "GoogleMap",
  data() {
    return {
      // default to Montreal to keep it simple
      // change this to whatever makes sense
      center: { lat: 51.4895, lng: 0.3108 },
      markers: [],
      places: [],
      currentPlace: null,
    };
  },

  mounted() {
    // this.geolocate();
  },

  methods: {
    // receives a place object via the autocomplete component
    setPlace(place) {
      this.currentPlace = place;
    },
    addMarker(colour) {
      console.log('addmarker');
      if (this.currentPlace) {
        const marker = {
          lat: this.currentPlace.geometry.location.lat(),
          lng: this.currentPlace.geometry.location.lng()
        };
        
        this.markers.push({ position: marker, icon: {url: require("../assets/bin_" + colour + ".svg")}});
        this.places.push(this.currentPlace);
        this.center = marker;
        // this.currentPlace = null;
        // console.log(this.places);
        console.log(this.markers);
      }
    },
    geolocate: function() {
      navigator.geolocation.getCurrentPosition(position => {
        this.center = {
          lat: position.coords.latitude,
          lng: position.coords.longitude
        };
      });
    }
  }
};
</script>