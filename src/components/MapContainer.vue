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
  import {EventBus} from '../eventbus.js'
  import View from 'ol/View'
  import Map from 'ol/Map'
  import TileLayer from 'ol/layer/Tile'
  import XYZ from 'ol/source/XYZ'
  import OSM from 'ol/source/OSM'
  import {defaults, FullScreen, ScaleLine, ZoomSlider, MousePosition, OverviewMap} from 'ol/control'
  import {DragRotate, defaults as defaultsInteraction} from 'ol/interaction'
  // import {MapBrowserEvent} from 'ol'  // 不需要导入MapBrowserEvent，也不需要导入setVisible
  import {createStringXY} from 'ol/coordinate' // 导入通过export function导出的方法要加大括号

  // 导入ol的CSS样式
  import 'ol/ol.css'

  export default {
    name: 'MapContainer',
    data() {
      return {
        show_osm: true,
        show_amap: false,
        show_arc: false,
        amapLayer: new TileLayer({
          source: new XYZ({
            url: "https://webst01.is.autonavi.com/appmaptile?style=6&x={x}&y={y}&z={z}"
          }),
          preload: Infinity,
          visible: false  // 默认隐藏该图层
        }),
        arcStreetLayer: new TileLayer({
          source: new XYZ({
            url: "https://map.geoq.cn/ArcGIS/rest/services/ChinaOnlineCommunity/MapServer/tile/{z}/{y}/{x}"
          }),
          preload: Infinity,
          visible: false   // 默认隐藏该图层
        }),
        osmLayer: new TileLayer({
          source: new OSM(),
          visible: true
        }),
      }
    },
    mounted() {
      let ctrlCondition = function(mapBrowserEvent){
        return mapBrowserEvent.originalEvent.ctrlKey;
      };

      EventBus.$on('switch-osm', (show_osm) => {
        this.show_osm = show_osm;
      }),
      EventBus.$on('switch-arc', (show_arc) => {
        this.show_arc = show_arc;
      }),
      EventBus.$on('switch-amap', (show_amap) => {
        this.show_amap = show_amap;
      });

      this.olMap = new Map({
        // 地图将通过'map-root'的ref属性进行绑定
        // 这里想用id属性也可以
        target: this.$refs['map-root'],
        layers: [this.amapLayer, this.arcStreetLayer, this.osmLayer],
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
        ]),
        interactions: defaultsInteraction().extend([
          new DragRotate({
            condition: ctrlCondition
          }), // ctrl+drag实现地图旋转
        ])
      })
    },
    watch: {
      show_osm: function(a,b,c) {
        a = this.amapLayer
        b = this.osmLayer
        c = this.arcStreetLayer
        b.setVisible(true)
        c.setVisible(false)
        a.setVisible(false)
      },
      show_amap: function(a,b,c) {
        a = this.amapLayer
        b = this.osmLayer
        c = this.arcStreetLayer
        a.setVisible(true)
        b.setVisible(false)
        c.setVisible(false)
      } ,
      show_arc: function(a,b,c) {
        a = this.amapLayer
        b = this.osmLayer
        c = this.arcStreetLayer
        c.setVisible(true)
        a.setVisible(false)
        b.setVisible(false)
      }
    },
    // 页面并不用跳转，这里写上只是习惯
    beforeDestroy(){
      EventBus.$off('switch-osm', 'switch-arc','switch-amap')
    }
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