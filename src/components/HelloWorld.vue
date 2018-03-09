<template>
<div>
  <section class="container" ref="pageList">
        <!-- 第一幅画面 -->
        <section class="page-a bg-adaptive" :class="{'effect-out': pageA}">
            <!-- 月亮 -->
            <figure class="moon"></figure>
            <!-- 圣诞树 -->
            <figure class="tree"></figure>
            <!-- 云 -->
            <figure class="cloudy"></figure>
            <!-- 男孩 -->
            <div ref="boy" class="chs-boy" :class="{'chs-boy-deer': boyDeep}"></div>
            <!-- 窗户 -->
            <div ref="window" class="window wood">
                <div class="window-bg"></div>
                <div class="window-content">
                    <div ref="windowleft" class="window-left" :class="{'window-transition': windowOpen,'hover': windowOpen}"></div>
                    <div ref="windowright" class="window-right" :class="{'window-transition': windowOpen,'hover': windowOpen}"></div>
                </div>
            </div>
        </section>
        <!-- 第二幅画面 -->
        <section class="page-b bg-adaptive" :class="{'effect-out': pageB}">
        <!-- 圣诞男孩 -->
            <!-- 猫 -->
            <figure class="cat"></figure>
            <!-- 小女孩 -->
            <figure ref="girl" class="girl" :class="{'girl-standUp': girlStandUp,'girl-throwBook': girlThrowBook,
            'girl-walk': girlWalk}"></figure>
            <figure v-show="christmasBoyHead" class="christmas-boy-head"></figure>
            <figure ref="christmasBoy" class="christmas-boy" :class="{'boy-walk': boyWalk, 'boy-stand': boyStand,
            'boy-unwrapp': boyUnwrapp, 'boy-strip-1': boyStrip1, 'boy-strip-2': boyStrip2, 'boy-strip-3': boyStrip3,
            'boy-hug': boyHug}"> </figure>
        </section>
        <!-- 第三幅画面 -->
        <section class="page-c bg-adaptive" :class="{'effect-in': pageC}">
        </section>
    </section>
    <el-select v-model="page" placeholder="">
      <el-option v-for="(item, index) in pages" :key="index" :label="item.value" :value="item.key"></el-option>
    </el-select>
    <el-button type="primary" @click="playMusic">点击播放音乐</el-button>
    <el-button type="primary" @click="playMusices">点击播放循环音乐</el-button>
    <el-button type="primary" @click="playBoy">点击运行雪橇动作</el-button>
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
      ],
      boyDeep: true,
      windowOpen: false,
      boyWalk: true,
      boyStand: false,
      boyUnwrapp: false,
      boyStrip1: false,
      boyStrip2: false,
      boyStrip3: false,
      boyHug: false,
      christmasBoyHead: false,
      girlStandUp: false,
      girlThrowBook: false,
      girlWalk: false
    }
  },
  created () {
  },
  methods: {
    pageAfun () {
      return this.transition(this.$refs.boy, {
        time: 10000,
        style: {
          top: '4rem',
          right: '16rem',
          scale: '1.2'
        }
      }).then(() => {
        return this.transition(this.$refs.boy, {
          time: 500,
          style: {
            rotateY: '-180',
            scale: '1.5'
          }
        })
      }).catch(() => {
        console.log(console)
      }).then(() => {
        return this.transition(this.$refs.boy, {
          time: 7000,
          style: {
            top: '7.8rem',
            right: '1.2rem'
          }
        })
      }).catch(() => {
        console.log(console)
      }).finally(() => {
        this.stopWalk()
      })
      // setTimeout(() => { // 采用箭头函数绑定this
      //   this.$nextTick(() => {
      //     // this.page = 1
      //   })
      // }, 8000)
    },
    pageBfun () {
      this.boyAction()
      this.girlAction()
      setTimeout(() => { // 采用箭头函数绑定this
        this.$nextTick(() => {
          // this.page = 2
        })
      }, 8000)
    },
    stopWalk () {
      this.boyDeep = false
      this.openWindow()
    },
    pageCfun () {
      setTimeout(() => { // 采用箭头函数绑定this
        this.$nextTick(() => {
          // this.page = 0
        })
      }, 8000)
    },
    transition (el, options) {
      return new Promise((resolve, reject) => {
        resolve(window.Velocity(el, options.style, options.time))
      })
    },
    playBoy () {
      this.Christmas()
    },
    run () {},
    openWindow () {
      this.windowOpen = true
    },
    Christmas () {
      // this.pageAfun()
      this.pageBfun()
    },
    boyAction () {
      return this.transition(this.$refs.christmasBoy, {
        time: 4000,
        style: {
          right: '4.5rem'
        }
      }).then(() => {
        // 停止走路
        this.boyWalk = false
        this.boyStand = true
      }).then(() => {
        // 解开包裹
        this.boyUnwrapp = true
        this.boyStand = false
        return this.$refs.christmasBoy.addEventListener('webkitAnimationEnd', () => {
          return Promise.resolve()
        })
      }).then(() => {
        // 脱衣动作
        setTimeout(() => { // 采用箭头函数绑定this
          this.boyStrip1 = true
          this.boyUnwrapp = false
        }, 1000)
        setTimeout(() => { // 采用箭头函数绑定this
          this.boyStrip2 = true
          this.boyUnwrapp = false
        }, 2000)
        setTimeout(() => { // 采用箭头函数绑定this
          this.boyStrip3 = true
          this.boyUnwrapp = false
        }, 3000)
        setTimeout(() => { // 采用箭头函数绑定this
          this.boyHug = true
          this.$refs.christmasBoy.addEventListener('webkitAnimationEnd', () => {
            this.christmasBoyHead = true
          })
        }, 4000)
      })
    },
    girlAction () {
      this.girlStandUpFun().then(() => {
        this.girlWalk = true
        return this.transition(this.$refs.girl, {
          time: 4000,
          style: {
            left: '4.5rem'
          }
        }).then(() => {
        })
      })
    },
    girlStandUpFun () {
      return new Promise((resolve, reject) => {
        setTimeout(() => { // 采用箭头函数绑定this
          this.girlStandUp = true
        }, 200)
        setTimeout(() => { // 采用箭头函数绑定this
          this.girlThrowBook = true
          resolve()
        }, 500)
      })
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
    // this.$nextTick(() => {
    //   this.page = 0
    // })
  },
  watch: {
    'page': function () {
      // this.$refs.pageList.children[this.page].style.zIndex = ++this.index
      // switch (this.page) {
      //   case 0:
      //     this.pageA = false
      //     this.pageB = false
      //     this.pageC = false
      //     this.pageAfun()
      //     break
      //   case 1:
      //     this.pageA = true
      //     this.pageBfun()
      //     break
      //   case 2:
      //     this.pageB = true
      //     this.pageC = true
      //     this.pageCfun()
      //     break
      // }
    },
    'windowOpen': function () {
      if (this.windowOpen) {
        this.$refs.windowleft.addEventListener('transitionend webkitTransitionEnd', () => {
          this.$nextTick(() => {
            this.windowOpen = false
          })
        })
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
        z-index: 6;
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
    /**
    * 圣诞树
    * animation: name duration timing-function delay iteration-count direction;
    */
    .tree {
        width: 2.71rem;
        height: 4.24rem;
        z-index: 15;
        position: absolute;
        bottom: 0;
        left: 1rem;
        background-image: url(http://img.mukewang.com/565d07d30001c97605420424.png);
        background-size: 200% 100%;
        -webkit-animation: treeAnim 1s steps(2) infinite;
        -moz-animation: treeAnim 1s steps(2) infinite;
    }
    @-webkit-keyframes treeAnim {
        0% {
            background-position: 0% 100%;
        }
        100% {
            background-position: -200% 100%;
        }
    }
    @-moz-keyframes treeAnim {
        0% {
            background-position: 0% 100%;
        }
        100% {
            background-position: -200% 100%;
        }
    }
    /*月亮*/
    .moon {
        background: #FCF0BC;
        width: 2rem;
        height: 2rem;
        border-radius: 50%;
        box-shadow: 0 0 1.5rem #FCF0BC;
        position: absolute;
        left: 3.3rem;
        top: .8rem;
        -webkit-animation: nucleus 2s infinite linear;
        -moz-animation: nucleus 2s infinite linear;
    }
    /**
    * 光晕效果
    */
    @-webkit-keyframes nucleus {
        0% {
            box-shadow: 0 0 0 transparent;
        }
        50% {
            box-shadow: 0 0 1rem #FCF0BC;
        }
        100% {
            box-shadow: 0 0 0 transparent;
        }
    }
    @-moz-keyframes nucleus {
        0% {
            box-shadow: 0 0 0 transparent;
        }
        50% {
            box-shadow: 0 0 1rem #FCF0BC;
        }
        100% {
            box-shadow: 0 0 0 transparent;
        }
    }
    /*云*/
    .cloudy {
        background: #60768D;
        border-radius: 50%;
        box-shadow: #60768D 1.2rem -0.2rem 0 -0.1rem, #60768D 0.5rem -0.5rem, #60768D 0.8rem 0.2rem,#60768D 1.5rem 0.2rem 0 -0.2rem;
        height: 1rem;
        width: 1rem;
        position: absolute;
        left: .5rem;
        top: 1.8rem;
        z-index: 5;
        -webkit-animation: cloudy 5s ease-in-out infinite;
        -moz-animation: cloudy 5s ease-in-out infinite;
    }
    @-webkit-keyframes cloudy {
        50% {
            -webkit-transform: translateY(-0.1rem);
        }
    }
    @-moz-keyframes cloudy {
        50% {
            -moz-transform: translateY(-0.1rem);
        }
    }
    /**
    * 圣诞男孩
    */

    .chs-boy {
        width           : 5rem;
        height          : 1.5rem;
        position        : absolute;
        z-index         : 3;
        top             : .1rem;
        right           : -3rem;
        transform       : scale(0.1);
        background      : url(http://img.mukewang.com/565d07490001365329660269.png) -300% -100%;
        background-size : 400% 100%;
    }

    /**
    * 男孩走路动作
    */

    .chs-boy-deer {
        -webkit-animation:chsBoyDeer 0.75s steps(3,end) infinite;
        -moz-animation:chsBoyDeer 0.75s steps(3,end) infinite;
    }

    @-webkit-keyframes chsBoyDeer {
        0% {
            background-position: -0% 100%;
        }
        100% {
            background-position: -300% 100%;
        }
    }
    @-moz-keyframes chsBoyDeer {
        0% {
            background-position: -0% 100%;
        }
        100% {
            background-position: -300% 100%;
        }
    }

    /**
    * 人物停止
    */

    .boy-stop-animate {
        -webkit-animation-play-state: paused;
    }

    /**
    * 窗户
    */

    .window {
        width: 2.6rem;
        height: 1.5rem;
        position: absolute;
        left: 9.7rem;
        top: 6.2rem;
        cursor: pointer;
        -webkit-perspective: 500px;
        -moz-perspective: 500px;
    }

    .window-content {
        -webkit-transform-style: preserve-3d;
        -moz-transform-style: preserve-3d;
        width: 91%;
        margin: 0 auto;
        height: 100%;
        overflow: hidden;
    }

    /**
    * 窗户背景
    */

    .window-bg {
        width: 86%;
        height: 92%;
        position: absolute;
        left: 50%;
        margin-left: -43%;
        bottom: 0;
        background: url(http://img.mukewang.com/565d07770001790814410901.png);
        background-size: 100% 100%;
        z-index: -1;
    }

    /**
    * 窗户底边
    * @type {[type]}
    */

    .window:before {
        content: "";
        background: url(http://img.mukewang.com/565d07e40001088402410017.png);
        width: 100%;
        height: 0.17rem;
        display: block;
        position: absolute;
        bottom: 0.05rem;
        background-size: 100% 100%;
        z-index: 100;
    }

    /**
    * 底边阴影
    * @type {[type]}
    */

    .window:after {
        content: "";
        background: url(http://img.mukewang.com/565d080400018d2702270009.png);
        width: 100%;
        height: 0.09rem;
        display: block;
        position: absolute;
        bottom: 0;
        background-size: 100% 100%;
        z-index: 100;
    }

    .wood {
        display: block;
        overflow: hidden;
    }

    /**
    * 左侧门
    */

    .window-left {
        float: left;
        background: url(http://img.mukewang.com/565d081d0001cfd901140134.png);
        -webkit-border-top-left-radius: 0.1rem;
        -moz-border-top-left-radius: 0.1rem;
    }

    /**
    * 右侧门
    */

    .window-right {
        float: right;
        background: url(http://img.mukewang.com/565d084c0001431b01140134.png);
        -webkit-border-top-right-radius: 0.1rem;
        -moz-border-top-left-radius: 0.1rem;
    }

    .window-left,
    .window-right {
        width: 1.17rem;
        height: 1.3rem;
        z-index: 110;
        box-shadow: 0 0 0.15rem #FCF0BC;
        background-size: 100% 100%;
    }

    .window-animation {
        -webkit-transition: 2s ease-in-out;
        -moz-transition: 2s ease-in-out;
    }

    /**
    * 动画过程
    */

    .window-transition {
        -webkit-transition: 2s ease-in-out;
        -moz-transition: 2s ease-in-out;
    }

    .window-left.hover {
    -webkit-transform: scale(0.95) rotateY(60deg);
        -moz-transform: scale(0.95) rotateY(60deg);
        margin-top: 0.1rem;
        margin-left: -0.25rem;
    }

    .window-right.hover {
    -webkit-transform: scale(0.95) rotateY(-60deg);
        -moz-transform: scale(0.95) rotateY(-60deg);
        margin-top: 0.1rem;
        margin-right: -0.25rem;
    }
/********************************************************
  小男孩动作
**********************************************************/

.walk-stop {
    -webkit-animation-play-state: paused;
    -moz-animation-play-state: paused;
}

.walk-run {
    -webkit-animation-play-state: running;
    -moz-animation-play-state: running;
}

/**
 * 圣诞小男孩
 */

.christmas-boy {
    width: 3.5rem;
    height: 4.06rem;
    position: absolute;
    z-index: 5;
    right: -3.5rem;
    top: 4rem;
    background: url(http://img.mukewang.com/565d09870001372152510407.png);
    background-size: 1500% 100%;
    background-position: 0% 100%;
}

/**
 * 男孩走路动作
 */

.boy-walk {
    -webkit-animation: boyWalk 0.75s steps(4, end) infinite;
    -moz-animation: boyWalk 0.75s steps(4, end) infinite;
}

@-webkit-keyframes boyWalk {
    0% {
        background-position: 0 100%;
    }
    100% {
        background-position: -400% 100%;
    }
}

@-moz-keyframes boyWalk {
    0% {
        background-position: 0 100%;
    }
    100% {
        background-position: -400% 100%;
    }
}

/**
 * 脱衣动作
 */

.boy-strip-1 {
    background-position: -800% 100%;
}

.boy-strip-2 {
    background-position: -900% 100%;
}

.boy-strip-3 {
    background-position: -1000% 100%;
}

/**
 * 站立动作
 */

.boy-stand {
    background-position: -400% 100%;
}

/**
 * 打开包裹
 */

.boy-unwrapp {
    -webkit-animation: unwrapp 2s steps(2, end) 1 forwards;
    -moz-animation: unwrapp 2s steps(2, end) 1 forwards;
}

@-webkit-keyframes unwrapp {
    0% {
        background-position: -400% 100%;
    }
    100% {
        background-position: -600% 100%;
    }
}

@-moz-keyframes unwrapp {
    0% {
        background-position: -400% 100%;
    }
    100% {
        background-position: -600% 100%;
    }
}

/**
 * 男孩拥抱
 */

.boy-hug {
    -webkit-animation: boyHug 1s steps(3, end) 1 forwards;
    -moz-animation: boyHug 1s steps(3, end) 1 forwards;
}

@-webkit-keyframes boyHug {
    0% {
        background-position: -1000% 100%;
    }
    100% {
        background-position: -1300% 100%;
    }
}
@-moz-keyframes boyHug {
    0% {
        background-position: -1000% 100%;
    }
    100% {
        background-position: -1300% 100%;
    }
}

/**
 * 男孩头部
 */

.christmas-boy-head {
    left: 7.85rem;
    top: 4rem;
    width: 3.5rem;
    height: 4.06rem;
    position: absolute;
    z-index: 12;
    background: url(http://img.mukewang.com/565d09870001372152510407.png);
    background-size: 1400% 100%;
    background-position: -1300% 100%;
    /* display: none; */
}
/********************************************************
  小女孩动作
**********************************************************/
/**
 * 小女孩看书
 */

.girl {
    width: 3.5rem;
    height: 4.06rem;
    position: absolute;
    z-index: 10;
    left: 1rem;
    top: 3.2rem;
    background: url(http://img.mukewang.com/565d09e50001156273510407.png);
    background-size: 2100% 100%;
}

/**
 * 起身
 */

.girl-standUp {
    left: 1.2rem;
    -webkit-animation: standUp 200ms steps(3, start) forwards;
    -moz-animation: standUp 200ms steps(3, start) forwards;
}

@-webkit-keyframes standUp {
    0% {
        background-position: 0% 100%;
    }
    100% {
        background-position: -300% 100%;
        top: 4rem;
    }
}

@-moz-keyframes standUp {
    0% {
        background-position: 0% 100%;
    }
    100% {
        background-position: -300% 100%;
        top: 4rem;
    }
}

/**
 * 抛书动作
 */

.girl-throwBook {
    top: 4rem;
    left: 0.5rem;
    -webkit-animation: throwBook 300ms steps(2, start) forwards;
    -moz-animation: throwBook 300ms steps(2, start) forwards;
}

@-webkit-keyframes throwBook {
    0% {
        background-position: -300% 100%;
    }
    100% {
        background-position: -500% 100%;
    }
}

@-moz-keyframes throwBook {
    0% {
        background-position: -300% 100%;
    }
    100% {
        background-position: -500% 100%;
    }
}

/**
 * 小女孩走路
 */

.girl-walk {
    left: 1rem;
    -webkit-animation: girlWalk 900ms steps(4, start) infinite;
    -moz-animation: girlWalk 900ms steps(4, start) infinite;
}

@-webkit-keyframes girlWalk {
    0% {
        background-position: -500% 100%;
    }
    100% {
        background-position: -900% 100%;
    }
}

@-moz-keyframes girlWalk {
    0% {
        background-position: -500% 100%;
    }
    100% {
        background-position: -900% 100%;
    }
}

/**
 * 站立动作
 */

.girl-stand {
    top: 4rem;
    background-position: -1000% 100%;
}

/**
 * 小女孩选择3d选择
 */

.girl-choose {
    -webkit-animation: girlChoose 2000ms steps(2, end) forwards;
    -moz-animation: girlChoose 2000ms steps(2, end) forwards;
}

@-webkit-keyframes girlChoose {
    0% {
        background-position: -1000% 100%;
    }
    100% {
        background-position: -1200% 100%;
    }
}

@-moz-keyframes girlChoose {
    0% {
        background-position: -1000% 100%;
    }
    100% {
        background-position: -1200% 100%;
    }
}

/**
 * 流泪奔跑
 */

.girl-weep {
    -webkit-animation: girlWeep 450ms steps(4, end) forwards infinite;
    -moz-animation: girlWeep 450ms steps(4, end) forwards infinite;
}

@-webkit-keyframes girlWeep {
    0% {
        background-position: -1300% 100%;
    }
    100% {
        background-position: -1700% 100%;
    }
}

@-moz-keyframes girlWeep {
    0% {
        background-position: -1300% 100%;
    }
    100% {
        background-position: -1700% 100%;
    }
}

/**
 * 女孩拥抱
 */

.girl-hug {
    -webkit-animation: girlHug 450ms steps(3, end) forwards;
    -moz-animation: girlHug 450ms steps(3, end) forwards;
}

@-webkit-keyframes girlHug {
    0% {
        background-position: -1700% 100%;
    }
    100% {
        left: 7.85rem;
        background-position: -2000% 100%;
    }
}

@-moz-keyframes girlHug {
    0% {
        background-position: -1700% 100%;
    }
    100% {
        left: 7.85rem;
        background-position: -2000% 100%;
    }
}
</style>
