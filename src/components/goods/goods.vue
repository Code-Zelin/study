<template>
  <div class="goods">
    <div class="goods_menu">
      <ul>
        <li v-for="(menu, index) in goods">
          <div class="menu_div border-1px">
            <tag :num="3" :type="menu.type" v-if="menu.type !== -1"></tag>{{menu.name}}
          </div>
        </li>
      </ul>
    </div>
    <div class="goods_list">
      <div class="goods_item" v-for="details in goods">
        <div class="goods_title">
          {{details.name}}
        </div>
        <div class="goods_detail" v-for="(detail,index) in details.foods" :class="{goods_detail_line: details.foods.length>1 && index!==details.foods.length-1}">
          <img :src="detail.icon" class="goods_img"/>
          <div class="goods_detail_box">
            <div class="goods_name">
              {{detail.name}}
            </div>
            <div class="goods_describe">
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
            <div class="goods_price">
              <span class="goods_price_new clearfix">
                <i class="app_left">￥</i>
                <b class="app_left">
                   {{detail.price}}
                </b>
              </span>
              <span class="goods_price_old clearfix" v-if="detail.oldPrice">
                <i class="app_left">￥</i>
                <b class="app_left">
                  {{detail.oldPrice}}
                </b>
              </span>
            </div>
          </div>
          <div class="goods_num">
            <i class="icon-remove-circle-outline"></i>
            <span>

            </span>
            <i class="icon-add-circle"></i>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import tag from '../tag/tag.vue'

  export default {
    data() {
      return {
        goods: []
      }
    },
    created() {
      this.$http.get('/api/goods').then(response => {
        let vueResponse = response.body
        if (vueResponse.errno === 0) {
          this.goods = vueResponse.data
          console.log(this.goods)
        }
      })
    },
    components: {
      tag
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";

  .goods {
    display: flex;
    .goods_menu {
      flex: 0 0 80px;
      width: 80px;
      overflow-y: auto;
      & > ul{
          background: #f3f5f7;
          & > li {
                display: table;
                width: 80px;
                height: 54px;
                padding: 0 12px;
                box-sizing: border-box;
                .menu_div{
                  display: table-cell;
                  vertical-align: middle;
                  font-size: 12px;
                  color: rgb(20, 20, 20);
                  font-weight: 200;
                  line-height: 14px;
                  border-1px(rgba(7,17,27,.1))
                  &.menu_cur{
                     background: #fff;
                     &:after{
                       display: none;
                     }
                   }
                  .sup_span{
                    margin-right: 4px;
                    vertical-align: text-bottom;
                  }
                }
                &:last-child{
                  .menu_div{
                    &:after{
                       display: none;
                    }
                  }
                 }
          }
      }
    }
    .goods_list{
      flex:100%;
      .goods_item{
        .goods_title{
          height: 26px;
          line-height: 26px;
          background: #f3f5f7;
          position: relative;
          padding-left: 14px;
          color:rgb(147,153,159);
          font-size:12px;
          font-weight:600;
          &:before{
             content: '';
             display: block;
             width: 4px;
             height:100%;
             background: #d9dde1;
             position: absolute;
             left:0;
             top:0;
           }
        }
        .goods_detail{
          padding: 18px 0;
          margin: 0 18px;
          &.goods_detail_line{
            border-1px(rgba(7,17,27,.1))
           }
          .goods_img{
            width: 55px;
            height: 55px;
            vertical-align: top;
          }
          .goods_detail_box{
            display: inline-block;
            font-size: 10px;
            color: rgb(147,153,159);
            line-height: 10px;
            .goods_name{
              font-size: 14px;
              color: rgb(7,17,27);
              line-height: 14px;
            }
            .goods_describe{
              margin: 8px 0;
            }
            .goods_sale{
              margin-bottom: 8px;
              .goods_sale_left{
                margin-right: 12px;
              }
            }
            .goods_price{
              line-height: 24px;
              i{
                font-style: normal;
                font-size: 10px !important;
                font-weight: normal !important;
                margin:0;
              }
              b{
                font-weight: 700;
              }
              &>span{
                  display: inline-block;
                  vertical-align: top;
                  line-height: 24px;
                }
              .goods_price_new{
                color: rgb(240,20,20);
                b{
                  font-size: 16px;
                }
              }
              .goods_price_old{
                b{
                  font-size: 10px;
                  text-decoration: line-through;
                }
                i{
                  text-decoration: line-through;
                }
              }
            }
          }
        }
      }
    }
  }
</style>
