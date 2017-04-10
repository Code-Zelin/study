<template>
  <div class="rateWrapper" ref="rateWrapper">
    <div class="ratings">
      <div class="ratings_top">
        <div class="ratings_left">
          <h2>
            {{seller.score}}
          </h2>
          <p>综合评分</p>
          <p>高于周边商家{{seller.rankRate}}%</p>
        </div>
        <div class="ratings_right">
          <div class="score">
            服务态度<star :size="24" :score="seller.serviceScore"></star><span>{{seller.serviceScore}}</span>
          </div>
          <div class="score">
            食物评价<star :size="24" :score="seller.foodScore"></star><span>{{seller.foodScore}}</span>
          </div>
          <div class="delivery_time">
            送达时间<span>{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <gap></gap>
      <rateList :ratings="ratings" :rateNum="seller.ratingCount" :rateTags="rateTags" @refresh="refreshScroll"></rateList>
      <ul class="ratings_list">
        <li class="border-1px" v-for="rating in ratings">
          <div class="ratings_list_head app_left">
            <img :src="rating.avatar">
          </div>
          <div class="ratings_list_right">
            <p class="clearfix">
              <span class="ratings_name app_left">{{rating.username}}</span>
              <span class="ratings_time app_right">{{rating.rateTime | formatDate}}</span>
            </p>
            <p>
              <star :size="24" :score="rating.score"></star><span v-if="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
            </p>
            <p class="ratings_content">{{rating.text}}</p>
            <div class="ratings_tags">
              <b class="icon-thumb_up" v-if="rating.rateType===0"></b><b class="icon-thumb_down" v-else-if="rating.rateType===1"></b><span class="ratings_tag" v-for="tag in rating.recommend">{{tag}}</span>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from 'components/star/star'
  import gap from 'components/gap/gap'
  import rateList from 'components/rateList/rateList'
  import BScroll from 'better-scroll'
  import {formatDate} from 'common/js/date'

  const errOk = 0

  export default {
    data() {
      return {
        ratings: [],
        seller: {},
        showHaveComment: true,
        rateType: 2,
        rateTags: [
          {
            text: '全部',
            type: 2,
            class: 'comment_all'
          },
          {
            text: '满意',
            type: 0,
            class: 'comment_recommend'
          },
          {
            text: '不满意',
            type: 1,
            class: 'comment_bad'
          }
        ]
      }
    },
    created() {
      this.$http.get('/api/ratings').then(res => {
        let vueResponse = res.body
        if (vueResponse.errno === errOk) {
          this.ratings = vueResponse.data
        }
      })
      this.$http.get('/api/seller').then(res => {
        let vueResponse = res.body
        if (vueResponse.errno === errOk) {
          this.seller = vueResponse.data
        }
      })
      this.$nextTick(() => {
        this.initScorll()
      })
    },
    methods: {
      initScorll() {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.rateWrapper, {
            click: true
          })
        } else {
          this.scroll.refresh()
        }
      },
      refreshScroll(event) {
        if (event.select) {
          this.showHaveComment = !this.showHaveComment
        }
        if ((typeof event.type) === 'number') {
          this.rateType = event.type
        }
        this.$nextTick(() => {
          this.scroll.refresh()
        })
      }
    },
    filters: {
      formatDate(time) {
        let date = new Date(time)
        return formatDate(date, 'yyyy-MM-dd hh:mm')
      }
    },
    components: {
      star,
      gap,
      rateList
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";
  .rateWrapper{
    height: calc(100% - 174px - 44px);
    overflow: hidden;
    .ratings{
      background: #f3f5f7;
      .ratings_top{
        display: flex;
        padding: 18px 0;
        background: #fff;
        .ratings_left{
          flex: 11;
          text-align: center;
          h2{
            font-size: 24px;
            color: rgb(255,153,0);
            line-height: 28px;
            margin-bottom: 6px;
          }
          p{
            font-size: 12px;
            color: rgb(7,17,27);
            line-height: 12px;
            margin-bottom: 8px;
          }
          p:last-child{
            font-size: 10px;
            color: rgb(147,153,159);
            line-height: 10px;
            margin-bottom: 0;
          }
        }
        .ratings_right{
          flex: 19;
          border-left: 1px solid rgba(7,17,27,.1);
          padding: 0 24px;
          &>div{
            font-size: 12px;
            line-height: 18px;
          }
          .score{
            margin-bottom: 8px;
            &>*{
              margin-left: 12px;
            }
            &>span{
              color: rgb(255,153,0);
            }
            .star_box{
              height: 18px;
              span{
                width: 18px;
                height:18px;
                margin-right: 6px;
                &:last-child{
                  margin-right: 0;
                }
              }
            }
          }
          .delivery_time{
            span{
              color: rgb(147,153,159);
              margin-left: 12px;
            }
          }
        }
      }
      .ratings_list{
        padding: 0 18px;
        background: #fff;
        li{
          display: flex;
          padding: 18px 0;
          border-1px(rgba(7,17,27,0.1))
          .ratings_list_head{
            flex: 28px 0 1;
            width: 28px;
            img{
              width: 28px;
              height: 28px;
              border-radius: 50%;
            }
          }
          .ratings_list_right{
            flex: 1;
            margin-left: 12px;
            p{
              font-size: 10px;
              line-height: 12px;
              margin-bottom: 4px;
              .ratings_name{
                color: rgb(7,17,27);
              }
              .ratings_time{
                color: rgb(147,153,159);
                font-weight: 200;
              }
              &:nth-child(2){
                margin-bottom: 6px;
                &>span{
                  margin-left: 6px;
                  color: rgb(147,153,159);
                }
                .star_box{
                  span{
                    width: 16px;
                    height: 16px;
                    margin-right: 4px;
                    &:last-child{
                      margin-right: 0;
                    }
                  }
                }
              }
            }
            .ratings_content{
              font-size: 12px;
              color: rgb(7,17,27);
              line-height: 18px;
              margin-bottom: 8px;
            }
            .ratings_tags{
              b{
                display: inline-block;
                font-size: 12px;
                color: rgb(0,160,220);
                line-height: 16px;
                height: 16px;
                vertical-align: top;
                &.icon-thumb_down{
                  color: rgb(147,153,159);
                }
              }
              span{
                display: inline-block;
                width: 60px;
                height: 16px;
                overflow: hidden;
                white-space: nowrap;
                text-overflow: ellipsis;
                text-align: center;
                margin-left: 8px;
                font-size: 9px;
                line-height: 14px;
                padding: 0 6px;
                border: 1px solid rgba(7,17,27,.1);
                border-radius: 1px;
                box-sizing: border-box;
              }
            }
          }
        }
      }
    }
  }
</style>
