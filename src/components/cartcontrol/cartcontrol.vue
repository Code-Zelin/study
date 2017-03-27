<template>
  <div class="cartcontrol clearfix">
    <transition name="subNum">
      <span class="icon-remove_circle_outline app_left" @click="subNum" v-show="food.count>0"></span>
    </transition>
    <transition name="addCart" v-if="!food.count && isDetail">
      <span class="add_shopcart app_right" @click="addNum">加入购物车</span>
    </transition>
    <transition name="addNum" v-else>
      <span class="icon-add_circle app_right" @click="addNum"></span>
    </transition>
    <transition name="count">
      <span class="goods_num_span app_right" v-show="food.count>0">{{food.count}}</span>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
  import vue from 'vue'

  export default {
    props: {
      food: {
        type: Object
      },
      isDetail: {
        type: Boolean,
        default() {
          return false
        }
      }
    },
    methods: {
      // 家数量
      addNum(event) {
        if (!event._constructed && !this.isDetail) {
          return
        }
        if (!this.food.count) {
          vue.set(this.food, 'count', 1)
        } else {
          this.food.count++
        }
        if (!this.isDetail) {
          this.$emit('add', event.target)
        }
      },
      // 减数量
      subNum() {
        if (this.food.count <= 0) {
          return
        } else {
          this.food.count--
        }
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol {
    width: 72px;
    height: 24px;
    line-height: 24px;
    text-align:right;
    position: relative;
    .subNum-enter-active{
      animation: subNum_in .5s;
    }
    .subNum-leave-active{
      animation: subNum_out .5s;
    }
    @keyframes subNum_in {
      0% {
        opacity: 0;
        transform: translateX(48px) rotate(0deg)
      }
      100% {
        opacity: 1;
        transform: translateX(0) rotate(360deg)
      }
    }
    @keyframes subNum_out {
      0% {
        opacity: 1;
        transform: translateX(0) rotate(360deg)
      }
      100% {
        opacity: 0;
        transform: translateX(48px) rotate(0deg)
      }
    }
    .count-enter-active{
      animation: count_in .5s;
    }
    .count-leave-active{
      animation: count_out .5s;
    }
    @keyframes count_in {
      0% {
        opacity: 0;
        width: 0px;
      }
      100% {
        opacity: 1;
        width: 24px;
      }
    }
    @keyframes count_out {
      0% {
        opacity: 1;
        width: 24px;
      }
      100% {
        opacity: 0;
        width: 0px;
      }
    }
    .addCart-enter-active{
      animation: count_in .5s;
    }
    .addCart-leave-active{
      animation: count_out .5s;
    }
    @keyframes addCart_in {
      0% {
        opacity: 0;
        width: 0px;
      }
      100% {
        opacity: 1;
        width: 24px;
      }
    }
    @keyframes addCart_out {
      0% {
        opacity: 1;
        width: 24px;
      }
      100% {
        opacity: 0;
        width: 0px;
      }
    }
    span {
      font-size: 24px;
      color: rgb(0, 160, 220);
      &.goods_num_span{
        display: inline-block;
        width: 24px;
        text-align: center;
        font-size: 10px;
        color: rgb(147, 153, 159);
        vertical-align: top;
        overflow: hidden;
      }
      &.add_shopcart{
        width: 74px;
        height: 24px;
        text-align: center;
        line-height: 24px;
        border-radius: 12px;
        background: rgb(0,160,220);
        color: #fff;
        font-size: 10px;
        position: absolute;
        right: 0;
      }
    }
  }
</style>
