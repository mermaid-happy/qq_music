<template>
  <div class="rank" ref="rank">
    <scroll class="toplist" :data="topList" ref="topList">
      <ul>
        <li class="item" v-for="item in topList" :key="item.key" @click="jump(item)">
          <div class="icon">
            <img src="" alt="" width="100" height="100" v-lazy="item.picUrl">
            <span class="listen_count">
              <i class="icon icon_listen">
              </i>
              {{(item.listenCount/10000).toFixed(1)+ '万'}}
            </span>
          </div>
          <ul class="songlist">
            <li class="topic_tit">
              {{item.topTitle}}
            </li>
            <li class="song" v-for="(song, index) in item.songList" :key="song.key">
              <span> {{index + 1}} </span>
              <span class="songname"> {{song.songname}}</span>
              <span> - {{song.singername}} </span>
            </li>
          </ul>
        </li>
      </ul>
      <div class="loading-container" v-show="!topList.length">
        <loading></loading>
      </div>
    </scroll>
    <router-view></router-view>
  </div>
</template>
<script>
  import { ERR_OK } from 'api/config'
  import { getTopList } from 'api/rank'
  import Scroll from 'base/scroll'
  import Loading from 'base/loading'
  import { playListMixin } from 'common/js/mixin'
  import { mapMutations } from 'vuex'
  export default {
    mixins: [playListMixin],
    data() {
      return {
        topList: []
      }
    },
    created() {
      this._getTopList()
    },
    components: {
      Scroll,
      Loading
    },
    methods: {
      handlePlayList(playlist) {
        const bottom = playlist.length > 0 ? '60px' : ''
        this.$refs.rank.style.bottom = bottom
        this.$refs.topList.refresh()
      },
      _getTopList() {
        getTopList().then(res => {
          if (res.code === ERR_OK) {
            this.topList = res.data.topList
          }
        })
      },
      jump(item) {
        this.$router.push({
          path: `/rank/${item.id}`
        })
        this.setTopList(item)
      },
      ...mapMutations({
        setTopList: 'SET_TOP_LIST'
      })
    }
  }
</script>
<style lang="stylus" scoped>
  @import "~common/stylus/variable"
  @import "~common/stylus/mixin"

   .rank
    position: fixed
    width: 100%
    top: 88px
    bottom: 0
    background-color :$color-background-r
    .toplist
      height: 100%
      overflow: hidden
      .item
        display: flex
        margin: 0 10px
        padding-top: 10px
        height: 100px
        &:last-child
          padding-bottom: 20px
        .icon
          position: relative
          flex: 0 0 100px
          width: 100px
          height: 100px
          .listen_count
            position absolute
            left: 5px
            bottom: 7px
            line-height: 12px
            color: #fff
            opacity: .6
            font-size: 9px
            z-index: 10
            .icon_listen
              background-image: url('./list_sprite.png')
              background-repeat: no-repeat
              background-size: 24px 60px
              z-index: 10
              display: block
              float: left
              width: 10px
              height: 10px
              background-position: 0 -50px
              margin-right: 3px
        .songlist
          flex: 1
          display: flex
          flex-direction: column
          justify-content: center
          padding: 0 20px
          height: 100px
          overflow: hidden
          background: $color-background
          color: $color-text-b
          font-size:$font-size-medium
          .topic_tit
            font-weight: normal
            margin-bottom: 5px
            font-size:$font-size-medium-x
          .song
            no-wrap()
            line-height: 22px
            color:rgba(0,0,0,.5)
            .songname
              color:$color-text-b
      .loading-container
        position: absolute
        width: 100%
        top: 50%
        transform: translateY(-50%)
</style>
