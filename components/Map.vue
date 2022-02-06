<template>
  <div>
    <div id="map"></div>
  </div>
</template>

<script>
export default { 
  head() {
    return {
      script: [
        {src: `https://maps.googleapis.com/maps/api/js?key=${process.env.GOOGLE_MAPS_API_KEY}&callback&libraries=drawing`}
      ]
    };
  },
  data() {
    return {};
  },
  methods: {},

  mounted() {
    const map = new google.maps.Map(document.getElementById("map"), {
      center: { lat: 59.334591, lng: 18.063240 },
      zoom: 8,
    });
    const drawingManager = new google.maps.drawing.DrawingManager({
      drawingMode: google.maps.drawing.OverlayType.POLYGON,
      drawingControl: true,
      drawingControlOptions: {
        position: google.maps.ControlPosition.TOP_CENTER,
        drawingModes: [
          google.maps.drawing.OverlayType.POLYGON,
        ],
      },
      polygonOptions: {
        editable: true,
        draggable: true,
        clickable: true,
      },
    });
    drawingManager.setMap(map);
  }, 
};
</script>

<style scoped>
#map{
    height: 100vh;
}
</style>