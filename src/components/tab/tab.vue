<template>
  <div class="tab">
    <router-link tag="div" class="tab-item" to="/recommend">
      <span class="tab-link">推荐</span>
    </router-link>
    <router-link tag="div" class="tab-item" to="/singer">
      <span class="tab-link">歌手</span>
    </router-link>
    <router-link tag="div" class="tab-item" to="/rank">
      <span class="tab-link">排行</span>
    </router-link>
    <router-link tag="div" class="tab-item" to="/search">
      <span class="tab-link">搜索</span>
    </router-link>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .tab
    display flex
    width 100%
    height 40px
    line-height 40px
    background-color #f8f8f8
  .tab-item
    flex 1
    text-align center
    display block
    color rgb(77, 85, 93)
    font-size 14px
    font-weight 200
    &.router-link-active
      border-bottom 2px solid #31c27c
      color: rgb(240, 20, 20)
      .tab-link
        font-weight 400
        padding-bottom 3px
        color #31c27c
</style>
