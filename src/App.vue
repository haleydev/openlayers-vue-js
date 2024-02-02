<script lang="ts">

import { fromLonLat } from 'ol/proj';
import Map from 'ol/Map';
import OSM from 'ol/source/OSM.js';
import TileLayer from 'ol/layer/Tile.js';
import View from 'ol/View.js';

export default {

  data() {


    return {

      map: new Map,



    }
  },

  mounted() {

    this.map.setTarget('map');

    this.map.setLayers([
      new TileLayer({
        source: new OSM(),
      }),
    ])

    this.map.setView(new View({
      center: fromLonLat([0, 0]),
      zoom: 0
    }));    

    setTimeout(() => {
      var view = this.map.getView();
      view.setCenterInternal(fromLonLat([-44.0316416, -20.0269543]));
      view.setZoom(18);
    }, 1000)



    
  },
}
</script>

<template>
  <div id="map">

  </div>
</template>

<style scoped>
#map {
  height: 100vh;
  width: 100vw;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  left: 0;
}
</style>
