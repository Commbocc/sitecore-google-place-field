<template lang="html">
  <div class="embed-responsive embed-responsive-16by9">
    <div id="map" ref="map" class="embed-responsive-item"></div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      map: null
    }
  },
  methods: {
    initMap () {
      var center = new google.maps.LatLng(27.9947144,-82.594708)

      this.map = new google.maps.Map(this.$refs.map, {
        center,
        zoom: 7
      })
    }
  },
  mounted () {
    this.initMap()
  }
}
</script>

<style lang="css">
</style>
