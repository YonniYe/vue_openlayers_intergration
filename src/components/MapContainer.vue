<template>
  <div ref="map-root"
       style="height: 100%; width: 100%">
    <div ref="mouse-position"
         class="mouse-position-wrapper">
      <div class="custom-mouse-position"></div>
    </div>
  </div>
</template>

<script>
  import View from 'ol/View'
  import Map from 'ol/Map'
  import TileLayer from 'ol/layer/Tile'
  import OSM from 'ol/source/OSM'
  import {defaults, FullScreen, ScaleLine, ZoomSlider, MousePosition, OverviewMap} from 'ol/control'
  import {createStringXY} from 'ol/coordinate' // 导入通过export function导出的方法要加大括号

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
          new OverviewMap({
            layers: [
              new TileLayer({
                source: new OSM(),
              })]
          }),
          new MousePosition({
            coordinateFormat: createStringXY(4), // 坐标格式
            projection: 'EPSG:4326',  // 投影坐标系（若未设置则输出为默认投影坐标系下的坐标）
            className: 'custom-mouse-position',  // CSS类名
            target: this.$refs['mouse-position']  // 元素名
          })
        ])
      })
    },
  }
</script>

<style scoped>
  /* 对包装器wrapper进行定位，把MousePosition放进包装器里 */
  .mouse-position-wrapper{
    width: 200px;
    color: black;
    position: relative; /* 我使用了相对位置 */
    height: 0px;
    left: 86%;
    top: 94%; 
    z-index: 999;  /* 在地图容器中的层，要设置z-index的值让其显示在地图上层 */
  }
</style>