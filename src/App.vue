<script lang="ts">

import { fromLonLat } from 'ol/proj';
import { Vector } from 'ol/layer';
import SourceVector from 'ol/source/Vector';
import Feature from 'ol/Feature';
import * as Style from 'ol/style';
import { Circle, Geometry } from 'ol/geom';
import Map from 'ol/Map';
import OSM from 'ol/source/OSM.js';
import TileLayer from 'ol/layer/Tile.js';
import View from 'ol/View.js';

export default {

  data() {
    const circle_vector = new Vector();
    const center = fromLonLat([-44.0316416, -20.0269543]);
    const radius: number = 0;

    return {
      map: new Map,
      radius: radius,
      center: center,
      circle_vector: circle_vector
    }
  },

  mounted() {
    // remove controlls
    this.map.getControls()?.forEach((control) => {
      control.setMap(null);
    })

    this.map.setTarget('map');

    this.map.setLayers([
      new TileLayer({
        source: new OSM(),
      }),
    ])

    this.map.setView(new View({
      center: this.center,
      zoom: 0
    }));

    setTimeout(() => {
      let view = this.map.getView();
      view.setCenterInternal(this.center);
      view.setZoom(18);

      // add circle
      let circle = new Circle(this.center, 100);
      let circle_feature = new Feature(circle);

      circle_feature.setId('circle_feature');

      this.circle_vector.setSource(new SourceVector({
        features: [circle_feature]
      }));

      this.circle_vector.setStyle(new Style.Style({
        stroke: new Style.Stroke({
          color: 'red',
          width: 2
        }),

        fill: new Style.Fill({
          color: 'rgba(255, 0, 0, 0.1)'
        })
      }))

      this.circle_vector.setMap(this.map as Map);
    }, 1000)

    // https://openlayers.org/en/latest/examples/device-orientation.html
  },

  methods: {
    raio: function () {
      let circle_feature = this.circle_vector.getSource()?.getFeatureById('circle_feature') as Feature<Geometry>;
      circle_feature.setGeometry(new Circle(this.center, this.radius));

      // remove
      // this.circle_vector.setMap(null);
    }
  }
}
</script>

<template>
  <div id="map">
    <input placeholder="Raio" id="raio" type="number" v-model="radius" v-on:input="raio">
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

#raio {
  position: fixed;
  z-index: 100;
  top: 5px;
  width: 40px;
  right: 5px;
  border: 1px solid rgb(137, 137, 137);
  padding: 5px;
  border-radius: 5px;
}
</style>
