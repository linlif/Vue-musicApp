<template>
  <div class="slider" ref="slider">
    <div class="slider-group" ref="sliderGroup">
      <slot></slot>
    </div>
    <div class="dots">
      <span class="dot" v-for="(item,index) in dots" :class="{active:currentPageIndex===index}"></span>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import {addClass} from '../../common/js/dom'

  export default {
    data () {
      return {
        dots: [],
        currentPageIndex: 0
      }
    },
    props: {
      loop: {
        type: Boolean,
        default: true
      },
      autoPlay: {
        type: Boolean,
        default: true
      },
      interval: {
        type: Number,
        default: 3000
      }
    },
    mounted () {
      // 和$nextTick()的效果一样
      setTimeout(() => {
        this._setSliderWidth()
        this._initSlider()
        this._initDots()
        // 自动滚动
        if (this.autoPlay) {
          this._play()
        }
      }, 20)
      // 增加resize事件监听，当窗口大小改变时刷新better-scroll对象
      window.addEventListener('resize', () => {
        if (!this.slider) {
          return
        }
        // 重新设置宽度，并传isResize的值
        this._setSliderWidth(true)
        // 刷新slider
        this.slider.refresh()
      })
    },
    destroyed () {
      clearTimeout(this.timer)
    },
    methods: {
      _setSliderWidth (isResize) {
        this.children = this.$refs.sliderGroup.children
        let width = 0
        let sliderWidth = this.$refs.slider.clientWidth
        for (let i = 0; i < this.children.length; i++) {
          let child = this.children[i]
          addClass(child, 'slider-item')
          child.style.width = sliderWidth + 'px'
          width += sliderWidth
        }
        // 根据参数决定是否宽度乘以2，防止窗口尺寸改变时意外触发
        if (this.loop && !isResize) {
          width += 2 * sliderWidth
        }
        // 设置子组件的宽度，让better-scroll可以滚动
        this.$refs.sliderGroup.style.width = width + 'px'
      },
      _initSlider () {
        this.slider = new BScroll(this.$refs.slider, {
          scrollX: true,
          scrollY: false,
          momentum: false,
          snap: true,
          snapLoop: this.loop,
          snapThreshold: 0.3,
          snapSpeed: 400,
          click: true
        })
        this.slider.on('scrollEnd', () => {
          let pageIndex = this.slider.getCurrentPage().pageX
          console.log(pageIndex)
          // 当前页面减1，因为无缝滚动时复制多了一份
          if (this.loop) {
            pageIndex -= 1
          }
          this.currentPageIndex = pageIndex
          // 自动滚动
          if (this.autoPlay) {
            clearTimeout(this.timer)
            this._play()
          }
        })
      },
      _initDots () {
        this.dots = new Array(this.children.length - 2)
      },
      _play () {
        let pageIndex = this.currentPageIndex + 1
        if (this.loop) {
          pageIndex += 1
        }
        this.timer = setTimeout(() => {
          this.slider.goToPage(pageIndex, 0, 400)
        }, this.interval)
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .slider
    position relative
    overflow hidden
    white-space nowrap
    .slider-item
      float left
      box-sizing border-box
      overflow hidden
      text-align center
      a
        display block
        overflow hidden
        text-decoration none
        width 100%
      img
        width 100%
        display block

    .dots
      position absolute
      right 0
      left 0
      bottom 10px
      text-align center
      font-size 0;
      .dot
        display inline-block
        width 6px
        height 6px;
        border-radius 50%
        background-color rgba(255,255,255,0.5)
        margin 0 4px
        &.active
          /*width 20px;*/
          border-radius 5px;
          background #fff
</style>
