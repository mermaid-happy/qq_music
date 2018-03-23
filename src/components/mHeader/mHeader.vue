<template>
  <div class="mod_header">
    <div class="music_logo"></div>
    <router-link tag="div" class="mine" to="/user">
      <i class="icon-mine"></i>
    </router-link>
  </div>
</template>

<script>
  export default {

  }
</script>

<style scoped lang="stylus">
  @import '~common/stylus/variable'
  @import '~common/stylus/mixin'

  .mod_header
    position: relative
    height: 44px
    text-align: left
    color: $color-theme
    background-color :$header-color-background
    font-size: 0
    .music_logo
      float: left;
      display: block;
      width: 90px;
      height: 25px;
      margin: 9px 0 0 10px;
      background :url("logo.png")
      background-size: cover;
      background-repeat: no-repeat;
      text-indent: -9999px;
    .mine
      position: absolute
      top: 0
      right: 0
      .icon-mine
        display: block
        padding: 12px
        font-size: 20px
        color: $color-theme
</style>
