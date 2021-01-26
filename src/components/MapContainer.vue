<template>
  <div ref="map-root"
       style="height: 100%; width: 100%">
  </div>
</template>

<script>
  import View from 'ol/View'
  import Map from 'ol/Map'
  import TileLayer from 'ol/layer/Tile'
  import OSM from 'ol/source/OSM'
  import {defaults, FullScreen, ScaleLine, ZoomSlider, MousePosition} from 'ol/control'
  // 导入ol的CSS样式
  import 'ol/ol.css'

  export default {
    name: 'MapContainer',
    components: {},
    props: {},
    mounted() {
      // 在mounted中写函数
      new Map({
        // 地图将通过'map-root'的ref属性进行绑定
        // 这里想用id属性也可以
        target: this.$refs['map-root'],
        layers: [
          new TileLayer({
            source: new OSM()
          }),
        ],
        view: new View({
          zoom: 0,
          center: [0, 0],
          constrainResolution: true  //确保OSM tile缩放到正确的级别
        }),
        // 注意地图控件的写法
        controls: defaults().extend([
          new FullScreen(),
          new ZoomSlider(),
          new ScaleLine(),
          new MousePosition()
        ])
      })
    },
  }
</script>