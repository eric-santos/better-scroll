<template>
  <div class="mouse-wheel-slide-vertical">
    <div class="slide-container">
      <div class="slide-wrapper" ref="slide">
        <div class="slide-content">
          <div class="slide-page page1">page 1</div>
          <div class="slide-page page2">page 2</div>
          <div class="slide-page page3">page 3</div>
          <div class="slide-page page4">page 4</div>
        </div>
      </div>
      <div class="dots-wrapper">
        <span
          class="dot"
          v-for="(item, index) in 4"
          :key="index"
          :class="{'active': currentPageIndex === index}"></span>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from '@better-scroll/core'
  import Slide from '@better-scroll/slide'
  import MouseWheel from '@better-scroll/mouse-wheel'
  BScroll.use(MouseWheel)
  BScroll.use(Slide)

  export default {
    data() {
      return {
        currentPageIndex: 0
      }
    },
    mounted() {
      this.init()
    },
    beforeDestroy() {
      this.slide.destroy()
    },
    methods: {
      init() {
        this.slide = new BScroll(this.$refs.slide, {
          scrollX: false,
          scrollY: true,
          slide: {
            loop: true,
            threshold: 100
          },
          mouseWheel: true,
          momentum: false,
          bounce: false,
          stopPropagation: true
        })
        this.slide.on('scrollEnd', this._onScrollEnd)
      },
      _onScrollEnd() {
        let pageIndex = this.slide.getCurrentPage().pageY
        this.currentPageIndex = pageIndex
      }
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">

.mouse-wheel-slide-vertical
  height 100%
  &.view
    padding 0
    height 100%
  .slide-container
    position relative
    height 100%
    font-size 0
  .slide-wrapper
    height 100%
    overflow hidden
    .slide-page
      display inline-block
      width 100%
      line-height 200px
      text-align center
      font-size 26px
      transform translate3d(0,0,0)
      backface-visibility hidden
      &.page1
        background-color #D6EADF
      &.page2
        background-color #DDA789
      &.page3
        background-color #C3D899
      &.page4
        background-color #F2D4A7
  .dots-wrapper
    position absolute
    right 4px
    top 50%
    transform translateY(-50%)
    .dot
      display block
      margin 4px 0
      width 8px
      height 8px
      border-radius 50%
      background #eee
      &.active
        height  20px
        border-radius 5px

</style>
