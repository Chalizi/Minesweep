<template>
<div @contextmenu="handleDefault" :style="styDivMsg">
  <ul>
    <li
      v-for = "item in list"
      :key = "item.id"
      @click = "handleClick(item)"
      @contextmenu = "handleMouseup(item, $event)"
      :index = "item.id - 1"
      :class = "{'redStyLi': !item.opening && redTheme, 'blueStyLi': !item.opening && blueTheme, 'grayStyLi': !item.opening && grayTheme, 'whiteStyLi': !item.opening && whiteTheme, 'violetStyLi': !item.opening && violetTheme}"
    >{{item.msg}}</li>
  </ul>
</div>
</template>

<script>
import '../../assets/js/game.js'

export default {
  props: ['inInitMsg', 'inInitTheme'],
  data () {
    return {
      list: [],
      styDivMsg: {
        padding: '0',
        width: '880px',
        left: '100px',
        top: '0px',
        maxHeight: '464px',
        maxWidth: '870px'
      },
      numOpens: 0, // 打开的棋子个数
      arrMineId: [], // 所有雷的id
      isMarkTrue: 0, // 标记正确的数量
      initCekMsg: {
        flags: 0
      },
      redTheme: true,
      blueTheme: false,
      grayTheme: false,
      whiteTheme: false,
      violetTheme: false
    }
  },
  watch: {
    // 监控棋盘数据
    inInitMsg (newval, oldval) {
      var row = newval.row // 行
      var col = newval.col // 列
      this.styChange(newval, col, row) // 改变当前模板样式
      this.format(newval, row, col) // 棋盘格式化
      this.initCekMsg.flags = newval.sweeps // 绑定旗帜的数量
      this.changeSre() // 雷数改变，旗帜也改
    },
    inInitTheme (newval, oldval) {
      this.redTheme = newval.redTheme
      this.blueTheme = newval.blueTheme
      this.grayTheme = newval.grayTheme
      this.whiteTheme = newval.whiteTheme
      this.violetTheme = newval.violetTheme
    }
  },
  methods: {
    format (newval, col, row) {
      var scope = this // 保存this
      this.list = [] // 棋盘
      this.numOpens = 0 // 打开的棋子个数
      this.isMarkTrue = 0 // 标记正确的数量
      this.arrMineId = [] // 所有雷的id
      // 计算棋盘数据
      for (var i = 1; i < col * row + 1; i++) {
        this.list.push({
          id: i, // 旗子id
          msg: '', // 旗子数据
          neighbor: null, // 旗子邻居
          ads: { // 旗子坐标
            row: Math.ceil(i / col),
            col: i - (Math.ceil(i / col) - 1) * col
          },
          opening: false, // 是否打开
          bad: false, // 是雷吗
          arrNbrId: [], // 周围格子的id
          numNbrBad: undefined, // 周围雷的数量
          flag: false // 标记了吗
        })
      }
      for (var j = 1; j < col * row + 1; j++) {
        this.list[j - 1].neighbor = scope.nbrItem(scope.list[j - 1], col, row) // 计算所有旗子的邻居
        this.list[j - 1].arrNbrId = scope.arrNbrId(scope.list[j - 1]) // 计算所有旗子邻居的id
      }
    },
    // 赢
    victory () {
      alert('你赢了')
      this.$emit('gamestate', false)
    },
    // 输
    defeat () {
      this.$emit('gamestate', false)
      var max = this.inInitMsg.row * this.inInitMsg.col
      for (var i = 0; i < max; i++) {
        var item = this.list[i]
        if (item.opening) continue
        if (item.flag) continue
        var obj = this.searchMines(item)
        var num = obj.num ? obj.num : '' // 查雷
        item.numNbrBad = num
        item.opening = !item.opening // 打开旗子
        item.bad ? item.msg = '💣' : item.msg = num // 检测雷
      }
      alert('你输了')
    },
    // 打开旗子
    opend (item) {
      this.numOpens++
      var obj = this.searchMines(item)
      var num = obj.num ? obj.num : '' // 查雷
      item.numNbrBad = num // 周围雷的数量
      item.opening = !item.opening // 打开旗子
      item.bad ? item.msg = '💣' : item.msg = num // 检测雷
    },
    // 标记旗子
    marked (item) {
      if (item.flag) {
        ++this.initCekMsg.flags
      } else {
        --this.initCekMsg.flags
      }
      item.flag = !item.flag // 旗子已经标记
      item.flag ? item.msg = '🚩' : item.msg = '' // 显示标记
      if (item.flag && item.bad) {
        ++this.isMarkTrue
      } else if (!item.flag && item.bad) {
        --this.isMarkTrue
      }
      this.changeSre() // 同步旗帜数量
    },
    handleDb (item) {
      if (!item.opening) return // 跳过没打开的旗子
      var num = this.searchMines(item).num
      if (num === 0) return // 跳过周围没有雷的旗子
      if (num === this.arrNbrFlags(item).length) {
        var arrItem = this.arrNbrnOpen(item)
        for (var i = 0; i < arrItem.length; i++) {
          if (arrItem[i].bad) this.defeat(arrItem[i]) // 双击触发了雷
          if (arrItem[i].opening) continue // 跳过已经打开的旗子
          if (this.searchMines(arrItem[i]).num === 0) {
            var arrItems = this.allNei(this.clickZero(arrItem[i])).target
            for (var j = 0; j < arrItems.length; j++) {
              this.opend(arrItems[j])
            }
            continue
          }
          this.opend(arrItem[i])
        }
      }
    },
    // 左键事件
    handleClick (item) {
      if (item.opening) {
        this.handleDb(item)
        return
      }
      if (item.flag) return // 如果打开或者已经标记了旗帜，跳过
      if (item.bad) {
        this.defeat() // 输了
        return
      }
      if (!(this.searchMines(item).num === 0) && this.numOpens) {
        this.opend(item)
        return
      }
      this.addOpens() // 有多少个旗子打开了
      // 第一次点击旗子
      if (!this.numOpens) { // 还没有旗子打开
        this.$emit('gamestate', true)
        this.ranDom(item) // 生成雷id
        this.initCekMsg.flags = this.inInitMsg.sweeps
      }
      var arrItem = this.allNei(this.clickZero(item)).target
      for (var i = 0; i < arrItem.length; i++) {
        this.opend(arrItem[i])
      }
    },
    clickZero (item) {
      var obj = {
        newval: {
          target: [item],
          id: [item.id],
          len: 1,
          rounds: 1
        },
        sum: {
          target: [item],
          id: [item.id],
          len: 1
        }
      }
      return this.searchZero(obj)
    },
    // 右键事件
    handleMouseup (item, event) {
      if (item.opening) return // 未打开的旗子
      this.marked(item)
      if (this.isMarkTrue === this.inInitMsg.sweeps) this.victory()
    },
    handleDefault (e) {
      e.preventDefault()
    },
    // 给当前模板生成对应样式
    styChange (newval, col, row) {
      var styWidth = row * 29 // 棋盘长
      var styHeight = col * 29 + 10 // 棋盘高
      col === 0 || row === 0 ? this.styDivMsg.padding = 0 : this.styDivMsg.padding = '5px' // 棋盘内边距
      this.styDivMsg.col = styWidth + 'px' // 宽赋值
      this.styDivMsg.left = (1080 - styWidth) / 2 + 'px' // left赋值
      this.styDivMsg.top = (475 - styHeight) / 2 + 'px' // top赋值
      this.styDivMsg.maxWidth = styWidth + 'px' // 最大宽赋值
      this.styDivMsg.maxHeight = styHeight + 'px' // 最大高赋值
    },
    // 统计打开状态下的旗子数量
    addOpens () {
      for (var i = 0; i < this.list.length; i++) {
        this.list[i].opening ? this.numOpens++ : void 0 // 检查打开了多少个旗子
      }
    },
    // 同步雷和旗帜的数量
    changeSre () {
      var initCekMsg = null
      var scope = this
      initCekMsg = {
        flags: scope.initCekMsg.flags,
        level: scope.inInitMsg.level
      }
      this.$emit('game', initCekMsg)
    },
    // 查雷
    searchMines (item) {
      var obj = {
        num: 0,
        arrId: []
      }
      var nei = item.neighbor
      var num = 0
      for (var val in nei) {
        if (!nei[val]) continue
        if (nei[val].opening) continue
        nei[val].bad ? ++num : void 0
        nei[val].bad ? obj.arrId.push(nei[val].id) : void 0
      }
      obj.num = num
      return obj
    },
    searchZero (obj) {
      var oldval = {
        target: [],
        id: [],
        len: 0,
        rounds: 0
      }
      if (obj.oldval) {
        oldval.target = obj.oldval.target
        oldval.id = obj.oldval.id
        oldval.len = obj.oldval.target.length
        oldval.rounds = obj.oldval.rounds
      }
      var newval = {
        target: obj.newval.target,
        id: obj.newval.id,
        len: obj.newval.target.length,
        rounds: obj.newval.rounds
      }
      var sum = {
        target: obj.sum.target,
        id: obj.sum.id,
        len: obj.sum.target.length
      }
      var newTarget = []
      var newId = []
      var newLen = 0
      var newRounds = 0
      for (var i = 0; i < newval.len; i++) {
        var item = newval.target[i]
        if (item.opening) continue // 跳过打开状态的旗子
        var nei = newval.target[i].neighbor
        for (var val in nei) {
          if (!nei[val]) continue // 跳过undefined
          var id = nei[val].id
          if (oldval.id.includes(id) || newval.id.includes(id)) continue // 跳过这一轮和上一轮已经有的元素
          if (newId.includes(id)) continue // 跳过刚查找的元素中已经有的元素
          var num = this.searchMines(nei[val]).num
          if (num === 0) {
            newTarget.push(nei[val])
            newId.push(nei[val].id)
          }
        }
      }
      oldval.target = newval.target
      oldval.id = newval.id
      oldval.len = newval.target.length
      oldval.rounds = newval.rounds
      if (!newId[0]) return obj.sum
      newLen = newId.length
      newRounds = oldval.rounds + 1
      newval.target = newTarget
      newval.id = newId
      newval.len = newLen
      newval.rounds = newRounds
      sum.target = sum.target.concat(newTarget)
      sum.id = sum.id.concat(newId)
      sum.len = sum.target.length
      var newObj = {
        newval: newval,
        oldval: oldval,
        sum: sum
      }
      return this.searchZero(newObj)
    },
    allNei (obj) {
      var target = obj.target
      var id = obj.id
      var newObj = {
        target: [],
        id: []
      }
      for (var i = 0; i < target.length; i++) {
        var nei = target[i].neighbor
        for (var val in nei) {
          if (!nei[val]) continue // 跳过undefined
          if (nei[val].opening) continue // 跳过已经打开的旗子
          if (newObj.id.includes(nei[val].id) || id.includes(nei[val].id)) continue // 跳过已经push的以及原有的
          newObj.target.push(nei[val])
          newObj.id.push(nei[val].id)
        }
      }
      newObj.target = target.concat(newObj.target)
      newObj.id = id.concat(newObj.id)
      return newObj
    },
    // 生成对应雷的个数
    ranDom (item) {
      var max = this.inInitMsg.row * this.inInitMsg.col
      if (+item.arrNbrId.length >= max || +this.inInitMsg.sweeps > max - item.arrNbrId.length) {
        alert('雷的数量超出了')
        return
      }
      var ranNum // 随机数
      var arr = [] // 初始化所有雷的id
      var i = 0
      // 生成雷
      while (i < this.inInitMsg.sweeps) {
        ranNum = this.rdmNum(max)
        if (arr.includes(ranNum) || item.arrNbrId.includes(ranNum)) {
          continue
        } else {
          i++
          this.list[ranNum - 1].bad = true
          arr.push(ranNum)
        }
      }
      this.arrMineId = arr
    },
    // 生成一个1-num大小的随机数
    rdmNum (max) {
      return Math.ceil(Math.random() * max)
    },
    // 计算一个旗子周围的9个邻居旗子
    nbrItem (item, maxRow, maxCol) {
      var scope = this
      var row = item.ads.row
      var col = item.ads.col
      var lu = this.adsToid(row - 1, col - 1, maxRow, maxCol) - 1
      var u = this.adsToid(row - 1, col, maxRow, maxCol) - 1
      var ru = this.adsToid(row - 1, col + 1, maxRow, maxCol) - 1
      var l = this.adsToid(row, col - 1, maxRow, maxCol) - 1
      var r = this.adsToid(row, col + 1, maxRow, maxCol) - 1
      var ld = this.adsToid(row + 1, col - 1, maxRow, maxCol) - 1
      var d = this.adsToid(row + 1, col, maxRow, maxCol) - 1
      var rd = this.adsToid(row + 1, col + 1, maxRow, maxCol) - 1
      var nbrObj = {
        lu: scope.list[lu],
        u: scope.list[u],
        ru: scope.list[ru],
        l: scope.list[l],
        self: item,
        r: scope.list[r],
        ld: scope.list[ld],
        d: scope.list[d],
        rd: scope.list[rd]
      }
      return nbrObj
    },
    // 坐标转id
    adsToid (row, col, maxRow, maxCol) {
      if (col <= 0 || row <= 0 || col > maxRow || row > maxCol) {
        return 0
      }
      return (row - 1) * maxRow + col
    },
    // 统计点击的那个旗子周围旗子的id
    arrNbrId (item) {
      var arr = []
      for (var val in item.neighbor) {
        item.neighbor[val] ? arr.push(item.neighbor[val].id) : void 0
      }
      return arr
    },
    arrNbrFlags (item) {
      var arr = []
      var nei = item.neighbor
      for (var val in nei) {
        if (!nei[val]) continue
        nei[val].flag ? arr.push(nei[val].id) : void 0
      }
      return arr
    },
    arrNbrnOpen (item) {
      var arr = []
      var nei = item.neighbor
      for (var val in nei) {
        if (!nei[val]) continue
        if (nei[val].flag) continue
        if (nei[val].opening) continue
        arr.push(nei[val])
      }
      return arr
    }
  }
}
</script>

<style scoped>
.redStyLi {
  background-color: red;
}
.blueStyLi {
  background-color: blue;
}
.whiteStyLi {
  background-color: white;
}
.violetStyLi {
  background-color: violet;
}
.grayStyLi {
  background-color: gray;
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
