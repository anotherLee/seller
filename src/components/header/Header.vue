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
      <div class="support-count" v-if="seller.supports" @click="showDetail = true">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail = true">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span><i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img v-bind:src="seller.avatar" alt=""/>
    </div>
    <transition name="fade">
      <div class="detail" v-show="showDetail">
        <div class="detail-wrapper">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <Star v-bind:score="seller.score" class="stars-wrapper"></Star>
            <div class="title" v-if="seller.supports">
              <div class="line"></div>
              <div class="information">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul class="detail-list" v-if="seller.supports">
              <li v-for="item in seller.supports" class="detail-information">
                <span class="icon" v-bind:class="classNames[item.type]"></span><span class="text">{{item.description}}</span>
              </li>
            </ul>
            <div class="bulletin-title" v-if="seller.bulletin">
              <div class="line"></div>
              <div class="information">商家公告</div>
              <div class="line"></div>
            </div>
            <p class="bulletin-text" v-if="seller.bulletin">
              {{seller.bulletin}}
            </p>
          </div>
        </div>
        <div class="detail-close" @click="showDetail = false">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
  import Star from '../star/star.vue'

  export default{
    props: ['seller'],
    data () {
      return {
        classNames: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
        showDetail: false
      }
    },
    methods: {
    },
    created () {
    },
//    watch: {
//      showDetail: function () {
//        console.log(this.showDetail)
//        this.$emit('blur')
//      }
//    },
    components: {
      Star
    }

  }

</script>

<style lang="scss">
  @import '../../common/scss/mixin.scss';

  .header{
    position:relative; background-color:rgba(7,17,27,0.5); color:#fff; overflow:hidden;

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
        font-size:0; line-height: 24px; color:rgb(255,255,255);
        border-radius:14px; text-align:center;
        .count{
          font-size:10px; line-height:24px;
        }
        .icon-keyboard_arrow_right{
          font-size:10px; line-height:24px;
        }
      }

    }

    .bulletin-wrapper{
      position:relative; line-height:28px; height:28px; padding:2px 22px 0 12px; background-color:rgba(7,17,27,0.2);
      white-space:nowrap; overflow:hidden; text-overflow:ellipsis; font-size:10px; color:rgb(255,255,255); font-weight:200;
      .bulletin-title{
        display:inline-block; width:22px; height:12px; line-height:28px; margin-top:7px;
        @include background-image(bulletin); background-size:22px 12px; background-repeat:no-repeat;
      }
      .bulletin-text{
        vertical-align:top; font-size:10px; line-height:28px;
      }
      .icon-keyboard_arrow_right{
        position:absolute; right:12px; top:9px; font-size:10px;
      }
    }

    .background{
      position:absolute; top:0; left:0; width:100%; height:100%; z-index:-1; filter:blur(5px); overflow:hidden;
      img{
        position:absolute; left:20%; top:10%; width:255px; height:100%;
      }
    }

    .detail{
      position:fixed; top:0; left:0; width:100%; height:100%; background-color:rgba(7,17,27,0.8); z-index:20;
      overflow:auto;
      &.fade-enter, &.fade-leave-to{
        opacity:0; background-color:rgba(7,17,27,0);
      }
      &.fade-enter-active, &.fade-leave-active{
        transition: all 0.8s;
      }
      .detail-wrapper{ position:relative;
        min-height:100%;
        .detail-main{
          padding-top:64px; padding-bottom:64px;
          .name{
            text-align:center; font-size:16px; font-weight:700;
            color:rgb(255,255,255); line-height:16px;
          }
          .stars-wrapper{
            text-align:center; margin:16px 0 28px;
          }
          .title{
            display:flex; width:80%; margin:0 auto;
            .information{
              margin:0 12px;
            }
            .line{
              position:relative; top:-8px; flex:1; width:110px;
              border-bottom:1px solid rgba(255,255,255,0.2);
            }
          }
          .detail-list{
            width:80%; margin:24px auto 28px; padding-left:12px;
            .detail-information{
              margin-bottom:12px;
              height:16px; line-height:16px;
              &:last-child{
                margin-bottom:0;
              }
              .icon{
                display:inline-block; vertical-align:top; width:16px; height:16px; margin-right:6px;
                background-size:16px 16px; background-repeat:no-repeat;
                &.decrease{
                  @include background-image(decrease_1);
                }
                &.discount{
                  @include background-image(discount_1);
                }
                &.guarantee{
                  @include background-image(guarantee_1);
                }
                &.invoice{
                  @include background-image(invoice_1);
                }
                &.special{
                  @include background-image(special_1);
                }
              }
              .text{
                line-height:16px; vertical-align:top;
                font-size:12px; font-weight:200; color:rgb(255,255,255);
              }
            }
          }
          .bulletin-title{
            @extend .title; margin-top:28px;
          }
          .bulletin-text{
            width:80%; padding:0 12px; margin:24px auto 0 auto;
            font-size:12px; font-weight:200; line-height:24px; color:rgb(255,255,255);
          }

        }
      }
      .detail-close{
        position:relative;
        width:32px; height:32px;
        margin: -64px auto 0 auto;
        font-size:32px; color:rgba(255,255,255,0.5);
        /*clear:both;*/
        .icon-close{
          position:absolute; left:50%; top:50%;
          transform:translate(-50%,-50%);
        }
      }
    }

  }

</style>
