<template>
  <div class="food_wrapper">
    <div class="back icon-arrow_lift" @click="_hide"></div>
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
            <cartcontrol :food="food" :isDetail="isDetail"></cartcontrol>
          </div>
        </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import vue from 'vue'
  import cartcontrol from 'components/cartcontrol/cartcontrol'

  export default {
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        isDetail: true
      }
    },
    created() {
    },
    computed: {
    },
    methods: {
      _hide() {
        this.$emit('hide', '')
      },
      _addShopcart() {
        vue.set(this.food, 'count', 1)
      }
    },
    components: {
      cartcontrol
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";

  .food_wrapper{
    position: fixed;
    top: 0;
    left: 0;
    z-index: 9;
    width: 100%;
    height: calc( 100% - 48px ) !important;
    background: #fff;
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
  }
</style>
