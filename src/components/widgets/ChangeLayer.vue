<template>
  <div>
    <p class="text">图层切换：</p>
    <table>
      <tr>
        <td><button @click="change_osm" class="btn">OSM底图</button></td>
        <td><button @click="change_arc_street" class="btn">Arcgis街道图</button></td>
        <td><button @click="change_amap_imagery" class="btn">高德影像图</button></td>
      </tr>
    </table>
  </div>
</template>

<script>
  // eslint-disable-next-line
  import {EventBus} from '../../eventbus.js'
  export default {
    name: "ChangeLayer",
    data(){
      return {
        switch_osm: false,
        switch_arc_street: false,
        switch_amap_imagery: false
      }
    },
    methods: {
      change_osm(){
        this.switch_osm = !this.switch_osm;
        EventBus.$emit('switch-osm', this.switch_osm)
      },
      change_arc_street(){
        this.switch_arc_street= !this.switch_arc_street;
        EventBus.$emit('switch-arc', this.switch_arc_street)
      },
      change_amap_imagery(){
        this.switch_amap_imagery= !this.switch_amap_imagery;
        EventBus.$emit('switch-amap', this.switch_amap_imagery)
      }
    },
  }
</script>

<style scoped>
  .btn{ 
    flex: 0 1 140px;
    /* flex布局设置 */
    background-color: #f6f6f6;
    color: black;
    padding: 8px 10px;
    text-align: center;
    text-decoration: none;
    margin-right: 2px;
    display: inline-block;
    font-size: 14px;
    margin: 4px 2px;
    cursor: pointer;
    border-radius: 8px;
    transition-duration: 0.4s;
  }
  .btn:hover {
    background-color: #4CAF50;
    color: white;
  }
  .btn:active {
    background-color:darkgreen
  }
  .btn:focus {
    outline: 0; /* 去除点击后的边框 */
  }
  .text {
    margin-block-start: 0.4em; 
    margin-block-end: 0.2em;
    margin-inline-start: 0.5em;
  }
</style>
