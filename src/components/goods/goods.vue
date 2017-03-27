<template>
  <div class="goods">
    <div class="goods_menu" id="menu-wrapper">
      <ul>
        <li v-for="(menu, index) in goods" :class="{menu_cur:index==currentIndex}" @click="selectMenu(index,$event)">
          <div class="menu_div border-1px">
            <tag :num="3" :type="menu.type" v-if="menu.type !== -1"></tag>{{menu.name}}
          </div>
        </li>
      </ul>
    </div>
    <div class="goods_list" id="goods-wrapper">
      <ul>
        <li class="goods_item" v-for="details in goods">
          <div class="goods_title">
            {{details.name}}
          </div>
          <div class="goods_detail" v-for="(detail,index) in details.foods" :class="{goods_detail_line: details.foods.length>1 && index!==details.foods.length-1}">
            <img :src="detail.icon" class="goods_img app_left" @click="_food(detail)"/>
            <div class="goods_detail_box app_left">
              <div class="goods_name" @click="_food(detail)">
                {{detail.name}}
              </div>
              <div class="goods_describe" v-if="detail.description">
                {{detail.description}}
              </div>
              <div class="goods_sale">
                <span class="goods_sale_left">
                  月售{{detail.sellCount}}份
                </span>
                <span>
                  好评率{{detail.rating}}%
                </span>
              </div>
              <div class="goods_price clearfix">
                <span class="goods_price_new clearfix">
                  <i class="app_left">¥</i>
                  <b class="app_left">
                     {{detail.price}}
                  </b>
                </span>
                <span class="goods_price_old clearfix" v-if="detail.oldPrice">
                  <i class="app_left">¥</i>
                  <b class="app_left">
                    {{detail.oldPrice}}
                  </b>
                </span>
                <div class="cartcontrol_wrapper app_right">
                  <cartcontrol :food="detail" @add="addFood"></cartcontrol>
                </div>
              </div>
            </div>
          </div>
        </li>
      </ul>
    </div>
    <vfooter ref="shopcart"  :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice" :selectFoods="selectGoods"></vfooter>
    <foodWrapper :food="food" v-show="showFood" ref="foodWrapper" @hide="hideFood"></foodWrapper>
  </div>
</template>

<script type="text/ecmascript-6">
  import tag from 'components/tag/tag'
  import vueFooter from 'components/footer/footer'
  import cartcontrol from 'components/cartcontrol/cartcontrol'
  import BScroll from 'better-scroll'
  import food from 'components/food/food'

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0,
        food: {},
        showFood: false
      }
    },
    created() {
      this.$http.get('/api/goods').then(response => {
        let vueResponse = response.body
        if (vueResponse.errno === 0) {
          this.goods = vueResponse.data
          this.$nextTick(() => {
            this._initScroll()
            this.goodsClientHeight()
          })
        }
      })
    },
    components: {
      vfooter: vueFooter,
      tag: tag,
      cartcontrol: cartcontrol,
      foodWrapper: food
    },
    computed: {
      currentIndex() {
        // 添加联动事件
        let gst = this.listHeight
        for (var gt = 0; gt < gst.length; gt++) {
          let height1 = gst[gt]
          let height2 = gst[gt + 1]
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return gt
          }
        }
      },
      selectGoods() {
        let Goods = []
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              if (food.count > 0) {
                Goods.push(food)
              }
            }
          })
        })
        return Goods
      }
    },
    methods: {
      _initScroll() {
        // 初始化菜单和商品列表
        this.menuScroll = new BScroll(document.getElementById('menu-wrapper'), {
          click: true
        })
        this.goodsScroll = new BScroll(document.getElementById('goods-wrapper'), {
          click: true,
          probeType: 3
        })

        // 添加滚动事件
        this.goodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y))
        })
      },
      goodsClientHeight() {
        // 初始化goodsScrollTop数组
        this.listHeight.push(0)
        let goodsClientHeight = 0
        for (let i = 0; i < document.getElementsByClassName('goods_item').length; i++) {
          goodsClientHeight += document.getElementsByClassName('goods_item')[i].clientHeight
          this.listHeight.push(goodsClientHeight)
        }
      },
      selectMenu(index, event) {
        if (!event._constructed) {
          return
        }
        // 点击菜单事件
        let posY = this.listHeight[index]
        console.log(posY)
        this.goodsScroll.scrollTo(0, -posY, 500)
      },
      addFood(target) {
        this._drop(target)
      },
      _drop(target) {
        this.$refs.shopcart.drop(target)
      },
      _food(element) {
        this.food = element
        this.showFood = true
      },
      hideFood(target) {
        this.showFood = false
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";

  .goods
    display: flex
    width: 100%
    height: calc(100% - 174px - 44px)
    &>div
      height: 100%
      overflow: hidden
    .goods_menu
      flex: 0 0 80px
      width: 80px
      background: #f3f5f7
      & > ul
        & > li
          display: table
          width: 80px
          height: 54px
          padding: 0 12px
          box-sizing: border-box
          .menu_div
            display: table-cell
            vertical-align: middle
            font-size: 12px
            color: rgb(20, 20, 20)
            font-weight: 200
            line-height: 14px
            border-1px(rgba(7,17,27,.1))
            .sup_span
              margin-right: 4px
              vertical-align: text-bottom
          &.menu_cur
             margin-top: -1px
             background: #fff
             position: relative
             z-index: 2
             .menu_div
               &:after
                  display: none
            &:last-child
              .menu_div
                &:after
                   display: none
    .goods_list
      flex: 1
      width: calc(100% - 80px)
      .goods_item
        .goods_title
          height: 26px
          line-height: 26px
          background: #f3f5f7
          position: relative
          padding-left: 14px
          color:rgb(147,153,159)
          font-size:12px
          font-weight:600
          &:before
             content: ''
             display: block
             width: 4px
             height:100%
             background: #d9dde1
             position: absolute
             left:0
             top:0
        .goods_detail
          padding: 18px 0
          margin: 0 18px
          overflow: hidden
          &.goods_detail_line
            border-1px(rgba(7,17,27,.1))
          .goods_img
            width: 55px
            height: 55px
            vertical-align: top
            margin-right: 5px
          .goods_detail_box
            display: inline-block
            font-size: 10px
            color: rgb(147,153,159)
            line-height: 10px
            width: calc(100% - 60px)
            &>div
                line-height: 100%
                margin-bottom: 8px
                &:last-child
                  margin-bottom: 0
            .goods_name
              font-size: 14px
              color: rgb(7,17,27)
            .goods_describe
              font-size: 10px
              white-space: nowrap
              text-overflow: ellipsis
              overflow: hidden
            .goods_sale
              .goods_sale_left
                margin-right: 12px
            .goods_price
              line-height: 24px
              i
                font-style: normal
                font-size: 10px !important
                font-weight: normal !important
                margin:0
              b
                font-weight: 700
              &>span
                  display: inline-block
                  vertical-align: top
                  line-height: 24px
              .goods_price_new
                color: rgb(240,20,20)
                b
                  font-size: 16px
              .goods_price_old
                b
                  font-size: 10px
                  text-decoration: line-through
                i
                  text-decoration: line-through
              .cartcontrol_wrapper
                width: 72px
</style>
