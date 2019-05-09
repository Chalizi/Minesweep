<template>
  <div id="score">
    <div id="input">
      <select @change="handleChange">
        <option value="3">高级</option>
        <option value="2">中级</option>
        <option value="1">初级</option>
      </select>
      <label for="width">宽:</label><input v-model="width" id="width" />
      <label for="height">高:</label><input v-model="height" id="height" />
      <label for="sweeps">雷:</label><input v-model="sweeps" id="sweeps" />
    </div>
    <ul>
      <li
        v-for = "item in list"
        :key = "item.id"
        @click="handleSubmit"
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
          msg: '开始'
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
      width: '30',
      height: '16',
      sweeps: '99',
      level: '3'
    }
  },
  methods: {
    handleSubmit () {
      var scope = this
      var initMsg = {
        width: scope.width,
        height: scope.height,
        sweeps: scope.sweeps,
        level: scope.level
      }
      this.$emit('start', initMsg)
    },
    handleChange (e) {
      this.level = +e.target.value
      if (this.level === 1) {
        this.height = this.width = 9
        this.sweeps = 10
      } else if (this.level === 2) {
        this.height = this.width = 16
        this.sweeps = 40
      } else {
        this.width = 30
        this.height = 16
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
    background-color: #747d8c;
    position: absolute;
    right: 0;
    box-shadow: -2px 0 2px #2f3542;
  }
  div#input::after {
    content: "";
    clear: both;
    display: block;
  }
  select {
    font-size: 16px;
    border-radius: 5px;
    border: none;
    display: block;
    width: 90px;
    height: 30px;
    margin: 20px 5px 5px;
    padding-left: 20px;
    outline: none;
    box-sizing: border-box;
    /* box-shadow: 0 0 0 5p rgba(255, 255, 255, .4); */
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
    background-color: rgba(255, 255, 255, .4);
    border-color: #fff;
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
    bottom: 10px;
  }
  li {
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
  li:hover {
    cursor: pointer;
    background-color: #66ccff;
    box-shadow: 0 0 0 2px #fff;
  }
</style>
