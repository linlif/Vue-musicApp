<template>
  <div class="loading">
    <img src="./loading.gif" alt="">
    <p class="desc" >{{title}}</p>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      title: {
        type: String,
        default: '正在载入..'
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .loading
    width 100%
    text-align center
    img
      width 24px
      height 24px
      margin-bottom 5px
    .desc
      font-size 14px;
      font-weight 400
      color #999
      line-height 18px
</style>
