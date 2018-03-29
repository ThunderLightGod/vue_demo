<template>
  <div>
      <canvas ref="snowflake" id="snowflake"></canvas>
  </div>
</template>
<script>
export default {
  data () {
    return {
      snowArr: [],
      snowNumber: 50,
      width: 0,
      height: 0,
      canvasContext: null
    }
  },
  props: {
    config: {
      type: Object
    }
  },
  methods: {
    initSnow () {
      let options = {
        // 雪球的半球距离
        minRadius: 3,
        maxRadius: 10,
        // 运动的范围区域
        maxX: this.config.clientWidth,
        maxY: this.config.clientHeight,
        // 速率
        minSpeedY: 0.05,
        maxSpeedY: 2,
        speedX: 0.05,
        // 滤镜
        minAlpha: 0.5,
        maxAlpha: 1.0,
        minMoveX: 4,
        maxMoveX: 18
      }
      for (var i = 0; i < this.snowNumber; ++i) {
        let snow = {}
        snow.snowSettings = options
        snow.radius = this.randomInRange(options.minRadius, options.maxRadius)
        // 初始的x位置
        snow.initialX = Math.random() * options.maxX
        snow.y = -(Math.random() * 500)
        // 运行的速率
        snow.speedY = this.randomInRange(options.minSpeedY, options.maxSpeedY)
        snow.speedX = options.speedX
        // 滤镜
        snow.alpha = this.randomInRange(options.minAlpha, options.maxAlpha)
        // 角度.默认是360
        snow.angle = Math.random(Math.PI * 2)
        // 运行的距离
        snow.x = snow.initialX + Math.sin(snow.angle)
        // x移动距离
        snow.moveX = this.randomInRange(options.minMoveX, options.maxMoveX)
        this.snowArr.push(snow)
      }
    },
    Snowflake () {
      this.canvasContext = this.$refs.snowflake.getContext('2d')
      this.$refs.snowflake.width = this.config.clientWidth
      // canvas尺寸修正
      this.$nextTick(() => {
        this.$refs.snowflake.width = this.config.clientWidth
        this.$refs.snowflake.height = this.config.clientHeight
      })
      // 构建雪球对象
      this.initSnow()
      // 渲染雪球对象
      this.renderAndUpdate()
    },
    renderAndUpdate () {
      // 清理之前的矩形数据
      this.canvasContext.clearRect(0, 0, this.config.clientWidth, this.config.clientHeight)
      for (let i = 0; i < this.snowArr.length; ++i) {
        this.render(i)
        this.update(i)
      }
      requestAnimationFrame(this.renderAndUpdate)
    },
    update (index) {
      this.snowArr[index].y += this.snowArr[index].speedY
      if (this.snowArr[index].y > this.snowArr[index].snowSettings.maxY) {
        this.snowArr[index].y -= this.snowArr[index].snowSettings.maxY
      }
      this.snowArr[index].angle += this.snowArr[index].speedX
      if (this.snowArr[index].angle > Math.PI * 2) {
        this.snowArr[index].angle -= Math.PI * 2
      }
      this.snowArr[index].x = this.snowArr[index].initialX + this.snowArr[index].moveX * Math.sin(this.snowArr[index].angle)
    },
    render (index) {
      // 清除路径
      // 开始一个画布中的一条新路径（或者子路径的一个集合）
      this.canvasContext.beginPath()
      // 用来填充路径的当前的颜色，白色的雪球
      this.canvasContext.fillStyle = 'rgba(255, 255, 255,' + this.snowArr[index].alpha + ')'
      // 一个中心点和半径，为一个画布的当前子路径添加一条弧线
      // 坐标，圆，沿着圆指定弧的开始点和结束点的一个角度
      // 弧沿着圆周的逆时针方向（TRUE）还是顺时针方向（FALSE）遍历
      this.canvasContext.arc(this.snowArr[index].x, this.snowArr[index].y, this.snowArr[index].radius, 0, Math.PI * 2, true)
      // 关闭子路径
      this.canvasContext.closePath()
      // fill() 方法使用 fillStyle 属性所指定的颜色、渐变和模式来填充当前路径
      this.canvasContext.fill()
    },
    randomInRange (min, max) {
      let random = Math.random() * (max - min) + min
      return random
    }
  },
  mounted () {
  }
}
</script>
<style scoped>
#snowflake{
    position:absolute;
    left:0;
    top:0;
    z-index: 999999;
}
</style>
