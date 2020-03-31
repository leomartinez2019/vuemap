<template>
  <div class="map-container">
    <div class="google-map" ref="googleMap" id="map"></div>
    <template v-if="this.map">
      <slot
        :map="map"
      />
    </template>
    <div class="markers-aside__info">
      <marker-item @testing="testData" v-for="dato in markerData" :key="dato.id" :dato="dato"/>
    </div>
  </div>
</template>

<script>
import MarkerItem from './MarkerItem.vue'

export default {
  props: {
    mapConfig: Object,
  },
  components: {
    'marker-item': MarkerItem
  },
  data() {
    return {
      map: null,
      features: [],
      markerData: [],
      iconUrl: "http://maps.google.com/mapfiles/ms/icons/"
    }
  },

  async mounted() {
    this.initializeMap()
    this.fetchData()
  },

  methods: {
    testData (payload) {
      this.centerMapOnMarker(payload)
      //console.log(payload)
    },
    initializeMap() {
      const mapContainer = this.$refs.googleMap
      this.map = new window.google.maps.Map(
        mapContainer, this.mapConfig
      )
    },
    centerMapOnMarker (obj) {
      this.map.setZoom(5)
      this.map.panTo(obj)
      //console.log("current zoom: ", this.map.getZoom())
    },
    getIcon (objeto) {
      let magnitude = objeto.magnitude
      if (magnitude > 7.0) {
        return this.iconUrl + "red-dot.png"
      }
      else if (magnitude > 6 && magnitude < 6.9) {
        return this.iconUrl + "orange-dot.png"
      }
      else if (magnitude > 5 && magnitude < 5.9) {
        return this.iconUrl + "yellow-dot.png"
      }
      else if (magnitude > 4 && magnitude < 4.9) {
        return this.iconUrl + "ltblue-dot.png"
      }
      else {
        return this.iconUrl + "purple-dot.png"
      }
    },
    putMarkers () {
      this.markerData.map((obj) => {
        let iconUrl = this.getIcon(obj)
        return new window.google.maps.Marker({
          position: {lat: obj.lat, lng: obj.lng},
          icon: {url: iconUrl},
          title: obj.place  + ". Magnitude: " + obj.magnitude + ". Depth: " + obj.depth + " km",
          map: this.map
        })
      })
    },
    fetchData() {
      fetch("https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/2.5_day.geojson")
        .then(response => response.json())
        .then(data => {
          //console.log(data.features)
          this.features = data.features
          //return data.features
        })
        .then(() => this.organizeData())
        .then(() => this.putMarkers())
    },
    organizeData () {
      //let organizedData = []
      this.markerData = this.features.map(dato => {
        let coords = dato['geometry']['coordinates']
        let magnitude = dato['properties']['mag']
        let place = dato['properties']['place']
        let time = dato['properties']['time']
        let id = dato.id

        return {
          id: id,
          lng: coords[0],
          lat: coords[1],
          depth: coords[2],
          magnitude: magnitude,
          place: place,
          time: new Date(time)
        }
      })
      //this.markerData = organizedData
    }
  }
}
</script>

<style>

#map {
  height: 500px;
  width: 500px;
}

.map-container {
  display: flex;
  justify-content: space-around;
}

.markers-aside__info {
  height: 80vh;
  overflow: scroll;
}
</style>
