<template>
  <div class="goods">
    <div class="goods_menu">
      <ul>
        <li v-for="menu in goods">
          {{menu.name}}
        </li>
      </ul>
    </div>

    <div class="goods_list">

    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    data () {
      return {
        goods: {}
      }
    },
    created () {
      this.$http.get('/api/goods').then(response => {
        let vueResponse = response.body
        if (vueResponse.errno === 0) {
          this.goods = vueResponse.data
          console.log(this.goods)
        }
      }, response => {
        // error callback
      })
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .goods{
    width:100%;
    display: flex;
    .goods_menu{
      flex: 0 0 80px;
      width: 80px;
    }
    .goods_list{
      
    }
  }
</style>
