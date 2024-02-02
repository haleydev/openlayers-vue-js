<script lang="ts">

import { fromLonLat } from 'ol/proj';
import { Vector, Layer } from 'ol/layer';
import SourceVector from 'ol/source/Vector';
import Feature from 'ol/Feature';
import * as Style from 'ol/style';
import { Circle } from 'ol/geom';
import Map from 'ol/Map';
import OSM from 'ol/source/OSM.js';
import TileLayer from 'ol/layer/Tile.js';
import View from 'ol/View.js';

export default {

  data() {
    return {
      map: new Map
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

    var centro = fromLonLat([-44.0316416, -20.0269543]);

    setTimeout(() => {
      let view = this.map.getView();
      view.setCenterInternal(centro);
      view.setZoom(18);

      // add circle
      let circle = new Circle(centro, 100);

      let layer_circle = new Vector({
        source: new SourceVector({
          features: [new Feature(circle)]
        }),

        style: new Style.Style({
          stroke: new Style.Stroke({
            color: 'red',
            width: 2
          }),

          fill: new Style.Fill({
            color: 'rgba(255, 0, 0, 0.1)'
          })
        })
      })

      this.map.addLayer(layer_circle);

      // remove circle
      setTimeout(() => {
        layer_circle.setSource(null);
        layer_circle.setStyle(null);
      }, 3000)

      // not found
      // this.map.removeLayer(layes_circle)
      //  this.map.getLayers().forEach(vl => {      
      //     // if (layer.get('name') && layer.get('name') == 'flag_vectorLayer') {
      //       this.map.removeLayer(vl)
      //     // }
      //   });
    }, 1000)





    // https://openlayers.org/en/latest/examples/device-orientation.html


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
