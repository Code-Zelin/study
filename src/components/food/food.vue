<template>
  <div class="food_wrapper" ref="foodWrapper">
    <div class="back icon-arrow_lift" @click="_hide"></div>
    <img :src="food.image">
    <div class="food_top border-1px">
      <div class="goods_detail_box">
        <div class="goods_name">
          {{food.name}}
        </div>
        <div class="goods_sale">
          <span class="goods_sale_left">
            月售{{food.sellCount}}份
          </span>
          <span>
            好评率{{food.rating}}%
          </span>
        </div>
        <div class="goods_price clearfix">
          <span class="goods_price_new app_left">
            <i class="app_left">¥</i>
            <b class="app_left">
              {{food.price}}
            </b>
          </span>
          <span class="goods_price_old app_left" v-if="food.oldPrice">
            <i class="app_left">¥</i>
            <b class="app_left">
              {{food.oldPrice}}
            </b>
          </span>
          <div class="cartcontrol_wrapper app_right">
            <cartcontrol :food="food" :isDetail="isDetail"></cartcontrol>
          </div>
        </div>
      </div>
    </div>
    <div class="food_center border-1px">
      <h2 class="food_title">商品介绍</h2>
      <p>
        {{food.info}}
      </p>
    </div>
    <div class="food_comment border-1px">
      <h2 class="food_title">商品评价</h2>
      <div class="comment_top border-1px">
        <ul class="border-1px comment_top_list">
          <li class="comment_all" @click="rateType=2">全部 <span>{{food.rating}}</span></li><li class="comment_recommend" @click="rateType=0">推荐</li><li class="comment_bad" @click="rateType=1">吐槽</li>
        </ul>
        <div class="select_wrapper clearfix">
          <span class="icon-check_circle app_left" :class="{not_select:!selected}" @click="select"></span>
          <p class="app_left">
            只看有内容的评论
          </p>
        </div>
      </div>
    </div>
    <ul class="comment_list">
      <li v-for="rating in food.ratings" :class="{none:(showHaveComment && !rating.text)}" v-if="rating.rateType==rateType || rateType==2">
        <div class="comment_msg clearfix">
              <span class="app_left">
                {{rating.rateTime}}
              </span>
          <img :src="rating.avatar" class="app_right">
              <span class="app_right">
                {{rating.username}}
              </span>
        </div>
        <div class="comment_content">
          <span class="icon-thumb_up" v-if="rating.rateType===0"></span><span class="icon-thumb_down" v-else-if="rating.rateType===1"></span>{{rating.text}}
        </div>
      </li>
    </ul>
  </div>
</template>

<script type="text/ecmascript-6">
  import vue from 'vue'
  import cartcontrol from 'components/cartcontrol/cartcontrol'
  import BScroll from 'better-scroll'

  export default {
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        isDetail: true,
        rateType: 2,
        selected: false,
        showHaveComment: false
      }
    },
    created() {
      this.$nextTick(() => {
        if (!this.foodScroll) {
          // 购物车列表初始化
          this.foodScroll = new BScroll(this.$refs.foodWrapper, {
            click: true
          })
        } else {
          this.foodScroll.refresh()
        }
      })
    },
    computed: {
    },
    methods: {
      _hide() {
        this.$emit('hide', '')
      },
      _addShopcart() {
        vue.set(this.food, 'count', 1)
      },
      select() {
        this.selected = !this.selected
        this.showHaveComment = !this.showHaveComment
      }
    },
    components: {
      cartcontrol
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";

  div.food_wrapper{
    position: fixed;
    top: 0;
    left: 0;
    z-index: 9;
    width: 100%;
    height: calc( 100% - 48px ) !important;
    overflow: auto;
    background: #f3f5f7;
    .back{
      position: fixed;
      top: 18px;
      left: 18px;
      width: 40px;
      height: 40px;
      line-height: 40px;
      text-align: center;
      border-radius: 50%;
      background: rgba(7,17,27,.5);
      color: #fff;
    }
    img{
      width: 100%;
    }
    .food_top{
      padding: 18px;
      background: #fff;
      border-1px(rgba(7,17,27,0.1));
      .goods_detail_box{
        display: inline-block;
        font-size: 10px;
        color: rgb(147,153,159);
        line-height: 10px;
        width: 100%;
        &>div{
          line-height: 100%;
          margin-bottom: 8px;
          &:last-child{
            margin-bottom: 0;
          }
        }
        .goods_name{
          font-size: 14px;
          white-space: nowrap;
          text-overflow: ellipsis;
          overflow: hidden;
          color: rgb(7, 17, 27);
          font-weight: 700;
        }
        .goods_sale {
          .goods_sale_left{
            margin-right: 12px;
          }
        }
        .goods_price{
          line-height: 24px;
          i {
            font-style: normal;
            font-size: 10px !important;
            font-weight: normal !important;
            margin: 0;
          }
          b{
            font-weight: 700;
            &>span {
              display: inline-block;
              vertical-align: top;
              line-height: 24px;
            }
          }
          .goods_price_new {
            color: rgb(240, 20, 20);
            b {
              font-size: 16px;
            }
          }
          .goods_price_old{
            margin-left: 12px;
            b {
              font-size: 10px;
              text-decoration: line-through;
            }
            i {
              text-decoration: line-through;
            }
          }
          .cartcontrol_wrapper {
            width: 72px;
          }
        }
      }
    }
    .food_title{
      font-size: 14px;
      font-weight:700;
      margin-bottom: 6px;
    }
    .food_center{
      border-1px(rgba(7,17,27,0.1), true)
      padding: 18px;
      background: #fff;
      margin: 16px 0;
      p{
        font-size: 12px;
        padding: 8px;
        line-height: 24px;
        color: rgb(77,85,93);
        text-indent: 2em;
      }
    }
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
    .comment_list{
      padding: 0 18px;
      background: #fff;
      li{
        border-1px(rgba(7,17,27,.1));
        padding: 16px 0;
        &.none{
           display: none;
         }
        .comment_msg{
          color: rgb(147,153,159);
          font-size: 10px;
          line-height: 12px;
          margin-bottom: 6px;
          img{
            width: 12px;
            height: 12px;
            border-radius: 50%;
            margin-left: 6px;
          }
        }
        .comment_content{
          font-size: 12px;
          color: rgb(7,17,27);
          line-height: 16px;
          span{
            vertical-align: text-bottom;
            margin-right: 4px;
            &.icon-thumb_up{
              color: rgb(0,160,220);
            }
            &.icon-thumb_down{
              color: rgb(147,153,159);
            }
          }
        }
      }
    }
  }
</style>
