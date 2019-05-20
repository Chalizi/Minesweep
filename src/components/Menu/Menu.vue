<template>
  <div id="score">
    <div id="input">
      <select @change="handleChange">
        <option value="3">高级</option>
        <option value="2">中级</option>
        <option value="1">初级</option>
      </select>
      <label for="row">宽:</label><input v-model="row" id="row" />
      <label for="col">高:</label><input v-model="col" id="col" />
      <label for="sweeps">雷:</label><input v-model="sweeps" id="sweeps" />
    </div>
    <ul>
      <li
        v-for = "item in list"
        :key = "item.id"
        @click="handleSubmit(item.id)"
      >{{item.msg}}</li>
    </ul>
  </div>
</template>

<script>
export default {
  data () {
    return {
      list: [
        {
          id: 1,
          msg: '新的游戏'
        },
        {
          id: 2,
          msg: '结束'
        },
        {
          id: 3,
          msg: '重置'
        }
      ],
      row: 30,
      col: 16,
      sweeps: 99,
      level: 3,
      click: false
    }
  },
  methods: {
    handleSubmit (id) {
      if (!confirm('将会重置棋盘？确认吗？')) return
      var initMenuMsg
      var click = !this.click
      if (id === 2 || id === 3) {
        initMenuMsg = {
          row: 0,
          col: 0,
          sweeps: 99,
          level: 3,
          click: click
        }
        this.$emit('newGame', initMenuMsg)
        return
      }
      var scope = this
      initMenuMsg = {
        row: +scope.row,
        col: +scope.col,
        sweeps: +scope.sweeps,
        level: +scope.level,
        click: scope.click,
        bol: false
      }
      this.$emit('newGame', initMenuMsg)
    },
    handleChange (e) {
      this.level = +e.target.value
      if (this.level === 1) {
        this.col = this.row = 9
        this.sweeps = 10
      } else if (this.level === 2) {
        this.col = this.row = 16
        this.sweeps = 40
      } else {
        this.row = 30
        this.col = 16
        this.sweeps = 99
      }
    }
  }
}
</script>

<style scoped>
  div#score {
    width: 100px;
    height: 475px;
    background-color: rgba(255, 255, 255, .3);
    position: absolute;
    right: 0;
    box-shadow: -2px 0 2px #2f3542;
    border-radius: 10px;
  }
  div#input::after {
    content: "";
    clear: both;
    display: block;
  }
  select, option {
    appearance:none;
    font-size: 16px;
    border: none;
    display: block;
    outline: none;
    width: 90px;
    height: 30px;
    padding-left: 20px;
    outline: none;
    transition: all .5s;
    height: 44px;
    margin: 5px;
    line-height: 44px;
    text-align: center;
    background-color: #fff;
    box-sizing: border-box;
    border-radius: 5px;
    transition: all .5s;
  }
  select:hover {
    cursor: pointer;
    background-color: #ff6b81;
    box-shadow: 0 0 0 2px #fff;
    transition: all .5s;
  }
  input {
    display: block;
    width: 65px;
    height: 30px;
    margin: 10px 0px;
    padding-left: .2em;
    border: 1px solid #000;
    box-sizing: border-box;
    /* margin-top: 25px; */
    float: left;
    outline: none;
    border-radius: 5px;
    transition: all .5s;
  }
  input:focus {
    background-color: #ff6b81;
    border: 2px solid #fff;
  }
  label {
    display: block;
    float: left;
    width: 30px;
    margin: 10px 0px;
    line-height: 30px;
    text-align: left;
    box-sizing: border-box;
    padding-left: 5px;
    /* margin-top: 25px; */
  }
  ul {
    position: absolute;
    bottom: 5px;
  }
  li {
    height: 44px;
    width: 90px;
    margin: 3px 5px;
    line-height: 44px;
    text-align: center;
    background-color: #fff;
    box-sizing: border-box;
    border-radius: 5px;
    transition: all .5s;
  }
  li:hover {
    cursor: pointer;
    background-color: #ff6b81;
    box-shadow: 0 0 0 2px #fff;
  }
</style>
