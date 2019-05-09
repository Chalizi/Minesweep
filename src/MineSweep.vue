<template>
  <div id="app">
    <Header></Header>
    <div id="container">
      <Scorecard
        :inInitMsg = "initMsg"
      ></Scorecard>
      <Checkerboard
        :inInitMsg = "initMsg"
      ></Checkerboard>
      <Menu
        @start="handleStart"
      ></Menu>
    </div>
    <Footer></Footer>
    <!-- <div>{{outMsg}}</div> -->
  </div>
</template>

<script>
import Header from './components/Header'
import Footer from './components/Footer'
import Menu from './components/Menu/Menu'
import Scorecard from './components/Scorecard/Scorecard'
import Checkerboard from './components/Checkerboard/Checkerboard'

export default {
  data () {
    return {
      initMsg: {}
    }
  },
  name: 'MineSweep',
  components: {
    'Header': Header,
    'Footer': Footer,
    'Menu': Menu,
    'Scorecard': Scorecard,
    'Checkerboard': Checkerboard
  },
  methods: {
    handleStart (initMsg) {
      this.initMsg = initMsg
    }
  }
}
</script>

<style scoped>
div#app {
  width: 1090px;
  margin: 150px auto;
}
div#container {
  content: "";
  width: 100%;
  position: relative;
  height: 475px;
  /* background-color: rgba(102, 204, 255 .3); */
  background-color: #747d8c;
  /* margin: 20px 0; */
}
</style>
