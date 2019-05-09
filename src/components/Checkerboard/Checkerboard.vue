<template>
<div :style="styMsg">
  <ul>
    <li
      v-for = "item in list"
      :key = "item.id"
      @click = "handleClick(item)"
      :class = "{
        'shMines': item.packing
      }"
    >{{item.msg}}</li>
  </ul>
</div>
</template>

<script>
export default {
  props: ['inInitMsg'],
  data () {
    return {
      list: [],
      styMsg: {
        padding: '0',
        width: '880px',
        left: '100px',
        top: '0px',
        maxHeight: '464px',
        maxWidth: '870px'
      },
      numOpens: 0,
      arrMineId: [],
      arrAds: []
    }
  },
  watch: {
    // 监控棋盘数据
    inInitMsg (newval, oldval) {
      this.initAds()
      // 给当前模板生成对应样式
      var styWidth = newval.row * 29
      var styHeight = newval.col * 29 + 10
      newval.row === 0 || newval.col === 0 ? this.styMsg.padding = 0 : this.styMsg.padding = '5px'
      var width = newval.row
      var height = newval.col
      this.styMsg.row = styWidth + 'px'
      this.styMsg.left = (1080 - styWidth) / 2 + 'px'
      this.styMsg.top = (475 - styHeight) / 2 + 'px'
      this.styMsg.maxWidth = styWidth + 'px'
      this.styMsg.maxHeight = styHeight + 'px'
      // 格式化棋盘
      this.list = []
      // 初始化棋盘数据
      for (var i = 1; i < width * height + 1; i++) {
        var scope = this
        this.list.push({
          id: i,
          msg: '',
          neighbor: scope.nbrItem(this),
          ads: {
            row: Math.ceil(i / newval.row),
            col: i - (Math.ceil(i / newval.row) - 1) * newval.row
          },
          packing: 'true',
          bad: this.arrMineId.includes(i) ? 1 : 0
        })
      }
    }
  },
  methods: {
    // 点击旗子
    handleClick (item) {
      // 第一次点击旗子
      if (this.addOpens()) {
        this.arrMineId = this.ranDom(item)
        return
      }
      if (!item.packing) return
      item.packing = !item.packing
      item.bad ? item.msg = '✖' : item.msg = ''
    },
    // 统计打开状态下的旗子数量
    addOpens () {
      for (var i = 0; i < this.list.length; i++) {
        !this.list[i].packing ? this.numOpens++ : void 0
      }
    },
    // 生成一个素组，存放所有雷所在的坐标
    ranDom (item) {
      var max = item.row * item.col
      var arrMineId = []
      var i = 0
      var arrNbrId = this.arrNbrId(item)
      // 生成雷
      while (i < item.sweeps) {
        var ranNum = Math.ceil(Math.random() * max)
        if (arrMineId.includes(ranNum) || arrNbrId.includes(ranNum)) {
          continue
        } else {
          i++
          arrMineId.push(ranNum)
        }
      }
      return arrMineId
    },
    // 计算一个旗子周围的9个邻居旗子
    nbrItem (item) {
      var nbrObj = {
        lu: {},
        u: {},
        ru: {},
        l: {},
        self: {},
        r: {},
        ld: {},
        d: {},
        rd: {}
      }
      // var arrNbrId = this.arrNbrId(item)
      return nbrObj
    },
    // 统计点击的那个旗子周围旗子的id
    arrNbrId (item) {
      var arrNbrId = []
      for (var val in item.neighbor) {
        item.neighbor[val] ? arrNbrId.push(item.neighbor[val].id) : arrNbrId.push(undefined)
      }
      return arrNbrId
    },
    // 统计所有旗子坐标
    initAds () {
      for (var i = 0; i < this.list.length; i++) {
        this.arrAds = this.arrAds.push(this.list[i].ads)
      }
    }
  }
}
</script>

<style scoped>
.shMines {
  background-color: red;
}
div {
  position: absolute;
  border-radius: 5px;
  transition: all .5s;
  background-color: #fff;
  overflow: hidden;
}
ul {
  overflow: hidden;
}
li {
  float: left;
  width: 25px;
  height: 25px;
  box-sizing: border-box;
  margin: 2px;
  border-radius: 2px;
  background-color: #a4b0be;
  text-align: center;
  user-select: none;
}
li:hover {
  box-shadow: 1px 1px 1px #000;
}
</style>
