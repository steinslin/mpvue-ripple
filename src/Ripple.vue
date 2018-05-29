<template>
  <div class="ripple-wrapper" :class="classes" @click="handleClick" :id="'ripple-wrapper-' + id">
    <slot></slot>
    <div class="ripple-content" :style="style"></div>
  </div>
</template>

<script>
let id = 1
export default {
  name: 'ripple',
  props: {
    duration: {
      type: Number,
      default: 1000
    },
    rippleClass: String,
    type: {
      type: String,
      default: 'fill',
      validator(v) {
        return ['fill', 'circle'].indexOf(v) !== -1
      }
    }
  },
  data () {
    return {
      style: '',
      timer: null,
      wrapperX: 0,
      wrapperY: 0,
      id
    }
  },
  computed: {
    classes () {
      const classes = []
      this.rippleClass && classes.push(this.rippleClass)
      classes.push(`ripple-wrapper-${this.type}`)
      return classes.join(' ')
    }
  },
  created() {
    id++
    const systemInfo = wx.getSystemInfoSync()
    this.pRpx = 750 / systemInfo.screenWidth
  },
  mounted () {
    const query = wx.createSelectorQuery()
    setTimeout(() => {
      query.select(`#ripple-wrapper-${this.id}`).boundingClientRect(rect => {
        this.wrapperX = rect.left
        this.wrapperY = rect.top
      }).exec()
    }, 200)
  },
  methods: {
    handleClick (e) {
      if (this.timer) {
        clearTimeout(this.timer)
        this.timer = null
        this.style = ''
      }
      this.$nextTick(() => {
        const { x, y } = e
        const style = {
          left: `${(x - this.wrapperX) * this.pRpx - 40}rpx`,
          top: `${(y - this.wrapperY) * this.pRpx - 40}rpx`,
          animation: `ripple-${this.type} ${this.duration / 1000}s cubic-bezier(0, 0, 0.2, 1)`
        }
        let s = ''
        Object.keys(style).forEach(k => {
          s += `${k}: ${style[k]};`
        })
        this.style = s
        this.timer = setTimeout(() => {
          this.style = ''
        }, this.duration)
      })
    }
  }
}
</script>

<style lang='scss'>
.ripple-wrapper {
  position: relative;
  .ripple-content {
    background: rgba(18, 150, 220, 0.15);
    border-radius: 50%;
    height: 40px;
    width: 40px;
    position: absolute;
    transform: scale(0);
  }
  &-fill {
    overflow: hidden;
  }
  &-circle {
    .ripple-content {
      background: rgba(18, 150, 220, 0.4);
    }
  }
}
@keyframes ripple-fill {
  0% {
    transform: scale(0);
  }
  100% {
    transform: scale(15);
  }
}
@keyframes ripple-circle {
  0% {
    transform: scale(0);
    opacity: 1;
  }
  100% {
    transform: scale(1.4);
    opacity: 0;
  }
}
</style>
