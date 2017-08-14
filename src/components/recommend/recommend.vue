<template>
  <div class="recommend">
    <div class="recommend-content">
      <div v-if="recommends.length" class="slider-wrapper">
        <slider>
          <div v-for="item in recommends">
            <a :href="item.linkUrl">
              <img :src="item.picUrl"/>
            </a>
          </div>
        </slider>
      </div>
    </div>

  </div>
</template>

<script type="text/ecmascript-6">
  import {getRecommend} from '../../api/recommend'
  import {ERR_OK} from '../../api/config'
  import Slider from '../../base/slider'

  export default {
    data () {
      return {
        recommends: []
      }
    },
    components: {
      Slider
    },
    created () {
      // 获取轮播图数据
      this._getRecommend()
    },
    methods: {
      // 定义获取轮播图的方法
      _getRecommend () {
        getRecommend().then((res) => {
          if (res.code === ERR_OK) {
            this.recommends = res.data.slider
          }
        })
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">

</style>
