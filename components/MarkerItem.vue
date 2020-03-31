<template>
  <div @click="$emit('testing', {lat: dato.lat, lng: dato.lng})" class="marker__item">
    <h3>{{ dato.place }}</h3>
    <p>Magnitude: {{ dato.magnitude }}</p>
    <p><span :style="iconColorObject" class="icon-square"></span> Intensity: {{ iconIntensity }}</p>
    <p>Depth: {{ dato.depth }} km</p>
    <p>{{ dato.time }}</p>
    <button class="btn btn-secondary">Locate in map</button>
  </div>
</template>

<script>

export default {
  name: 'MarkerItem',
  props: ['dato'],
  data () {
    return {

    }
  },
  computed: {
    iconColorObject () {
      if (this.dato.magnitude > 7.0) {
        return {background: 'red'}
      }
      else if (this.dato.magnitude > 6 && this.dato.magnitude < 6.9) {
        return {background: 'orange'}
      }
      else if (this.dato.magnitude > 5 && this.dato.magnitude < 5.9) {
        return {background: 'yellow'}
      }
      else if (this.dato.magnitude > 4 && this.dato.magnitude < 4.9) {
        return {background: '#67dddd'}
      }
      else {
        return {background: '#8e67fd'}
      }
    },
    iconIntensity () {
      if (this.dato.magnitude > 7.0) {
        return "Severe"
      }
      else if (this.dato.magnitude > 6 && this.dato.magnitude < 6.9) {
        return "Very Strong"
      }
      else if (this.dato.magnitude > 5 && this.dato.magnitude < 5.9) {
        return "Strong"
      }
      else if (this.dato.magnitude > 4 && this.dato.magnitude < 4.9) {
        return "Moderate"
      }
      else {
        return "Light"
      }
    }
  },
  methods: {

  }
}

</script>

<style>
.marker__item {
  border: 1px solid grey;
  padding: 4px;
}

.icon-square {
  border: 1px solid grey;
  display: inline-block;
  width: 15px;
  height: 15px;
}
</style>
