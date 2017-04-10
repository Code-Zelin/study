<template>
  <transition name="foodWrapper">
    <div class="food_wrapper" v-show="showFlag" ref="foodWrapper">
      <div>
        <img :src="food.image">
        <div class="food_top">
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
                <cartcontrol :food="food" @add="addFood" :isDetail="isDetail"></cartcontrol>
              </div>
            </div>
          </div>
        </div>
        <gap></gap>
        <div v-if="food.info">
          <div class="food_center border-1px">
            <h2 class="food_title">商品介绍</h2>
            <p>
              {{food.info}}
            </p>
          </div>
          <gap></gap>
        </div>
        <rateList :rateNum="food.rating" :rateType="rateType" :rateTags="rateTags" @refresh="refreshScroll"></rateList>
        <ul class="comment_list">
          <li v-for="rating in food.ratings" :class="{none:(showHaveComment && !rating.text)}" v-if="rating.rateType==rateType || rateType==2">
            <div class="comment_msg clearfix">
          <span class="app_left">
            {{rating.rateTime | formatDate}}
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
      <div class="back icon-arrow_lift" @click="_hide"></div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  import cartcontrol from 'components/cartcontrol/cartcontrol'
  import rateList from 'components/rateList/rateList'
  import gap from 'components/gap/gap'
  import BScroll from 'better-scroll'
  import {formatDate} from 'common/js/date'

  export default {
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        isDetail: true,
        showFlag: false,
        rateType: 2,
        showHaveComment: true,
        rateTags: [
          {
            text: '全部',
            type: 2,
            class: 'comment_all'
          },
          {
            text: '推荐',
            type: 0,
            class: 'comment_recommend'
          },
          {
            text: '吐槽',
            type: 1,
            class: 'comment_bad'
          }
        ]
      }
    },
    created() {
    },
    computed: {
    },
    methods: {
      show() {
        this.showFlag = true
        this.$nextTick(() => {
          if (!this.foodScroll) {
            // 购物车列表初始化
            this.foodScroll = new BScroll(this.$refs.foodWrapper, {
              click: true
            })
          } else {
            this.foodScroll.scrollTo(0, 0)
            this.foodScroll.refresh()
          }
        })
      },
      refreshScroll(event) {
        if (event.select) {
          this.showHaveComment = !this.showHaveComment
        }
        if ((typeof event.type) === 'number') {
          this.rateType = event.type
        }
        this.$nextTick(() => {
          this.foodScroll.refresh()
        })
      },
      addFood(target) {
        this.$emit('add', target)
      },
      _hide() {
        this.showFlag = false
      }
    },
    components: {
      cartcontrol,
      rateList,
      gap
    },
    filters: {
      formatDate(time) {
        let date = new Date(time)
        return formatDate(date, 'yyyy-MM-dd hh:mm')
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";

  .foodWrapper-enter-active {
    animation: fade-in .5s;
  }
  .foodWrapper-leave-active {
    animation: fade-out .5s;
  }
  @keyframes fade-in {
    0% {
      opacity: 0;
      transform: translateX(100%);
    }
    100% {
      opacity: 1;
      transform: translateX(0);
    }
  }
  @keyframes fade-out {
    0% {
      opacity: 1;
      transform: translateX(0);
    }
    100% {
      opacity: 0;
      transform: translateX(100%);
    }
  }
  div.food_wrapper{
    position: fixed;
    top: 0;
    left: 0;
    z-index: 9;
    width: 100%;
    height: calc( 100% - 48px ) !important;
    overflow: hidden;
    background: #f3f5f7;
    .back{
      position: fixed;
      top: 10px;
      padding:10px;
      font-size: 20px;
      text-align: center;
      border-radius: 50%;
      color: #fff;
    }
    img{
      width: 100%;
    }
    .food_top{
      padding: 18px;
      background: #fff;
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
      border-1px(rgba(7,17,27,0.1))
      padding: 18px;
      background: #fff;
      p{
        font-size: 12px;
        padding: 8px;
        line-height: 24px;
        color: rgb(77,85,93);
        text-indent: 2em;
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
