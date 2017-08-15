<template xmlns:v-bind="http://www.w3.org/1999/xhtml">
  <div id="app">
    <v-header v-bind:seller="seller"></v-header>
    <div class="nav border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评价</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>

    <router-view></router-view>

  </div>
</template>

<script>
  import Header from './components/header/Header.vue'
  export default{
    components: {
      'v-header': Header
    },
    data () {
      return {
        seller: {}
      }
    },
    methods: {
    },
    created () {
      this.$http.get('../static/data.json').then((response) => {
        response = response.body
        this.seller = response.seller
        console.log(response.data)
      }, function () {
        alert('请求失败')
      })
    }
  }

</script>

<style lang="scss">
  @import "./common/scss/mixin.scss";

  #app{
    .nav{
      display:flex; line-height:40px; width:100%;
      /*border-bottom:1px solid rgba(7,17,27,0.2);*/
      @include border-bottom-1px(rgba(7,17,27,0.2));
      .tab-item{
        flex:1; text-align:center;

        a{
          font-size:14px; color:rgb(77,85,93);
        }

        a.active{
          color:rgb(240,20,20);
        }

      }
    }
  }
</style>
