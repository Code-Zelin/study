<template>
  <div class="v_header" @click="show_mask">
    <img :src="seller.avatar" alt="">
    <p>
      <span class="brand"></span>
      {{seller.name}}
    </p>
    我是header
    <transition name="fade">
      <div class="mask" v-show="mask_show">
        <div class="mask_content">
          <p>
            <v_star :size="24" :score="seller.score"></v_star>
            {{seller.score}}
          </p>
          <p>{{seller.bulletin}}</p>
          <p>{{seller.bulletin}}</p>
          <p>{{seller.bulletin}}</p>
          <p>{{seller.bulletin}}</p>
          <p>{{seller.bulletin}}</p>
          <p>{{seller.bulletin}}</p>
          <p>{{seller.bulletin}}</p>
          <p>{{seller.bulletin}}</p>
        </div>
        <div class="mask_close">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script type='text/ecmascript-6'>
  import star from '../star/star.vue'

  export default {
    data () {
      return {
        mask_show: false
      }
    },
    props: {
      seller: Object
    },
    methods: {
      show_mask: function () {
        this.mask_show = !this.mask_show
      }
    },
    components: {
      v_star: star
    }
  }
</script>

<style lang='stylus' rel="stylesheet/stylus">
  .v_header{
    padding: 20px;
    .brand{
      display: inline-block;
      width: 30px;
      height: 18px;
      background: url("brand@2x.png") no-repeat center;
      background-size: contain;
    }
    .fade-enter-active, .fade-leave-active {
      transition: opacity 0.5s
    }
    .fade-enter, .fade-leave-active {
      opacity: 0
    }
    .mask{
      position: fixed;
      z-index:9;
      width: 100%;
      height:100%;
      padding-bottom: 32px;
      -webkit-box-sizing: border-box;
      box-sizing: border-box;
      overflow-y: auto;
      top:0;
      left:0;
      background: rgba(7,17,27,0.8);
      .mask_content{
        min-height: calc(100% - 96px);
        margin-top: 32px;
        color: #fff;
        padding:0 32px;
      }
      .mask_close{
        font-size: 32px;
        text-align: center;
        color: rgba(255,255,255,0.5);
      }
    }
  }
</style>
