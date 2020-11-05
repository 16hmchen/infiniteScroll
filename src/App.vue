<!--
 * @Author: chm
 * @Date: 2020-11-05 20:52:51
 * @LastEditTime: 2020-11-05 21:05:01
 * @LastEditors: Please set LastEditors
 * @Description:
 * @FilePath: /infinityScroll/src/App.vue
-->
<template>
  <div id="app">
    <scroll-view style="width: 50%; height: 400px;" />
  </div>
</template>

<script>
import ScrollView from './components/ScollView'

export default {
  name: 'App',
  components: {
    ScrollView
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  justify-content: center;
}
</style>
