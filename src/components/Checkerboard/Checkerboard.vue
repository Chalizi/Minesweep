<template>
<div :style="styMsg">
  <ul>
    <li
      v-for = "item in list"
      :key = "item.id"
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
        top: '0px'
      }
    }
  },
  watch: {
    inInitMsg (newval, oldval) {
      var styWidth = newval.width * 29
      var styHeight = newval.height * 29 + 10
      newval.width === 0 || newval.height === 0 ? this.styMsg.padding = 0 : this.styMsg.padding = '5px'
      var width = newval.width
      var height = newval.height
      this.list = []
      this.styMsg.width = styWidth + 'px'
      this.styMsg.left = (1080 - styWidth) / 2 + 'px'
      this.styMsg.top = (475 - styHeight) / 2 + 'px'
      for (var i = 0; i < width * height; i++) {
        this.list.push({msg: ''})
      }
      // alert(this.list[0].msg)
    }
  }
}
</script>

<style scoped>
 div {
   position: absolute;
   border-radius: 5px;
   transition: all .5s;
   background-color: #fff;
   /* box-sizing: content-box; */
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
</style>
