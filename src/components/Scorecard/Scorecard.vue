<template>
  <div id="score">
    <ul>
      <li
        v-for="item in list"
        :key = "item.id"
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
  props: ['inInitMsg'],
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
          content: (new Date()).getHours()
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
      level: '3'
    }
  },
  watch: {
    inInitMsg (newval, oldval) {
      this.width = newval.width
      this.height = newval.height
      this.list[1].content = this.list[0].content = this.sweeps = newval.sweeps
      this.list[3].content = this.level = newval.level
    }
  }
}
</script>

<style scoped>
div#score {
  width: 100px;
  height: 475px;
  background-color: #747d8c;
  position: absolute;
  box-shadow: 2px 0px 2px#2f3542;
  z-index: 5;
}
li {
  height: 50px;
  line-height: 50px;
  text-align: left;
  padding-left: 5px;
  position: relative;
}
li div {
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
  bottom: 10px;
}
ul:last-of-type li {
  height: 44px;
  width: 90px;
  margin: 3px 5px;
  line-height: 44px;
  text-align: center;
  background-color: rgba(255, 255, 255, .4);
  box-sizing: border-box;
  border-radius: 5px;
  transition: all .5s;
}
ul:last-of-type li:hover {
  cursor: pointer;
  background-color: #66ccff;
  box-shadow: 0 0 0 2px #fff;
}
</style>
