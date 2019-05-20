<template>
  <div id="app">
    <Header></Header>
    <div id="container">
      <Scorecard
        :inInitMsg = "initCekMsg"
        :inInitState = "initState"
        @theme = "handleTheme"
      ></Scorecard>
      <Checkerboard
        :inInitMsg = "initMenuMsg"
        :inInitTheme = "initTheme"
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
  name: 'MineSweep',
  data () {
    return {
      initMenuMsg: null,
      initCekMsg: null,
      initTheme: null,
      initState: false
    }
  },
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
      this.initState = initMenuMsg.bol
    },
    handleGame (initCekMsg) {
      this.initCekMsg = initCekMsg
      this.initCekMsg.sweeps = this.initMenuMsg.sweeps
    },
    handleState (bol) {
      this.initState = bol
    },
    handleTheme (obj) {
      this.initTheme = obj
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
