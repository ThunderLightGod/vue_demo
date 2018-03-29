// pageAfun () {
    //   return this.transition(this.$refs.boy, {
    //     time: 10000,
    //     style: {
    //       top: '4rem',
    //       right: '16rem',
    //       scale: '1.2'
    //     }
    //   }).then(() => {
    //     return this.transition(this.$refs.boy, {
    //       time: 500,
    //       style: {
    //         rotateY: '-180',
    //         scale: '1.5'
    //       }
    //     })
    //   }).catch(() => {
    //     console.log(console)
    //   }).then(() => {
    //     return this.transition(this.$refs.boy, {
    //       time: 7000,
    //       style: {
    //         top: '7.8rem',
    //         right: '1.2rem'
    //       }
    //     })
    //   }).catch(() => {
    //     console.log(console)
    //   }).finally(() => {
    //     this.stopWalk()
    //   })
    // },


    // boyAction () {
    //   return this.transition(this.$refs.christmasBoy, {
    //     time: 4000,
    //     style: {
    //       right: '4.5rem'
    //     }
    //   }).then(() => {
    //     // 停止走路
    //     this.boyWalk = false
    //     this.boyStand = true
    //   }).then(() => {
    //     // 解开包裹
    //     this.boyUnwrapp = true
    //     this.boyStand = false
    //     return this.$refs.christmasBoy.addEventListener('webkitAnimationEnd', () => {
    //       return Promise.resolve()
    //     })
    //   }).then(() => {
    //     // 脱衣动作
    //     setTimeout(() => { // 采用箭头函数绑定this
    //       this.boyStrip1 = true
    //       this.boyUnwrapp = false
    //       this.giftShow = true
    //       // 显示礼物界面
    //       this.$nextTick(() => {
    //         this.transition(this.$refs.gift, {
    //           time: 2000,
    //           style: {
    //             left: '6.8rem',
    //             top: '1.5rem',
    //             perspective: '500px',
    //             transform: 'scale3d(.8,.8,.8)'
    //           }
    //         })
    //       })
    //     }, 1000)
    //     setTimeout(() => { // 采用箭头函数绑定this
    //       this.boyStrip2 = true
    //       this.boyUnwrapp = false
    //     }, 2000)
    //     setTimeout(() => { // 采用箭头函数绑定this
    //       this.boyStrip3 = true
    //       this.boyUnwrapp = false
    //     }, 3000)
    //     setTimeout(() => { // 采用箭头函数绑定this
    //       this.boyHug = true
    //       this.$refs.christmasBoy.addEventListener('webkitAnimationEnd', () => {
    //         this.christmasBoyHead = true
    //       })
    //     }, 10000)
    //   })
    // },

    // girlAction () {
    //   this.girlStandUpFun().then(() => {
    //     this.girlWalk = true
    //     return this.transition(this.$refs.girl, {
    //       time: 4000,
    //       style: {
    //         left: '4.5rem'
    //       }
    //     })
    //   }).then(() => {
    //     return this.girlStopWalk()
    //   }).then(() => {
    //   })
    // },

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