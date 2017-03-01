<template>
  <footer class="vue_footer">
    <div class="shop_car_box">
      <div class="shop_car icon-shopping_cart" :class="{shop_car_blue:num>0}">
        <span class="shop_num" v-show="num>0">
          {{num}}
        </span>
      </div>
    </div>
    <div class="shop_center clearfix">
      <div class="shop_total">
        ¥{{total}}
      </div><div class="shop_line"></div><div class="shop_postage">
        另需配送费¥{{seller.deliveryPrice}}元
      </div>
    </div>
    <div class="shop_start_price shop_ok" v-if="total>=seller.minPrice">
      去结算
    </div>
    <div class="shop_start_price" v-else>
      <span v-if="total===0">
        ¥{{seller.minPrice}}起送
      </span>
      <span v-else>
        还差¥{{diffPrice}}起送
      </span>
    </div>
  </footer>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      seller: {
        type: Object
      },
      selectGoods: {
        type: Array,
        default() {
          return [
            {
              price: 10,
              num: 0
            },
            {
              price: 5,
              num: 1
            }
          ]
        }
      }
    },
    data() {
      return {
        all: 40
      }
    },
    computed: {
      total() {
        let total = 0
        this.selectGoods.forEach((element, index) => {
          total += element.price * element.num
        })
        return total
      },
      num() {
        let num = 0
        this.selectGoods.forEach((element, index) => {
          num += element.num
        })
        return num
      },
      diffPrice() {
        let diff = this.seller.minPrice - this.total
        return diff
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .vue_footer {
    width: 100%;
    height: 48px;
    background: #141d27;
    position: fixed;
    bottom: 0;
    display: flex;
    .shop_car_box{
      flex: 0 0 56px;
      width: 58px;
      height: 58px;
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
        &.shop_car_blue{
           background: #00a0dc;
           color: #fff;
        }
        .shop_num{
          position: absolute;
          top: 0;
          left: 33px;
          height: 16px;
          line-height: 16px;
          background: #f01414;
          color: #fff;
          border-radius: 8px;
          font-size: 8px;
          padding: 0 8px;
        }
      }
    }
    .shop_center{
      flex:1;
      padding: 0 12px;
      box-sizing: border-box;
      &>div{
          float: left;
          height: 48px;
          line-height: 48px;
      }
      .shop_total{
        font-size: 16px;
        font-weight:700;
        color: rgba(255,255,255,0.4);
      }
      .shop_line{
        width: 1px;
        height: 24px;
        background: rgba(255,255,255,.1);
        margin: 12px 10px 0;
      }
      .shop_postage{
        font-size: 10px;
        color: rgba(255,255,255,0.4);
      }
    }
    .shop_start_price{
      flex: 0 0 105px;
      width: 105px;
      height: 48px;
      line-height: 48px;
      text-align: center;
      background: #2b333b;
      font-size: 12px;
      font-weight:700;
      span{
        color: rgba(255,255,255,0.4);
        font-weight:700;
      }
      &.shop_ok{
         background: #00b43c;
         color: #fff;
       }
    }
  }
</style>
