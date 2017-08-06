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
          <span class="icon" v-bind:class="classNames[seller.supports[0].type]"></span><span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="support-count" v-if="seller.supports">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span><i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img v-bind:src="seller.avatar" alt=""/>
    </div>
  </div>
</template>

<script>
  export default{
    props: ['seller'],
    data () {
      return {
        classNames: ['decrease', 'discount', 'special', 'invoice', 'guarantee']
      }
    },
    created () {
    }
  }

</script>

<style lang="scss">
  @import '../../common/scss/mixin.scss';

  .header{
    position:relative; background-color:rgba(7,17,27,0.5); color:#fff;
    .content-wrapper{
      position:relative; padding:24px 12px 18px 24px; font-size:0;
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
            display:inline-block; vertical-align:top; margin-right:6px; width:30px; height:18px;
            @include background-image(brand); background-size: 30px 18px; background-repeat:no-repeat;
          }
          .name{
            color:rgb(255,255,255); line-height:18px; font-size:16px; font-weight:bold;
          }
        }
        .description{
          margin-bottom:10px; font-size:12px; font-weight:200; color:rgb(255,255,255); line-heigth:12px;
        }
        .support{
          .icon{
            display:inline-block; vertical-align:top; width:12px; height:12px; margin-right:4px; background-size:12px 12px;
            background-repeat:no-repeat;
            @include icon;
          }
          .text{
            line-height:12px; font-size:10px;
          }
        }

      }
      .support-count{
        position:absolute; right: 12px; bottom:18px; padding:0 7px 0 9px; height:24px; background-color:rgba(0,0,0,0.2);
        font-size:0px; line-height: 24px; color:rgb(255,255,255); border-radius:14px; text-align:center;
        .count{
          font-size:10px;
        }
        .icon-keyboard_arrow_right{
          font-size:10px;
        }
      }

    }
    .bulletin-wrapper{
      position:relative; line-height:28px; height:28px; padding:0 22px 0 12px; background-color:rgba(7,17,27,0.2);
      white-space:nowrap; overflow:hidden; text-overflow:ellipsis; font-size:10px; color:rgb(255,255,255); font-weight:200;
      .bulletin-title{
        display:inline-block; vertical-align:top; width:22px; height:12px; margin-top:7px;
        @include background-image(bulletin); background-size:22px 12px; background-repeat:no-repeat;
      }
      .bulletin-text{
        vertical-align:top; font-size:10px;
      }
      .icon-keyboard_arrow_right{
        position:absolute; right:12px; bottom:10px; font-size:10px;
      }
    }
    .background{
      position:absolute; top:0; left:0; width:100%; height:100%; z-index:-1; filter:blur(5px); overflow:hidden;
      img{
        width:70%; height:100%; position:absolute; left:15%; top:20%;
      }
    }

  }

</style>
