<template>
  <div class="stars_box clearfix">
    <span v-for="star in stars" :class="[starType, star]">

    </span>
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
      starType () {
        return 'star_' + this.size
      },
      stars () {
        let STARS_LIST = []
        let score = this.score
        score = Math.floor(score * 2) / 2
        let decimal = score % 1 !== 0
        console.log(decimal)
        let integer = Math.floor(score)
        for (let s = 0; s < integer; s++) {
          STARS_LIST.push(CLA_ON)
        }
        if (decimal) {
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

  .stars_box{
    display: inline-block;
    vertical-align: text-bottom;
    &>span{
      display: inline-block;
      bg-set();
      &:last-child{
        margin-right: 0;
      }
      &.star_24{
        width: 20px;
        height: 20px;
        margin-right: 20px;
        &.on{
          bg-image(star24_on);
        }
        &.half{
         bg-image(star24_half);
         }
        &.off{
         bg-image(star24_off);
         }
      }
    }
  }
</style>
