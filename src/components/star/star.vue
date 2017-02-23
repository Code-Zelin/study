<template>
  <div class="star_box">
    <span v-for="star in stars" :class="[starType,star]"></span>
  </div>
</template>

<script type="text/ecmascript-6">
  let STAR_NUM = 5
  let CLA_ON = 'on'
  let CLA_HALF = 'half'
  let CLA_OFF = 'off'

  export default {
    props: {
      size: Number,
      score: Number
    },
    computed: {
      starType() {
        return 'star_' + this.size
      },
      stars() {
        let STARS_LIST = []
        let score = Math.floor(this.score * 2) / 2
        let decimal = score % 1 === 0
        let integer = Math.floor(score)
        for (let s = 0; s < integer; s++) {
          STARS_LIST.push(CLA_ON)
        }
        if (!decimal) {
          STARS_LIST.push(CLA_HALF)
        }
        for (let s = STARS_LIST.length; s < STAR_NUM; s++) {
          STARS_LIST.push(CLA_OFF)
        }
        return STARS_LIST
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";

  .star_box{
    display: inline-block;
    vertical-align: text-bottom;;
    &>span{
        display: inline-block;
        bg_set(true);
        &.star_24{
           width: 20px;
           height:20px;
           margin-right: 20px;
           &.on{
            bg_image(star24_on);
           }
           &.half{
            bg_image(star24_half);
           }
           &.off{
            bg_image(star24_off);
           }
         }
        &.star_36{
           width: 30px;
           height: 30px;
           margin-right: 30px;
          &.on{
           bg_image(star36_on);
           }
          &.half{
           bg_image(star36_half);
           }
          &.off{
           bg_image(star36_off);
           }
        }
        &.star_48{
           width: 40px;
           height: 40px;
           margin-right: 40px;
          &.on{
           bg_image(star48_on);
           }
          &.half{
           bg_image(star48_half);
           }
          &.off{
           bg_image(star48_off);
           }
        }
        &:last-child{
           margin-right: 0;
         }
    }
  }
</style>
