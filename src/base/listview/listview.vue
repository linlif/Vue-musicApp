<template>
  <scroll class="listview" :data="data">
      <ul>
        <li v-for="group in data" class="list-group">
          <h2 class="list-group-title">{{group.title}}</h2>
          <ul>
            <li v-for="item in group.items" class="list-group-item">
              <img class="avatar" v-lazy="item.avatar" alt="" >
              <span class="name">{{item.name}}</span>
            </li>
          </ul>
        </li>
      </ul>
  </scroll>
</template>

<script type="text/ecmascript-6">
  import Scroll from '../scroll/scroll'
  export default {
    props: {
      data: {
        type: Array,
        default: []
      }
    },
    components: {
      Scroll
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .listview
    position relative
    width 100%
    height 100%
    overflow: hidden
    .list-group
      padding-bottom 0px
    .list-group-title
      height 34px
      line-height 34px
      width 100%
      background-color #dfdfdf
      padding-left 15px
      margin-bottom 15px
    .list-group-item
      height 70px
      padding-left 15px
      display flex
      .avatar
        width 50px
        height 50px
        border-radius 50%
        flex 0 0 50px
      .name
        margin-left 10px
        line-height 50px
        flex-direction column
        justify-content center
</style>
