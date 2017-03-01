<template>
  <footer class="vue_footer">
    <div class="shop_car_box">
      <div class="shop_car icon-shopping_cart"></div>
    </div>
    <div class="shop_center">
      <div class="shop_total">
        ￥{{total}}
      </div><div class="shop_line"></div><div class="shop_postage">
        另需配送费￥{{seller.deliveryPrice}}元
      </div>
    </div>
    <div class="shop_start_price" v-if="total<seller.minPrice">
      ￥{{seller.minPrice}}起送
    </div>
    <div class="shop_start_price shop_ok" v-else>
      确认订单
    </div>
  </footer>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      seller: Object
    },
    data() {
      return {
        all: 40
      }
    },
    computed: {
      total() {
        let total = 0
        return total
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .vue_footer {
    height: 44px;
    background: #141d27;
    position: relative;
    display: flex;
    .shop_car_box{
      flex: 0 0 56px;
      width: 56px;
      height: 56px;
      position: relative;
      top: -12px;
      margin-left: 12px;
      padding: 6px;
      background: #141d27;
      box-sizing: border-box;
      border-radius: 50%;
      .shop_car {
        width: 44px;
        height: 44px;
        line-height: 44px;
        background: rgba(255,255,255,0.4);
        border-radius: 50%;
        text-align: center;
        font-size: 24px;
        color: rgba(255,255,255,0.4);
      }
    }
    .shop_center{
      flex:1;
      padding: 0 12px;
      box-sizing: border-box;
      &>div{
          display: inline-block;
          height: 44px;
          line-height: 44px;
      }
      .shop_total{
        font-size: 16px;
        font-weight:700;
        color: rgba(255,255,255,0.4);
      }
      .shop_line{
        width: 1px;
        height: 24px;
        vertical-align: text-top;
        background: rgba(255,255,255,.1);
        margin: 0 10px;
      }
      .shop_postage{
        font-size: 14px;
        color: rgba(255,255,255,0.4);
      }
    }
    .shop_start_price{
      flex: 0 0 105px;
      width: 105px;
      height: 44px;
      line-height: 44px;
      text-align: center;
      background: rgba(255,255,255,0.4);
      color: rgba(255,255,255,0.4);
      font-weight:700;
      &.shop_ok{
         background: #36a0dc;
         color: #fff;
       }
    }
  }
</style>
