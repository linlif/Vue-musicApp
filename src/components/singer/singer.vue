<template>
  <div class="singer">
    <listview :data="singers"></listview>
  </div>
</template>

<script type="text/ecmascript-6">
  import {getSingerList} from '../../api/singer'
  import {ERR_OK} from '../../api/config'
  import Singer from '../../common/js/singer'
  import Listview from '../../base/listview/listview'

  const HOT_NAME = '热门'
  const HOT_SINGER_LEN = 10

  export default {
    data () {
      return {
        singers: []
      }
    },
    created () {
      this._getSingerList()
    },
    components: {
      Listview
    },
    methods: {
      _getSingerList () {
        getSingerList().then((res) => {
          if (res.code === ERR_OK) {
            this.singers = this._normalizeSinger(res.data.list)
          }
        })
      },
      // 初始化歌手，将数据格式化成想要的样子
      _normalizeSinger (list) {
        // 定义需要的数据结构
        let map = {
          hot: {
            title: HOT_NAME,
            items: []
          }
        }
        list.forEach((item, index) => {
          if (index < HOT_SINGER_LEN) {
            // 将前10条数据push到hot对象中
            map.hot.items.push(new Singer({
              id: item.Fsinger_mid,
              name: item.Fsinger_name
            }))
            // 获取Findex(歌手的首字母缩写)
            const key = item.Findex
            // 如果没有key，动态创建
            if (!map[key]) {
              map[key] = {
                title: key,
                items: []
              }
            }
            // 将歌手的数据push到key对象中
            map[key].items.push(new Singer({
              id: item.Fsinger_mid,
              name: item.Fsinger_name
            }))
          }
        })
        // 处理map，得到有序列表
        let hot = []
        let ret = []
        // 将热门push到hot，剩下的push到ret
        for (var key in map) {
          let val = map[key]
          if (val.title.match(/[a-zA-Z]/)) {
            ret.push(val)
          } else if (val.title === HOT_NAME) {
            hot.push(val)
          }
        }
        // 对剩下的做排序，根据title的charCode做升序排列
        ret.sort((a, b) => {
          return a.title.charCodeAt(0) - b.title.charCodeAt(0)
        })
        // 字符串拼接（把ret拼接到hot后面）
        return hot.concat(ret)
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .singer
    position fixed
    width 100%
    top 84px
    bottom 0
</style>
