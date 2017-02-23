<template>
  <span class="sup_span" :class="sup"></span>
</template>

<script type="text/ecmascript-6">
 export default {
   props: {
     num: Number,
     type: Number
   },
   computed: {
     sup() {
       let sup = 'sup_' + this.num + '_' + this.type
       return sup
     }
   }
 }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";

  span.sup_span {
    display: inline-block;
    width: 12px;
    height: 12px;
    vertical-align: top;
    bg_set(ture);
    &.sup_1_0 {
     bg_image(decrease_1);
     }
    &.sup_1_1 {
     bg_image(discount_1);
     }
    &.sup_1_2 {
     bg_image(guarantee_1);
     }
    &.sup_1_3 {
     bg_image(invoice_1);
     }
    &.sup_1_4 {
     bg_image(special_1);
     }
    &.sup_2_0 {
     bg_image(decrease_2);
     }
    &.sup_2_1 {
     bg_image(discount_2);
     }
    &.sup_2_2 {
     bg_image(guarantee_2);
     }
    &.sup_2_3 {
     bg_image(invoice_2);
     }
    &.sup_2_4 {
     bg_image(special_2);
     }
    &.sup_3_0 {
     bg_image(decrease_3);
     }
    &.sup_3_1 {
     bg_image(discount_3);
     }
    &.sup_3_2 {
     bg_image(guarantee_3);
     }
    &.sup_3_3 {
     bg_image(invoice_3);
     }
    &.sup_3_4 {
     bg_image(special_3);
     }
  }
</style>
