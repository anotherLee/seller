<template>
  <div class="seller">
    <div class="content-wrapper">

      <div class="overview">
        <div class="basic">
          <div class="name">{{seller.name}}</div>
          <div class="extra">
            <Star class="star" :score="seller.score"></Star>
            <span class="ratingCount">({{seller.ratingCount}})</span>
            <span class="sellCount">月售{{seller.sellCount}}份</span>
          </div>
          <div class="collection">
            <div class="iconWrapper" @click="collect" :class="{active:collected===true}">
              <i class="icon-favorite"></i>
            </div>
            <span class="collect" v-show="collected===false">收藏</span>
            <span class="collected" v-show="collected===true">已收藏</span>
          </div>
        </div>
        <div class="moreInfo">
          <div class="send">
            <span class="title">起送价</span>
            <div class="price">
              <span class="number">{{seller.minPrice}}</span>
              <span class="unit">元</span>
            </div>
          </div>
          <div class="deliveryMoney">
            <span class="title">商家配送</span>
            <div class="price">
              <span class="number">{{seller.deliveryPrice}}</span>
              <span class="unit">元</span>
            </div>
          </div>
          <div class="deliveryTime">
            <span class="title">平均配送时间</span>
            <div class="price">
              <span class="number">{{seller.deliveryTime}}</span>
              <span class="unit">分钟</span>
            </div>
          </div>
        </div>
      </div>
      {{'请求改成绝对路径'}}
      <Split></Split>

      <div class="bulletin">
        <h1 class="title">公告与活动</h1>
        <p class="content">{{seller.bulletin}}</p>
        <div class="wrapper">
          <ul>
            <li class="bulletin-item" v-for="(support, index) in seller.supports">
              <span class="icon" :class="classMap[support.type]"></span>
              <span class="information">{{support.description}}</span>
            </li>
          </ul>
        </div>
      </div>

      <Split></Split>

      <div class="scene">
        <div class="title">商家实景</div>
        <div class="picture-wrapper" >
          <div class="picture" v-for="(picture, index) in seller.pics">
            <img :src="picture" alt=""/>
          </div>
        </div>
      </div>

      <Split></Split>

      <div class="informations">
        <div class="title">商家信息</div>
        <div class="info" v-for="(item, index) in seller.infos">
          {{item}}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
//  let faker = {
//    name: '粥品香坊（回龙观）',
//    description: '蜂鸟专送',
//    deliveryTime: 38,
//    score: 4.2,
//    serviceScore: 4.1,
//    foodScore: 4.3,
//    rankRate: 69.2,
//    minPrice: 20,
//    deliveryPrice: 4,
//    ratingCount: 24,
//    sellCount: 90,
//    bulletin: "粥品香坊其烹饪粥料的秘方源于中国千年古法，在融和现代制作工艺，由世界烹饪大师屈浩先生领衔研发。坚守纯天然、0添加的良心品质深得消费者青睐，发展至今成为粥类的引领品牌。是2008年奥运会和2013年园博会指定餐饮服务商。",
//    supports: [
//      {
//        type: 0,
//        description: "在线支付满28减5"
//      },
//      {
//        type: 1,
//        "description": "VC无限橙果汁全场8折"
//      },
//      {
//        "type": 2,
//        "description": "单人精彩套餐"
//      },
//      {
//        "type": 3,
//        "description": "该商家支持发票,请下单写好发票抬头"
//      },
//      {
//        "type": 4,
//        "description": "已加入“外卖保”计划,食品安全保障"
//      }
//    ],
//    "avatar": "https://i.loli.net/2017/08/03/598313417c004.jpg",
//    "pics": [
//      "https://i.loli.net/2017/08/03/59831397a040a.jpeg",
//      "https://i.loli.net/2017/08/03/598313dc536fe.jpeg",
//      "https://i.loli.net/2017/08/03/598314166398b.jpeg",
//      "https://i.loli.net/2017/08/03/5983144652659.jpeg"
//    ],
//    "infos": [
//      "该商家支持发票,请下单写好发票抬头",
//      "品类:其他菜系,包子粥店",
//      "北京市昌平区回龙观西大街龙观置业大厦底商B座102单元1340",
//      "营业时间:10:00-20:30"
//    ]
//  }

  import Star from '../star/star.vue'
  import Split from '../split/split.vue'

  export default{
    props: ['data'],
    data () {
      return {
        seller: {},
//        seller: this.data.seller,
        collected: false,
        classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee']
      }
    },
    methods: {
      collect () {
        console.log('click')
        this.collected = !this.collected
      }
    },
    created () {
      this.$http.get('/static/data.json').then((response) => {
        response = response.body
        this.seller = response.seller
      })
    },
    computed: {
//      if (this)
    },
    components: {
      Star,
      Split
    }
  }

