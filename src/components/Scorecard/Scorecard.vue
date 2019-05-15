<template>
  <div id="score">
    <ul>
      <li
        v-for="item in list"
        :key = "item.id"
        @click="handleClick(item)"
      >
      {{item.msg + ":"}}
        <div>{{item.content}}</div>
      </li>
    </ul>
    <ul>
      <li
        v-for = "item in btn"
        :key = "item.id"
      >{{item.msg}}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ['inInitMsg', 'inInitState'],
  data () {
    return {
      list: [
        {
          id: 1,
          msg: '雷',
          content: '99'
        },
        {
          id: 2,
          msg: '旗',
          content: '99'
        },
        {
          id: 3,
          msg: '时间',
          content: 0
        },
        {
          id: 4,
          msg: '难度',
          content: '3'
        }
      ],
      btn: [
        {
          id: 1,
          msg: '夜晚'
        },
        {
          id: 2,
          msg: '白天'
        },
        {
          id: 3,
          msg: '亮丽'
        },
        {
          id: 4,
          msg: '鲜艳'
        },
        {
          id: 5,
          msg: '性感'
        }
      ],
      width: '30',
      height: '16',
      sweeps: '99',
      flags: '99',
      level: '3',
      clock: null
    }
  },
  watch: {
    inInitMsg (newval, oldval) {
      this.list[1].content = this.inInitMsg.flags
      this.list[3].content = this.inInitMsg.level
    },
    inInitState (newval, oldval) {
      var scope = this
      var seconds = 0
      var fun = function () {
        scope.list[2].content = ++seconds
      }
      if (newval) {
        this.clock = setInterval(fun, 1000)
      } else {
        clearInterval(this.clock)
      }
    }
  }
}
</script>

<style scoped>
div#score {
  width: 100px;
  height: 470px;
  padding-top: 5px;
  background-color: rgba(255, 255, 255, .3);
  position: absolute;
  box-shadow: 2px 0px 2px#2f3542;
  z-index: 5;
  border-radius: 10px;
}
li {
  background-color: #fff;
  padding-left: 5px;
  position: relative;
  height: 44px;
  width: 90px;
  margin: 3px 5px;
  line-height: 44px;
  box-sizing: border-box;
  border-radius: 5px;
  transition: all .5s;
}
li div {
  color: red;
  font-weight: 800;
  position: absolute;
  top: 0;
  right: 0;
  width: 50px;
  height: 30px;
  text-indent: 1em;
}
ul:first-of-type {
  text-indent: .5em;
}
ul:last-of-type {
  position: absolute;
  bottom: 5px;
}
ul:last-of-type li {
  height: 44px;
  width: 90px;
  margin: 3px 5px;
  line-height: 44px;
  text-align: center;
  box-sizing: border-box;
  border-radius: 5px;
  transition: all .5s;
}
ul:last-of-type li:hover {
  cursor: pointer;
  background-color: #ff6b81;
  box-shadow: 0 0 0 2px #fff;
}
</style>
