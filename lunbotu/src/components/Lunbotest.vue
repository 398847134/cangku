<template>
    <div class="public">
        <div class="carousel">
             <div class="car" ref="swiper">
            <p ref="ritem" v-for="(item, index) in list" :key="index"
            @touchstart="touchStart"
            @touchmove="touchMove($event)"
            @touchend="touchEnd($event)"
            :style="{backgroundImage: 'url('+ item.src +')'}"  >{{index}}
            </p>
        </div>
        </div>
    </div>
</template>
<script>
export default {
  data () {
    return {
      startX: '',
      moveX: '',
      list: [
        {src: 'http://chzflive.caihome.cn/web/o_1c7390v9q1al1rak17g2v3uhc438?x-oss-process=image/resize,m_fill,h_240,w_320'},
        {src: 'http://chzflive.caihome.cn/web/o_1c7390v9q1al1rak17g2v3uhc438?x-oss-process=image/resize,m_fill,h_240,w_320'},
        {src: 'http://chzflive.caihome.cn/web/o_1c7390v9q1al1rak17g2v3uhc438?x-oss-process=image/resize,m_fill,h_240,w_320'},
        {src: 'http://chzflive.caihome.cn/web/o_1c7390v9q1al1rak17g2v3uhc438?x-oss-process=image/resize,m_fill,h_240,w_320'},
        {src: 'http://chzflive.caihome.cn/web/o_1c7390v9q1al1rak17g2v3uhc438?x-oss-process=image/resize,m_fill,h_240,w_320'},
        {src: 'http://chzflive.caihome.cn/web/o_1c7390v9q1al1rak17g2v3uhc438?x-oss-process=image/resize,m_fill,h_240,w_320'}
      ],
      contenter: this.$refs.swiper,
      active: 0,
      off: true,
      autoplay: 5000,
      start: null

    }
  },
  mounted: function () {
    this.$nextTick(() => {
      this.backlate()
      this.backtime()
      this._autoplay()
    })
  },

  computed: {
  },
  created () {

  },
  methods: {
    // 图片移动
    backlate (offert) {
      if (-this.active > this.list.length - 1) {
        this.active = 0
      }
      let _that = this
      if (!offert) offert = 0;
      (this.list).forEach((item, index) => {
        this.$refs.ritem[index].style.transform = 'translate3d(' + ((index + this.active) * _that.$refs.ritem[index].clientWidth + offert) + 'px,0,0)'
      })
    },
    // 图片移动时间
    backtime (duration) {
      if (!duration) duration = '0ms';
      (this.$refs.ritem).forEach((item) => {
        item.style.webkitTransition = duration
        item.style.transition = duration
      })
    },
    touchStart (e) {
      this.startX = e.touches[0].pageX
    },
    touchMove (e) {
      e.preventDefault()
      e.stopPropagation()
      this.moveX = e.touches[0].pageX - this.startX
      this.backlate(this.moveX)
    },
    touchEnd (e) {
      this.backtime('300ms')
      if (this.moveX > 100) {
        this.backlate(this.$refs.ritem[0].clientWidth)
        this.active++
      } else if (this.moveX < -100) {
        this.backlate(-this.$refs.ritem[0].clientWidth)
        this.active--
      }
      this.setactive(this.active)
      setTimeout(() => {
        this._autoplay()
      })
    },
    // 循环滚动处理
    setactive (active) {
      if (active === 1) {
        this.active = -(this.list.length - 1)
      } else if (active === -this.list.length) {
        this.active = 0
      } else {
        return false
      }
      this.backtime()
      setTimeout(() => {
        this.backlate()
      }, 300)
    },
    next () {
      this.backtime('300ms')
      this.active--
      this.backlate()
      this.setactive(this.active)
    },
    _autoplay () {
      if (this.autoplay !== '') {
        this.cleartime()
        this.start = setTimeout(() => {
          this.next()
          this._autoplay()
        }, this.autoplay)
      }
    },
    cleartime () {
      clearTimeout(this.start)
      this.start = null
    }

  }
}
</script>
<style>
.carousel {
  overflow: hidden;
  width: 100%;
  height: 300px;
  position: relative;
}
.car {
  width: 100%;
  height:100%;
  position: relative;
}
p{
  position: absolute;
  width: 100%;
  flex: 1;
  height:100%;
  background-size: 100%;
  text-align: center;
  font-size: 30px;
  color: #fff;
  line-height: 500px;
}

</style>
