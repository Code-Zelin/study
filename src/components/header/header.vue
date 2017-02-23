<template>
  <div class="header" @click="mask_show=!mask_show">
    <div class="header_bg">
      <img :src="seller.avatar">
    </div>
    <div class="header_content">
      <img class="header_img" :src="seller.avatar"/>
      <div class="header_right">
        <h2 class="seller_name">
          <span class="seller_brand"></span>
          {{seller.name}}
        </h2>
        <p class="seller_logistics">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </p>
        <p class="seller_supports" v-if="seller.supports">
          <sup_tag :type="seller.supports[0].type" :num="1"></sup_tag>
          {{seller.supports[0].description}}
        </p>
      </div>
      <div class="header_notice">
        <span></span>
        <p>
          {{seller.bulletin}}
        </p>
        <b class="icon-keyboard_arrow_right"></b>
      </div>
    </div>
    <div class="mask" v-show="mask_show">
      <div class="mask_content">
        <h2 class="mask_name">
          {{seller.name}}
        </h2>
        <p class="star_p">
          <star :size="24" :score="seller.score"></star>
        </p>
        <div class="title clearfix">
          <div class="line border-1px"></div>
          <div class="title_name">
            优惠信息
          </div>
          <div class="line border-1px"></div>
        </div>
        <p class="sup_p" v-for="sup in seller.supports">
          <sup_tag :type="sup.type" :num="2"></sup_tag>
          <span class="sup_content">
            {{sup.description}}
          </span>
        </p>
        <div class="title clearfix">
          <div class="line border-1px"></div>
          <div class="title_name">
            商家公告
          </div>
          <div class="line border-1px"></div>
        </div>
        <p class="seller_bulletin">
          {{seller.bulletin}}
        </p>
      </div>
      <div class="mask_close">
        <i class="icon-close"></i>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from '../star/star.vue'
  import tag from '../tag/tag.vue'

  export default {
    props: [
      'seller'
    ],
    data() {
      return {
        mask_show: false
      }
    },
    components: {
      star: star,
      sup_tag: tag
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";

  .header{
    background: #ccc;
    overflow: hidden;
    position: relative;
    .header_content {
      position: relative;
      padding-top: 24px;
      .header_img {
        width: 64px;
        height: 64px;
        border-radius: 4px;
        overflow: hidden;
        vertical-align: top;
        margin-left: 24px;
      }

      .header_right {
        display: inline-block;
        height: 64px;
        width: 250px;
        color: #fff;
        .seller_name {
          height: 18px;
          line-height: 18px;
          font-size: 16px;
          font-weight: bold;
          .seller_brand {
            display: inline-block;
            width: 30px;
            height: 18px;
            bg_image(brand);
            bg_set(ture);
            vertical-align: top;
          }
        }
        .seller_logistics {
          font-size: 12px;
          margin: 8px 0 10px;
        }
        .seller_supports {
          font-size: 10px;
          font-weight: 200;
        }
      }
    }
    .header_bg{
      width: 100%;
      height: 100%;
      position: absolute;
      z-index:0;
      top: 0;
      filter: blur(10px);
      img{
        width: 100%;
      }
      &:after{
         content: '';
         display: block;
         width: 100%;
         height:100%;
         position: absolute;
         top:0;
         background: rgba(7,17,27,.5);
      }
    }
    .header_notice{
      width:100%;
      height:28px;
      line-height:28px;
      background: rgba(7,17,27,.2);
      margin-top: 18px;
      padding:0 12px;
      span{
        display: inline-block;
        width: 22px;
        height: 100%;
        bg_set(ture);
        bg_image(bulletin);
      }
      p{
        display: inline-block;
        width:80%;
        height: 28px;
        line-height: 28px;
        color: #fff;
        font-weight:200;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
      }
      b{
        display: inline-block;
        height: 28px;
        line-height: 28px;
        vertical-align: top;
        color: #fff;
      }
    }
  }
  .mask{
    width: 100%;
    height:100%;
    overflow-y: auto;
    position: fixed;
    top:0;
    left:0;
    z-index:9;
    background: rgba(7,17,27,.8);
    box-sizing: border-box;
    .mask_content{
      width: 100%;
      min-height: calc(100% - 64px - 96px);
      margin-top: 64px;
      padding: 0 36px;
      color: rgba(255,255,255,1);
      box-sizing: border-box;
      .mask_name{
        text-align: center;
        font-size: 16px;
        font-weight:700;
        color: #ffffff;
        line-height: 16px;
      }
      .star_p{
        text-align: center;
        margin: 16px 0;
      }
      .title{
        width: 100%;
        height: 14px;
        margin-bottom: 24px;
        font-size: 14px;
        .line{
          float: left;
          display: inline-block;
          width: calc((100% - 24px - 4em) / 2);
          height: 14px;
          border-1px(rgba(255,255,255,0.2));
          &:after{
             bottom: 7px;
           }
        }
        .title_name{
          float: left;
          display: inline-block;
          width: calc(4em + 22px);
          padding: 0 10px;
          box-sizing: border-box;
          height: 14px;
          line-height: 14px;
          font-weight: 700;
          vertical-align: top;
        }
      }
      .sup_p{
        font-size: 12px;
        font-weight: 200;
        color: #fff;
        line-height: 12px;
        margin-bottom: 12px;
        padding: 0 12px;
        &:nth-last-child(3){
           margin-bottom: 28px;
        }
        .sup_span{
          vertical-align: middle;
          height: 16px;
          width: 16px;
        }
      }
      .seller_bulletin{
        padding:0 12px;
        font-size: 12px;
        font-weight:200;
        color: #fff;
        line-height: 24px;
        text-indent: 2em;
      }
    }
    .mask_close{
      height: 32px;
      line-height: 32px;
      font-size: 32px;
      text-align: center;
      color: rgba(255,255,255,.5);
      margin: 32px 0;
    }
  }
</style>
