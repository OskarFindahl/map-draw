<template>
  <div>
    <button id="delete">Delete Selected</button>
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

    let drawings=[];

    google.maps.event.addListener(drawingManager, 'overlaycomplete', function(e) {
      drawings.push(e);
      let newShape = e.overlay;
      google.maps.event.addListener(newShape, 'click', function() {
        setSelection(newShape);
      });  
    });
    google.maps.event.addDomListener(document.getElementById('delete'), 'click', deleteSelectedShape);
    let selectedShape;
    function deleteSelectedShape() {
      if (selectedShape) {
        selectedShape.setMap(null);
      }
    }
    function setSelection(shape) {
      clearSelection();
      selectedShape = shape;
    }
    function clearSelection() {
      if (selectedShape) {
        selectedShape.setEditable(false);
        selectedShape = null;
      }
    }
  }
}
</script>

<style scoped>
#map{
  height: 80vh;
}
#delete {
  width: 172px;
  height: 58px;
  margin: 6px;
  color: red;
  border-radius: 5px;
  background-color: rgb(143 241 241);
}
    

</style>