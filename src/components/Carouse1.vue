<template>
    <div v-show="show">
        <figure ref="spinner" style="width: 4rem; height:auto; transform: rotateY(-0deg); transform-style: preserve-3d;" >
            <figure v-for="(item,index) in spinners" :key="index" :style="item.styleObject" @click="turnPage(index)"
            style="width:4rem;transform: rotateY(0deg) translateZ(2.5rem) scaleY(.9);position:absolute;">
                <img :src="item.imgUrl" style="width:100%;height:100%;">
                <video v-if="index == videoIndex" :class="{'videoClass': videoClass,'bounceOut': bounceOut}" ref="videoplay" :src="spinners[videoIndex].videoUrl" preload="auto" class="bounceIn" style="width:100%;height:100%;position:absolute;top:0;left:0;"></video>
            </figure>
        </figure>
    </div>
</template>
<script>
export default {
  data () {
    return {
      rotate: 0,
      videoIndex: -1,
      videoClass: false,
      bounceOut: false
    }
  },
  props: {
    spinners: {
      type: Array
    },
    show: {
      type: Boolean
    }
  },
  created () {
    this.rotate = 360 / this.spinners.length
  },
  methods: {
    turnPage (index) {
      if (this.videoIndex !== index) {
        let angle = index * this.rotate
        this.videoIndex = -1
        let styleObject = {}
        styleObject.rotateY = '-' + angle + 'deg'
        window.Velocity(this.$refs.spinner, styleObject, 2000, () => {
          this.videoIndex = index
          this.videoClass = true
          this.$nextTick(() => {
            this.$refs.videoplay[0].oncanplay = () => {
              this.$refs.videoplay[0].play()
            }
            this.$refs.videoplay[0].onended = () => {
              this.$refs.videoplay[0].pause()
              this.bounceOut = true
              this.$refs.videoplay[0].addEventListener('webkitAnimationEnd', () => {
                this.videoIndex = -1
                this.bounceOut = false
                this.$emit('girlChooseGift')
              })
            }
          })
        })
      }
    }
  },
  watch: {
  }
}
</script>
<style>
.videoClass {
  position: absolute;
  z-index: 999
}

@-webkit-keyframes bounceIn {
  from, 20%, 40%, 60%, 80%, to {
    -webkit-animation-timing-function: cubic-bezier(0.215, 0.610, 0.355, 1.000);
    animation-timing-function: cubic-bezier(0.215, 0.610, 0.355, 1.000);
  }

  0% {
    opacity: 0;
    -webkit-transform: scale3d(.3, .3, .3);
    transform: scale3d(.3, .3, .3);
  }

  20% {
    -webkit-transform: scale3d(1.1, 1.1, 1.1);
    transform: scale3d(1.1, 1.1, 1.1);
  }

  40% {
    -webkit-transform: scale3d(.9, .9, .9);
    transform: scale3d(.9, .9, .9);
  }

  60% {
    opacity: 1;
    -webkit-transform: scale3d(1.03, 1.03, 1.03);
    transform: scale3d(1.03, 1.03, 1.03);
  }

  80% {
    -webkit-transform: scale3d(.97, .97, .97);
    transform: scale3d(.97, .97, .97);
  }

  to {
    opacity: 1;
    -webkit-transform: scale3d(1, 1, 1);
    transform: scale3d(1, 1, 1);
  }
}

@keyframes bounceIn {
  from, 20%, 40%, 60%, 80%, to {
    -webkit-animation-timing-function: cubic-bezier(0.215, 0.610, 0.355, 1.000);
    animation-timing-function: cubic-bezier(0.215, 0.610, 0.355, 1.000);
  }

  0% {
    opacity: 0;
    -webkit-transform: scale3d(.3, .3, .3);
    transform: scale3d(.3, .3, .3);
  }

  20% {
    -webkit-transform: scale3d(1.1, 1.1, 1.1);
    transform: scale3d(1.1, 1.1, 1.1);
  }

  40% {
    -webkit-transform: scale3d(.9, .9, .9);
    transform: scale3d(.9, .9, .9);
  }

  60% {
    opacity: 1;
    -webkit-transform: scale3d(1.03, 1.03, 1.03);
    transform: scale3d(1.03, 1.03, 1.03);
  }

  80% {
    -webkit-transform: scale3d(.97, .97, .97);
    transform: scale3d(.97, .97, .97);
  }

  to {
    opacity: 1;
    -webkit-transform: scale3d(1, 1, 1);
    transform: scale3d(1, 1, 1);
  }
}

.bounceIn {
  -webkit-animation-duration: 1s;
  animation-duration: 1s;
  -webkit-animation-name: bounceIn;
  animation-name: bounceIn;
}

@-webkit-keyframes bounceOut {
  20% {
    -webkit-transform: scale3d(.9, .9, .9);
    transform: scale3d(.9, .9, .9);
  }

  50%, 55% {
    opacity: 1;
    -webkit-transform: scale3d(1.1, 1.1, 1.1);
    transform: scale3d(1.1, 1.1, 1.1);
  }

  to {
    opacity: 0;
    -webkit-transform: scale3d(.3, .3, .3);
    transform: scale3d(.3, .3, .3);
  }
}

@keyframes bounceOut {
  20% {
    -webkit-transform: scale3d(.9, .9, .9);
    transform: scale3d(.9, .9, .9);
  }

  50%, 55% {
    opacity: 1;
    -webkit-transform: scale3d(1.1, 1.1, 1.1);
    transform: scale3d(1.1, 1.1, 1.1);
  }

  to {
    opacity: 0;
    -webkit-transform: scale3d(.3, .3, .3);
    transform: scale3d(.3, .3, .3);
  }
}

.bounceOut {
  -webkit-animation-duration: 1s;
  animation-duration: 1s;
  -webkit-animation-name: bounceOut;
  animation-name: bounceOut;
}
</style>
