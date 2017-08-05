<template xmlns:v-bind="http://www.w3.org/1999/xhtml">
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" v-bind:src="seller.avatar" alt=""/>
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span><span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="support" v-if="seller.supports">
          <span class="icon"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
    </div>
    <div class="bulletin-wrapper">
    </div>
  </div>
</template>

<script>
  export default{
    props: ['seller'],
    data () {
      return {
      }
    },
    created () {
    }
  }

</script>

<style lang="scss">
  @import '../../common/scss/mixin.scss';

  .header{
    background:black; color:#fff;
    .content-wrapper{
      padding:24px 12px 18px 24px; font-size:0;
      .avatar{
        display:inline-block; border-radius:2px; vertical-align:top;
        img{
          border-radius:4px;
        }
      }
      .content{
        display:inline-block; margin-left:16px; font-size:12px;
        .title{
          margin:2px 0 8px 0;
          .brand{
            display:inline-block; margin-right:6px; width:30px; height:18px; vertical-align:top;
            @include background-image(brand); background-size: 30px 18px; background-repeat:no-repeat;
          }
          .name{
            color:rgb(255,255,255); line-height:18px; font-size:16px; font-weight:bold;
          }
        }
        .description{
          margin-bottom:10px; font-size:12px; font-weight:200; color:rgb(255,255,255); line-heigth:12px;
        }

      }
    }
  }

</style>
