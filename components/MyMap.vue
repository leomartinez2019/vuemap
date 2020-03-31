<template>
  <div class="hello">
    <h1>{{ title }}</h1>
    <MapLoader :mapConfig="mapConfig"/>
  </div>
</template>

<script>
import MapLoader from './MapLoader.vue'

const mapConfig = {
  center: {lat: 11.0, lng: -74.80},
  zoom: 3
}

export default {
  name: 'MyMap',
  components: {
    MapLoader
  },
  props: {
  },
  data () {
    return {
      title: 'Eathquake Map',
      mapConfig: mapConfig
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
