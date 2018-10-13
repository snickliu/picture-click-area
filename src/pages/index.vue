<template>
  <div class="p-main">
    <section class="main-head" ref="head" @click="vControl('GETMOUSEPOSINPIC')">
      <canvas ref="canvasHead" width="width" height="400"></canvas>
    </section>
    <section></section>
  </div>
</template>

<script>
export default {
  name: 'Index',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      width: '',
      JPos: {}
    }
  },
  created () {
  },
  mounted () {
    this.width = this.$refs.head.offsetWidth
    this.init()
  },
  methods: {
    // 初始化
    init () {
      let canvas = this.$refs.canvasHead
      let context = canvas.getContext('2d')
      this.drawGrid(context, '#09f', 10, 10)
    },
    drawGrid (context, color, stepx, stepy) {
      context.save()
      context.strokeStyle = color
      context.lineWidth = 0.5
      context.clearRect(0, 0, context.canvas.width, context.canvas.height)
      for (let i = stepx + 0.5; i < context.canvas.width; i += stepx) {
        context.beginPath()
        context.moveTo(i, 0)
        context.lineTo(i, context.canvas.height)
        context.stroke()
      }
      for (let i = stepy + 0.5; i < context.canvas.height; i += stepy) {
        context.beginPath()
        context.moveTo(0, i)
        context.lineTo(context.canvas.width, i)
        context.stroke()
      }
      context.restore()
    },
    getAbsPos (p) {
      let _x = 0
      let _y = 0
      while (p.offsetParent) {
        _x += p.offsetLeft
        _y += p.offsetTop
        p = p.offsetParent
      }
      _x += p.offsetLeft
      _y += p.offsetTop
      return {x: _x, y: _y}
    },
    getMousePos (evt) {
      let _x = 0
      let _y = 0
      evt = evt || window.event
      if (evt.pageX || evt.pageY) {
        _x = evt.pageX
        _y = evt.pageY
      } else if (evt.clientX || evt.clientY) {
        _x = evt.clientX + document.body.scrollLeft - document.body.clientLeft
        _y = evt.clientY + document.body.scrollTop - document.body.clientTop
      } else {
        return this.getAbsPos(evt.target)
      }
      return {x: _x, y: _y}
    },
    vControl (pChoice) {
      switch (pChoice) {
        case 'GETMOUSEPOSINPIC':
          let mPos = this.getMousePos()
          let iPos = this.getAbsPos(this.$refs.head)
          window.status = (mPos.x - iPos.x) + ' ' + (mPos.y - iPos.y)
          alert('x : ' + (mPos.x - iPos.x) + ', y : ' + (mPos.y - iPos.y))
          break
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.p-main {
}
.main-head {
  background-image: url('../assets/img/aaa.png');
  background-size:100% 100%;
  -moz-background-size:100% 100%;
  height: 400px;
  margin: 0 100px 0 100px;
}
</style>
