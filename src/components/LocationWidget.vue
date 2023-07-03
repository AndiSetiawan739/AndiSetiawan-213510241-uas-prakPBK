<template>
  <div class="container">
    <h1 class="title">Location Finder</h1>
    <div class="map-container">
      <GmapMap
        :center="{ lat: latitude, lng: longitude }"
        :zoom="zoom"
        style="width: 100%; height: 400px"
      >
        <GmapMarker
          :position="{ lat: latitude, lng: longitude }"
          :clickable="false"
        ></GmapMarker>
      </GmapMap>
    </div>
    <div class="search-container">
      <div class="form-group">
        
        <div class="input-field">
          <input
            type="text"
            class="form-control"
            id="inputLatitude"
            v-model="inputLatitude"
            placeholder="Enter latitude"
          />
        </div>
      </div>
      <div class="form-group">
        
        <div class="input-field">
          <input
            type="text"
            class="form-control"
            id="inputLongitude"
            v-model="inputLongitude"
            placeholder="Enter longitude"
          />
        </div>
      </div>
      <button class="btn btn-primary" @click="fetchLocationDetails">
        Find Location
      </button>
    </div>
    <div class="result-container" v-if="foundLocation">
      <div class="card">
        <div class="card-body">
          <h5 class="card-title">Location Details</h5>
          <p>
            <strong>Address:</strong> {{ foundLocation.formatted }}
          </p>
          <p>
            <strong>Latitude:</strong> {{ foundLocation.geometry.lat }}
          </p>
          <p>
            <strong>Longitude:</strong> {{ foundLocation.geometry.lng }}
          </p>
          <p>
            <strong>Postal Code:</strong> {{ foundLocation.components.postcode }}
          </p>
        </div>
      </div>
    </div>
    <div class="alert alert-danger" role="alert" v-if="error">
      {{ error }}
    </div>
  </div>
</template>

<script>
import { GmapMap, GmapMarker } from 'vue2-google-maps';

export default {
  components: {
    GmapMap,
    GmapMarker,
  },
  data() {
    return {
      latitude: null,
      longitude: null,
      inputLatitude: '',
      inputLongitude: '',
      foundLocation: null,
      error: '',
      zoom: 12,
    };
  },
  mounted() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(this.getPosition);
    }
  },
  methods: {
    getPosition(position) {
      this.latitude = position.coords.latitude;
      this.longitude = position.coords.longitude;
    },
    async fetchLocationDetails() {
      try {
        const apiKey = '92591005a7b94008909d59a64b6d2a49';
        const latitude = this.inputLatitude || this.latitude;
        const longitude = this.inputLongitude || this.longitude;
        const apiUrl = `https://api.opencagedata.com/geocode/v1/json?q=${encodeURIComponent(
          latitude + ',' + longitude
        )}&key=${apiKey}`;

        const response = await fetch(apiUrl);
        const data = await response.json();

        if (data.results && data.results.length > 0) {
          const location = data.results[0];
          this.foundLocation = location;
          console.log('Location:', location);
          
        } else {
          this.error = 'Location not found.';
        }
      } catch (error) {
        console.error('Error fetching location data:', error);
        this.error = 'Error fetching location data.';
      }
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid white; /* Menambahkan border 2px warna putih */
  background-color:  rgb(27, 27, 27);
}

.title {
  text-align: center;
  margin-bottom: 30px;
  color: white;
}

.map-container {
  margin-bottom: 20px;
}

.search-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.form-group {
  display: flex;
  flex-direction: row;
  align-items: center;
  width: 100%;
  margin-bottom: 15px;
}

.input-label {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  width: 30%;
}

.input-field {
  width: 70%;
}

.btn-primary {
  width: 100%;
  background-color: #007bff;
  color: white;
  font-size: 16px;
  padding: 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.result-container {
  margin-top: 20px;
}

.card {
  background-color: #eaf6ff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.card-body {
  padding: 20px;
}

.card-title {
  font-size: 20px;
  font-weight: bold;
}

.card p {
  margin-bottom: 10px;
}

.card p strong {
  font-weight: bold;
}

.alert-danger {
  margin-top: 20px;
}

.form-control {
  border: 2px solid #ccc;
  border-radius: 4px;
  padding: 10px;
}
</style>
