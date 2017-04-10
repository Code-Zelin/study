<template>
  <div>
    <div class="food_comment border-1px">
      <h2 class="food_title">商品评价</h2>
      <div class="comment_top border-1px">
        <ul class="border-1px comment_top_list">
          <li :class="rateTag.class" @click="setType(rateTag.type)" v-for="rateTag in rateTags">{{rateTag.text}}<span v-if="rateTag.type===2"> {{rateNum}}</span></li>
        </ul>
        <div class="select_wrapper clearfix">
          <span class="icon-check_circle app_left" :class="{not_select:!selected}" @click="select"></span>
          <p class="app_left">
            只看有内容的评论
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">

  export default {
    props: {
      ratings: {
        type: Array
      },
      rateNum: {
        type: Number,
        default: 0
      },
      rateType: {
        type: Number
      },
      rateTags: {
        type: Array
      }
    },
    data() {
      return {
        selected: true
      }
    },
    methods: {
      setType(type) {
        this.$emit('refresh', {type: type})
      },
      select() {
        this.selected = !this.selected
        this.$emit('refresh', {select: true})
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";

  .food_comment{
    border-1px(rgba(7,17,27,.1),true);
    background: #fff;
    padding: 18px;
    padding-bottom: 0;
    .comment_top{
      border-1px(rgba(7,17,27,.1))
      .comment_top_list{
        border-1px(rgba(7,17,27,.1));
        padding: 12px 0 18px;
        li{
          display: inline-block;
          padding: 8px 12px;
          font-size: 12px;
          line-height: 16px;
          border-radius: 3px;
          margin-right: 8px;
          &:last-child{
             margin-right: 0;
          }
          span{
            font-size: 8px;
          }
          &.comment_all{
             background: rgb(0,160,220);
          }
          &.comment_recommend{
             background: rgba(0,160,220,.2);
             color: rgb(77,85,93);
          }
          &.comment_bad{
             color: rgb(77,85,93);
             background: rgba(77,85,93,.2);
          }
        }
      }
      .select_wrapper{
        height: 24px;
        padding: 12px 0;
        font-size: 12px;
        line-height: 24px;
        color: rgb(147,153,159);
        span{
          margin-right: 4px;
          font-size: 24px;
          line-height: 24px;
          color: rgb(0,160,220);
          &.not_select{
             color: rgb(147, 153,159);
          }
        }
        p{
          display: inline-block;
        }
      }
    }
  }
</style>
