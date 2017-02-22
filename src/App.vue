<template>
  <div class="sellApp">
    <v_header :seller="seller"></v_header>
    <div class="tab border-1px">
      <!-- 使用 router-link 组件来导航. -->
      <!-- 通过传入 `to` 属性指定链接. -->
      <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
      <div class="tab_item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab_item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab_item">
        <router-link to="/seller">店铺</router-link>
      </div>
    </div>
    <!-- 路由出口 -->
    <!-- 路由匹配到的组件将渲染在这里 -->
    <router-view></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import header from './components/header/header.vue'

  export default {
    data () {
      return {
        seller: {}
      }
    },
    created () {
      // GET /someUrl
      this.$http.get('/api/seller').then(response => {
        var vueResponse = response.body
        // get body data
        if (vueResponse.errno === 0) {
          this.seller = vueResponse.data
          console.log(this.seller.name)
        }
      }, response => {
        // error callback
      })
    },
    components: {
      v_header: header
    }
  }
</script>

<style lang='stylus' rel="stylesheet/stylus">
  @import './common/stylus/mixin.styl'
  .sellApp{
    .tab{
      display: flex;
      width:100%;
      border-1px(rgba(7,17,27,0.1));
      .tab_item{
        flex:1;
        height:40px;
        line-height: 40px;
        text-align: center;
        &>a{
            display:block;
            font-size:14px;
            color:rgb(77,85,93);
              &.tab_active{
                 color: rgb(240,20,20);
               }
        }
      }
    }
  }

</style>
