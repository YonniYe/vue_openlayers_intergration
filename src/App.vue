<template>
  <div id="app">
    <div class="cell cell-map">
      <!-- 使用 -->
      <MapContainer></MapContainer>
    </div>
    <div class="cell cell-panel">
      <Panel></Panel>
    </div>
    <div class="cell cell-inspect">
      Inspect
    </div>
  </div>
</template>

<script>
  import MapContainer from './components/MapContainer'  // 导入
  import Panel from './components/Panel'
  export default {
    name: 'App',
    components: {
      MapContainer, // 注册
      Panel
    }
  }
</script>

<style>
  html, body {
    height: 100%;
    margin: 0;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    height: 100%;
    display: grid;   /* 声明容器采用网格布局 */
    grid-template-columns: 70vw;   /* 定义每一列的列宽，70px表示占视窗宽度的70% */
    grid-auto-rows: 1fr;
    gap: 1rem;  /* 也就是grid-gap，表示行与列之间的间隔 */
    padding: 1rem;
    box-sizing: border-box;  /* 并排的两个带边框的框 */
  }

  .cell {
    border-radius: 4px;
    background-color: lightgrey;
  }

  .cell-map {
    grid-column: 1;
    grid-row: 1 / 3;
  }
  /* 第一列，从第一根水平线线到第三根水平线 */

  .cell-panel {
    grid-column: 2;
    grid-row: 1;  /* 等价于1 / 2 */
  }
  /* 第二列，第一行 */

  .cell-inspect {
    grid-column: 2;
    grid-row: 2;  /* 等价于2 / 3 */
  }
  /* 第二列，第二行 */
</style>