<template>
  <div id="app">
    <Header></Header>
    <div id="container">
      <Scorecard
        :inInitMsg = "initCekMsg"
        :inInitState = "initState"
      ></Scorecard>
      <Checkerboard
        :inInitMsg = "initMenuMsg"
        @gamestate = "handleState"
        @game = "handleGame"
      ></Checkerboard>
      <Menu
        @newGame = "handleNewGame"
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
      initMenuMsg: null,
      initCekMsg: null,
      initState: false
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
    handleNewGame (initMenuMsg) {
      this.initMenuMsg = initMenuMsg
    },
    handleGame (initCekMsg) {
      this.initCekMsg = initCekMsg
    },
    handleState (bol) {
      this.initState = bol
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
  border-radius: 10px;
  content: "";
  width: 100%;
  position: relative;
  height: 475px;
}
</style>
