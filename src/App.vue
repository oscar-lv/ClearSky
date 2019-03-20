<template>
<div id="app">
  <div class="w3-sidebar barText">
    <div class="upperBorder">
      <img src="./assets/paperp.png" alt="icon" class="center">
      <h1 style="text-align:center"> Clear Sky</h1>
      <br>
      <vue-google-autocomplete style="width:89%;margin-left:2%" ref="address" id="map" classname="form-control" placeholder="Type your address" v-on:placechanged="getAddressData">
      </vue-google-autocomplete>
      <ul style="text-align:left">
        <li> Locality : {{address.country}} </li>
        <li> Latitude : {{Math.floor(address.latitude)}} </li>
        <li> Longitude : {{Math.floor(address.longitude)}}</li>
        <li> Air Pollution : {{emission}} </li>
        <li> Noise Pollution : {{noise}} </li>
        <li> Number of Planes : {{planes}} </li>
        <li>{{post}}</li>
        <li>Weather : {{weather}} </li>
        <li> Air pollution is scaled accordingly to the BAQI scale, 100 being the cleanest and 0 the poorest. </li>
      </ul>
    </div>
  </div>

  <div style="margin-left:15%">


    <iframe width="100%" height="800" frameborder="0" style="border:0" :src="source+map_code" allowfullscreen>
    </iframe>
  <div id="map" onload="initMap()"></div>
  </div>
  <footer style="text-align:center">
    ©Addil, Summan, Valentin and Oscar (2018)
  </footer>
</div>
</template>

<script>
import VueGoogleAutocomplete from 'vue-google-autocomplete';
import axios from 'axios';
import _ from 'lodash';


export default {
  components: {
    VueGoogleAutocomplete
  },
  name: 'app',
  data: function() {
    return {
      address: '',
      emission: 0,
      noise: 0,
      post: '',
      planes: '',
      source: "https://www.google.com/maps/embed/v1/place?key=AIzaSyCuBzFfC_Mb706zJ8B-FgcsrJco-hle-2s",
      map_code: "&q=.",
      weather: '',
    }
  },
  methods: {
    /**
     * When the location found
     * @param {Object} addressData Data of the found location
     * @param {Object} placeResultData PlaceResult object
     * @param {String} id Input container ID
     */
    getAddressData: function(addressData, placeResultData, id) {
      this.address = addressData;
      this.map_code = '&q=' + this.address.route + '&zoom=10';
      var vm = this;
      var lat = Math.floor(this.address.latitude);
      var lon = Math.floor(this.address.longitude);
      this.emission = Math.floor(Math.random() * 20) + 50  ;
      this.noise = Math.floor(Math.random() * 10) + 60 + ' db' ;
      this.planes = Math.floor(Math.random() * 10) + 5;
      /*
      Axios request which fetches Weather api data and plugs it in
      */
      axios.get('http://api.openweathermap.org/data/2.5/weather?lat=' + lat + '&lon=' + lon + '&units=metric&APPID=de28e63e4d471d4e3f193fa39d400c31')
        .then(function(response) {
          this.weather = (_.startCase(response.data.weather[0].description) + ', ' + (response.data.main.temp) + '°C');
        })
        .catch(function(error) {
          console.log(error);
        });
      /*
      Fetch function which should fetch from Clear Sky API
      */
      axios.post('http://51.254.212.72/api/search/', {
        lat: "12.4",
        lng: "12.3"
       })
       .then(function(res) {
         console.log('Response from the back-end service:');

       })
       .catch(function (err) {
         console.log('Error on the request to the back-end service: ' + err);
       });
    },
},
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  padding: 0;
  margin: 0;

}

.w3-sidebar {
  height: 100%;
  width: 15%;
  background-color: #fff;
  position: fixed !important;
  z-index: 1;
  border-style: solid;
}

h1,
h2 {
  font-weight: normal;
}

.barText {
  color: white;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block !important;
  float: left;
  margin: 8px 7px 8px 0px;
}

a {
  color: #42b983;
}

.upperBorder {
  background-color: #37474F;
  height: 100%;
  padding: 0;
  margin: 0 !important;
}

.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}

#over {
  font-size: 5em;
  position: absolute;
  display:block;
  margin : auto;
  top: 20px;
  left: 20px;
  z-index: 2
}

</style>
