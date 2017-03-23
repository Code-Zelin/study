<template>
  <footer class="vue_footer">
    <div class="shop_car_wrapper">
      <div class="shop_car_box" @click="shopClick">
        <div class="shop_car icon-shopping_cart" :class="{shop_car_blue:num>0}">
        <span class="shop_num" v-show="num>0">
          {{num}}
        </span>
        </div>
      </div>
      <div class="shop_center clearfix">
        <div class="shop_total" :class="{ shop_total_white:total>0 }">
          ¥{{total}}
        </div><div class="shop_line"></div><div class="shop_postage">
          另需配送费¥{{deliveryPrice}}元
        </div>
      </div>
      <div class="shop_start_price" :class="startPrice.class">
        {{startPrice.html}}
      </div>
    </div>
    <div class="ball_wrapper">
      <transition name="drop" @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter" v-for="ball in balls">
        <div class="ball" v-show="ball.show">
          <div class="inner inner-hook"></div>
        </div>
      </transition>
    </div>
    <div class="footer_mask" v-show="listShow" @click="hide"></div>
    <transition name="shop_show">
      <div class="footer_list_wrapper" v-show="listShow">
        <h2 class="clearfix footer_list_title">
          购物车
          <span class="footer_close app_right" @click="closeList">
            清空
          </span>
        </h2>
        <div class="footer_list" ref="footerList">
          <ul>
            <li v-for="good in selectFoods" class="border-1px" v-if="good.count>0">
              <span class="f_l_goods_name">
                {{good.name}}
              </span>
              <span class="f_l_goods_price">
                ¥<span>{{good.price}}</span>
              </span>
              <div class="cartcontrol_wrapper">
                <cartcontrol :food="good" @add="addFood"></cartcontrol>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </transition>
  </footer>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import cartcontrol from 'components/cartcontrol/cartcontrol'

  export default {
    props: {
      selectFoods: {
        type: Array,
        default() {
          return []
        }
      },
      deliveryPrice: {
        type: Number,
        default() {
          return 0
        }
      },
      minPrice: {
        type: Number,
        default() {
          return 0
        }
      }
    },
    data() {
      return {
        maskIsShow: true,
        balls: [
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          }
        ],
        dropBalls: []
      }
    },
    components: {
      cartcontrol
    },
    computed: {
      // 商品总价
      total() {
        let total = 0
        this.selectFoods.forEach((element, index) => {
          total += element.price * element.count
        })
        return total
      },
      // 商品总数
      num() {
        let num = 0
        this.selectFoods.forEach((element, index) => {
          num += element.count
        })
        return num
      },
      // 起送价格
      startPrice() {
        let startPrice = {}
        if (this.total >= this.minPrice) {
          startPrice.html = '去结算'
          startPrice.class = 'shop_ok'
        } else {
          if (this.total === 0) {
            startPrice.html = '¥' + this.minPrice + '起送'
          } else {
            startPrice.html = '还差¥' + (this.minPrice - this.total) + '起送'
          }
        }
        return startPrice
      },
      // 购物车是否显示
      listShow() {
        if (this.selectFoods.length === 0) {
          this.maskIsShow = true
          return false
        }
        let show = !this.maskIsShow
        if (show) {
          this.$nextTick(() => {
            if (!this.scroll) {
              // 购物车列表初始化
              this.scroll = new BScroll(this.$refs.footerList, {
                click: true
              })
            } else {
              this.scroll.refresh()
            }
          })
        }
        return show
      }
    },
    methods: {
      // 下落
      drop(el) {
        for (let x = 0; x < this.balls.length; x++) {
          let ball = this.balls[x]
          if (!ball.show) {
            ball.show = true
            ball.el = el
            this.dropBalls.push(ball)
            return
          }
        }
      },
      shopClick() {
        if (this.selectFoods.length === 0) {
          return
        }
        this.maskIsShow = !this.maskIsShow
      },
      hide() {
        this.maskIsShow = true
      },
      // 清空数组
      closeList() {
        this.selectFoods.forEach((food) => {
          food.count = 0
        })
      },
      addFood(target) {
        this.drop(target)
      },
      // 过渡前
      beforeEnter: function (el) {
        let count = this.balls.length
        while (count--) {
          let ball = this.balls[count]
          if (ball.show) {
            let rect = ball.el.getBoundingClientRect()
            let x = rect.left - 32
            let y = -(window.innerHeight - rect.top - 22)
            el.style.display = 'block'
            el.style.webkitTransform = `translate3d(0, ${y}px, 0)`
            el.style.transform = `translate3d(0, ${y}px, 0)`
            let inner = el.querySelector('.inner-hook')
            inner.style.webkitTransform = `translate3d(${x}px, 0, 0)`
            inner.style.transform = `translate3d(${x}px, 0, 0)`
          }
        }
      },
      // 过度完成
      enter: function (el, done) {
        /* eslint-disable no-unused-vars */
        let rf = el.offsetHeight
        this.$nextTick(() => {
          el.style.webkitTransform = 'translate3d(0, 0, 0)'
          el.style.transform = 'translate3d(0, 0, 0)'
          let inner = el.querySelector('.inner-hook')
          inner.style.webkitTransform = 'translate3d(0, 0, 0)'
          inner.style.transform = 'translate3d(0, 0, 0)'
          el.addEventListener('transitionend', done)
        })
      },
      // 过度之后
      afterEnter: function (el) {
        let ball = this.dropBalls.shift()
        if (ball) {
          ball.show = false
          el.style.display = 'none'
        }
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";

  .vue_footer {
    width: 100%;
    height: 48px;
    position: fixed;
    bottom: 0;
    .shop_car_wrapper{
      width:100%;
      display: flex;
      position: relative;
      background: #141d27;
      z-index:1;
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
            font-size: 9px;
            padding: 0 6px;
            box-shadow: 0 4px 8px 0 rgba(0,0,0,.4);
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
      &.shop_total_white{
         color: #fff;;
       }
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
        color: rgba(255,255,255,0.4);
        font-weight:700;
        &.shop_ok{
           background: #00b43c;
           color: #fff;
         }
        }
      }
    .footer_mask{
       position: fixed;
       z-index:-2;
       width: 100%;
       height:100%;
       top:0;
       left:0;
       background: rgba(7,17,27,0.8);
       filter: blur(10px);
    }
    .footer_list_wrapper{
      position: absolute;
      z-index:-2;
      top: 0;
      left:0;
      transform: translateY(-100%);
      width: 100%;
      max-height: 230px;
      min-height: 90px;
      background: #fff;
      &.shop_show-enter-active, &.shop_show-leave-active {
        transition: all 0.5s
      }
      &.shop_show-enter, &.shop_show-leave-active{
        transform: translate3d(0, 0, 0)
      }
      .footer_list_title{
        height: 40px;
        width: 100%;
        background: #f3f5f7;
        padding:0 18px;
        color: rgb(7,17,27);
        line-height: 40px;
        font-weight: 200;
        box-sizing: border-box;
        .footer_close{
          color: rgb(0,160,220);
          font-size: 12px;
        }
      }
      .footer_list{
        overflow: hidden;
        max-height: 190px;
        ul{
          padding: 0 18px 18px;
          li{
            display: flex;
            height: 48px;
            border-1px(rgba(7,17,27,0.1));
            padding: 12px 0;
            box-sizing: border-box;
            .f_l_goods_name{
              display: inline-block;
              flex: 1;
              font-size: 14px;
              color: rgb(7,17,27);
              line-height: 24px;
            }
            .f_l_goods_price{
              display: inline-block;
              flex: 0 0 30px;
              text-align: center;
              font-size: 10px;
              color: rgb(240,20,20);
              line-height: 24px;
              margin: 0 12px 0 18px;
              span{
                font-size: 14px;
                font-weight:700;
              }
            }
            .cartcontrol_wrapper {
              flex: 0 0 72px;
            }
          }
        }
      }
    }
    .ball_wrapper{
      .ball{
        position: fixed;
        left: 32px;
        bottom: 22px;
        z-index: 99;
        transition: all 0.6s cubic-bezier(0,-0.47,.96,-0.3);
        .inner{
          width: 16px;
          height: 16px;
          border-radius: 50%;
          background: rgb(0,160,220);
          transition: all 0.6s linear;
        }
      }
    }
  }
</style>
