<template>
    <div class="recommend">
      <scroll ref="scroll" class="recommend-content" :data="discList">
        <!--// 必须包裹一个div-->
        <div>
          <!--轮播图-->
          <div v-if="recommends.length" class="slider-wrapper">
            <slider>
              <div v-for="item in recommends">
                <a :href="item.linkUrl">
                  <!-- 监听img的onload事件-->
                  <!--设置needsclick 让fastclick能识别点击-->
                  <!--使用vue-lazyload插件进行图片懒加载-->
                  <img class="needsclick" @load="loadImage" :src="item.picUrl"/>
                </a>
              </div>
            </slider>
          </div>
          <!--推荐歌单-->
          <div class="recommend-list">
            <h1 class="list-title">热门歌单</h1>
            <ul>
              <li v-for="item in discList" class="item">
                <div class="icon">
                  <img width="80" height="80" v-lazy="item.imgurl" alt="">
                </div>
                <div class="text">
                  <h2 class="dissname" v-html="item.dissname"></h2>
                  <div class="name" v-html="item.creator.name"></div>
                  <div class="listennum" v-html="'播放量：'+(item.listennum/10000).toFixed(1)+'万'"></div>
                </div>
              </li>
            </ul>
          </div>
          <div class="loading-container" v-show="!discList.length">
            <loading :title="'loading...'"></loading>
          </div>
        </div>
      </scroll>
    </div>
</template>

<script type="text/ecmascript-6">
  import {getRecommend, getDiscList} from '../../api/recommend'
  import {ERR_OK} from '../../api/config'
  import Slider from '../../base/slider/slider'
  import Scroll from '../../base/scroll/scroll'
  import Loading from '../../base/loading/loading.vue'

  export default {
    data () {
      return {
        recommends: [],
        discList: []
      }
    },
    components: {
      Slider,
      Scroll,
      Loading
    },
    created () {
      // 获取轮播图数据
      this._getRecommend()
      this._getDiscList()
//      setTimeout(() => {
//        this._getDiscList()
//      }, 3000)
    },
    methods: {
      // 定义获取轮播图的方法
      _getRecommend () {
        getRecommend().then((res) => {
          if (res.code === ERR_OK) {
            this.recommends = res.data.slider
          }
        })
      },
      _getDiscList () {
        getDiscList().then((res) => {
          if (res.code === ERR_OK) {
            this.discList = res.data.list
          }
        })
      },
      // 监听每张图片的onload事件，并触发refresh事件
      loadImage () {
        // 确保只刷新一次
        if (!this.checkLoaded) {
          this.$refs.scroll.refresh()
          this.checkLoaded = false
        }
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .recommend
    position fixed
    width 100%
    top 84px
    bottom 0

  .recommend-content
    height 100%
    overflow: hidden

  .slider-wrapper
    position relative
    width 100%
    overflow hidden

  .recommend-list
    .list-title
      height 65px
      line-height 65px
      font-size 18px
      color #31c27c
      text-indent 15px
    .item
      display flex
      align-items center
      padding 0 15px 15px 15px
      boxsizing border-box
      .icon
        flex 0 0 80px
        width 80px
        padding-right 15px
      .text
        display flex
        flex 1
        line-height 18px
        overflow hidden
        font-size 14px
        flex-direction column
        justify-content center
        .dissname
          font-weight 400
          margin-bottom 2px
          color #333
        .name, .listennum
          color #999
          font-weight 200
  .loading-container
    position: absolute
    top 50%
    margin-top 164px
    height 100%
    width 100%
</style>
