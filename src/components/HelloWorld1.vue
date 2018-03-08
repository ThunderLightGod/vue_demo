<template>
<div>
  <section class="container" ref="pageList">
        <!-- 第一幅画面 -->
        <section class="page-a bg-adaptive" :class="{'effect-out': pageA}">
        </section>
        <!-- 第二幅画面 -->
        <section class="page-b bg-adaptive" :class="{'effect-out': pageB}">
        </section>
        <!-- 第三幅画面 -->
        <section class="page-c bg-adaptive" :class="{'effect-in': pageC}">
        </section>
    </section>
    选择页面：
    <el-select v-model="page" placeholder="">
      <el-option v-for="(item, index) in pages" :key="index" :label="item.value" :value="item.key"></el-option>
    </el-select>
    <el-button type="primary" @click="playMusic">点击播放音乐</el-button>
    <el-button type="primary" @click="playMusices">点击播放循环音乐</el-button>
</div>
</template>

<script>
export default {
  data () {
    return {
      page: -1,
      index: 0,
      pageA: false,
      pageB: false,
      pageC: false,
      pages: [
        {value: 'page-a', key: 0},
        {value: 'page-b', key: 1},
        {value: 'page-c', key: 2}
      ]
    }
  },
  created () {
  },
  methods: {
    pageAfun () {
      setTimeout(() => { // 采用箭头函数绑定this
        this.$nextTick(() => {
          this.page = 1
        })
      }, 8000)
    },
    pageBfun () {
      setTimeout(() => { // 采用箭头函数绑定this
        this.$nextTick(() => {
          this.page = 2
        })
      }, 8000)
    },
    pageCfun () {
      setTimeout(() => { // 采用箭头函数绑定this
        this.$nextTick(() => {
          // this.page = 0
        })
      }, 8000)
    },
    HTML5Audio (url, loop) {
      let audio = new Audio(url)
      audio.autoplay = true
      audio.loop = loop || false // 是否循环
      audio.play()
      return {
        end: function (callback) {
          audio.addEventListener('ended', function () { // 侦听音乐结束
            callback()
          }, false)
        }
      }
    },
    playMusic () {
      // 背景音乐
      let audio1 = this.HTML5Audio('http://www.tingge123.com/mp3/2017-07-20/1500565663.mp3')
      audio1.end(function () {
        alert('音乐结束')
      })
    },
    playMusices () {
      let audio1 = this.HTML5Audio('http://www.tingge123.com/mp3/2017-07-20/1500565663.mp3')
      audio1.end(() => {
        this.Html5Audio('http://www.tingge123.com/mp3/2017-07-20/1500565663.mp3', true)
      })
    }
  },
  mounted () {
    this.$nextTick(() => {
      this.page = 0
    })
  },
  watch: {
    'page': function () {
      // this.$refs.pageList.children[this.page].style.zIndex = ++this.index
      switch (this.page) {
        case 0:
          this.pageA = false
          this.pageB = false
          this.pageC = false
          this.pageAfun()
          break
        case 1:
          this.pageA = true
          this.pageBfun()
          break
        case 2:
          this.pageB = true
          this.pageC = true
          this.pageCfun()
          break
      }
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
*{
    margin: 0;
    padding: 0;
}
/*body{
    width: 100%;
    height: 100%;
}*/
.container {
    width: 100%;
    height: 100%;
    position: relative;
    overflow: hidden;
}
.bg-adaptive {
    background-size: 100% 100%;
}
.container .page-a {
    width  : 100%;
    height : 100%;
    background-image: url("http://img.mukewang.com/565d07770001790814410901.png");
    position: absolute;
    z-index: 5;
}
.container .page-b {
    width  : 100%;
    height : 100%;
    background-image: url("http://img.mukewang.com/565d09fa000145a614410901.png");
    position: absolute;
    z-index: 4;
}
.page-c {
    width  : 100%;
    height : 100%;
    background-image: url("http://img.mukewang.com/565d0b280001788014410901.png");
    position: absolute;
    z-index: 3;
}
/**
 * 页面切换
 * 镜头方法
 */
.effect-out{
    -webkit-animation: effectOut 8s ease-in-out forwards;
    -webkit-transform-origin:71% 72%;
    -moz-animation: effectOut 8s ease-in-out forwards;
    -moz-transform-origin:71% 72%;
}
@-webkit-keyframes effectOut{
    0% { opacity:1; }
    100% { -webkit-transform: scale(20); opacity:0; }
}
@-moz-keyframes effectOut{
    0% { opacity:1; }
    100% { -moz-transform: scale(20); opacity:0; }
}
.effect-in{
    z-index: 15;
    display: block;
    opacity:0;
    -webkit-transform: scale(8);
    -webkit-animation: effectIn 5s ease-in-out forwards;
    -webkit-transform-origin:58.5% 73.5%;
    -moz-transform: scale(8);
    -moz-animation: effectIn 5s ease-in-out forwards;
    -moz-transform-origin:58.5% 73.5%;
}
@-webkit-keyframes effectIn{
    100% { -webkit-transform: scale(1); opacity:1; }
}
@-moz-keyframes effectIn{
    100% { -moz-transform: scale(1); opacity:1; }
}
</style>