</script>

<style lang="scss">
  @import '../../common/scss/mixin.scss';
  .seller{
    position:absolute;
    left:0;
    top:175px;
    bottom:0;
    width:100%;
    overflow:hidden;
    .content-wrapper{
      overflow-y:scroll;
      width:102%;
      height:100%;
      .overview{
        margin:18px;
        .basic{
          position:relative;
          padding-bottom:18px;
          border-bottom:1px solid rgba(7,17,27,0.1);
          .name{
            line-height:14px;
            font-size:14px;
            color:rgb(7,17,27);
            margin-bottom:8px;
          }
          .extra{
            font-size:0;
            height:17px;
            .star{
              display:inline-block;
              /*vertical-align:top;*/
              margin-right:8px;
              font-size:10px;
              span{
                margin-right:4px;
              }
            }
            .ratingCount{
              display:inline-block;
              vertical-align:top;
              line-height:17px;
              margin-right:12px;
              font-size:10px;
            }
            .sellCount{
              display:inline-block;
              vertical-align:top;
              line-height:17px;
              font-size:10px;
            }
          }
          .collection{
            position:absolute;
            right:0;
            top:0;
            text-align:center;
            width:36px;
            .iconWrapper{
              line-height: 24px;
              font-size:24px;
              color:rgba(7,17,27,0.5);
              &.active{
                 color:rgb(240,20,20);
              }

            }
            .collect{
              line-height:10px;
              font-size:10px;
              font-weight:300;
              color:rgb(77,85,93);
            }
            .collected{
              line-height:10px;
              font-size:10px;
              font-weight:300;
              color:rgb(77,85,93);
            }
          }
        }
        .moreInfo{
          display:flex;
          .send{
            flex:1;
            margin:18px 0 0 0;
            text-align:center;
            .title{
              font-size:10px;
              line-height: 10px;
              color:rgba(7,17,27,0.5);
            }
            .price{
              font-size:0;
              margin-top:4px;
              .number{
                font-size:24px;
                font-weight:200;
                color:rgb(7,17,27);
                line-height:24px;
              }
              .unit{
                margin-top:4px;
                font-size:10px;
                font-weight:200;
                color:rgb(7,17,27);
                line-height:24px;
              }
            }
          }
          .deliveryMoney{
            @extend .send;
          }
          .deliveryTime{
            @extend .send;
          }
        }
      }
      .bulletin{
        margin:18px 18px 0 18px;
        .title{
          line-height:14px;
          font-size:14px;
          color:rgb(7,17,27);
        }
        .content{
          padding:8px 12px 16px 12px;
          line-height:24px;
          font-size:12px;
          font-weight:200;
          color:rgb(240,20,20);
        }
        .wrapper{
          ul{
            .bulletin-item{
              padding-left:12px;
              height:48px;
              font-size:0;
              border-top:1px solid rgba(7,17,27,0.1);
              .icon{
                display:inline-block;
                vertical-align:top;
                width:16px;
                height:16px;
                margin:16px 6px 16px 0;
                background-size:16px 16px;
                background-repeat:no-repeat;
                &.decrease{
                  @include background-image(decrease_4)
                }
                &.discount{
                  @include background-image(discount_4)
                }
                &.special{
                  @include background-image(special_4)
                }
                &.invoice{
                  @include background-image(invoice_4)
                }
                &.guarantee{
                  @include background-image(guarantee_4)
                }
              }
              .information{
                display:inline-block;
                line-height:48px;
                font-size:12px;
              }
            }
          }
        }
      }
      .scene{
        width:90%;
        padding:18px;
        @media screen and (max-width:320px) {
          .scene{
            width:85%;
          }
        }
        .title{
          line-height:14px;
          margin-bottom:12px;
          font-size:14px;
          color:rgb(7,17,27);
        }
        .picture-wrapper{
          width:100%;
          display:flex;
          .picture{
            flex:1;
            /*display:inline-block;*/
            width: 20%;
            margin-right:4px;
            img{
              width:100%;
              border-radius:2px;
            }
          }

        }
      }
      .informations{
        padding:18px 18px 0 18px;
        .title{
          line-height:14px;
          margin-bottom:12px;
          font-size:14px;
          color:rgb(7,17,27);
        }
        .info{
          line-height: 16px;
          padding:16px 12px;
          font-size:12px;
          font-weight:200;
          color:rgb(7,17,27);
          border-top:1px solid rgba(7,17,27,0.1);
        }
      }
    }
  }

</style>
