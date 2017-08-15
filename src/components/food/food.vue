<template xmlns:v-bind="http://www.w3.org/1999/xhtml">
  <transition name="move">
    <div class="food" v-show="showFlag" ref="foodDetail">
      <div class="food-content">

        <div class="image-header">
          <img v-bind:src="food.image" alt=""/>
          <div class="icon-wrapper" @click="showFlag=false">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>

        <div class="content">
          <h1 class="name">{{food.name}}</h1>
          <div class="description">
            <span class="sellCount">月售{{food.sellCount}}</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <div class="nowPrice">￥{{food.price}}</div>
            <div class="oldPrice" v-show="food.oldPrice">￥{{food.oldPrice}}</div>
          </div>
          <div class="addToCart">
            <transition name="button">
              <button class="add" @click.stop.prevent="addFirst" v-show="status">加入购物车</button>
            </transition>
            <Cartcontrol v-bind:food="food" @addListCount="addListCount" @removeListCount="removeListCount" @cartAdd="cartAdd" ref="control" class="control"></Cartcontrol>
          </div>
        </div>

        <Split></Split>

        <div class="information" v-show="food.info">
          <div class="title">商品介绍</div>
          <p class="text">{{food.info}}</p>
        </div>

        <Split v-show="food.info"></Split>

        <Ratingselect :ratings="food.ratings" :description="description" :click="click" :onlyContent="onlyContent" @clickBlock="changeNumber" @statusChanged="changeBool"></Ratingselect>

        <div class="ratings-wrapper">
          <ul>
            <li class="rating-item" v-for="rating in food.ratings" v-show="needShow(rating.rateType, rating.text)">
              <div class="comment">
                <div class="comment-time">{{rating.rateTime}}</div>
                <div class="comment-text">{{rating.text || '该用户没有评价'}}</div>
              </div>
              <div class="user">
                <div class="user-name">{{rating.username}}</div>
                <div class="avatar">
                  <img :src="rating.avatar" alt=""/>
                </div>
              </div>
            </li>
          </ul>
        </div>

      </div>
    </div>
  </transition>

</template>

<script>
  import Cartcontrol from '../cartcontrol/Cartcontrol.vue'
  import Split from '../split/split.vue'
  import Ratingselect from '../ratingselect/Ratingselect.vue'
//  import BScroll from 'better-scroll'
  export default{
    props: ['food'],
    data () {
      return {
        showFlag: false,
        status: true,
        description: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        },
        click: 0,
        onlyContent: false
      }
    },
    methods: {
      show () {
        this.showFlag = true
        this.click = 0
        this.onlyContent = false
        console.log('数据被重置')
//        this.$nextTick(() => {
//          if (!this.scroll) {
//            this.scroll = new BScroll(this.$refs.foodDetail, {
//              click: true
//            })
//          } else {
//            this.scroll.refresh()
//          }
//        })
      },
      addListCount (foodName) {
        this.$emit('addListCount', foodName)
      },
      removeListCount (foodName) {
        this.$emit('removeListCount', foodName)
      },
      addFirst (event) {
//        if (!event._constructed) {
//          return
//        }
        this.$refs.control.add(event)
        this.status = false
      },
      cartAdd (target) {
        this.$emit('cartAdd', target)
      },
      changeNumber (data) {
        this.click = data
      },
      changeBool (bool) {
        this.onlyContent = bool
      },
      needShow (type, text) {
        if (!this.onlyContent) {
          if (this.click === 0) {
            return true
          } else if (type === this.click - 1) {
            return true
          }
        } else {
          if (!text) {
            return false
          } else if (type === this.click - 1) {
            return true
          } else {
            return true
          }
        }
      }
    },
    components: {
      Cartcontrol,
      Split,
      Ratingselect
    }
  }
</script>

<style lang="scss">
  *{
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
    padding:0;
    margin:0;
  }

  .move-enter-active, .move-leave-active {
    transition:all 0.4s;
  }
  .move-enter, .move-leave-to{
    transform:translate3d(100%,0,0);
  }
  .move-enter-to, .move-leave{
    transform:translate3d(0,0,0);
  }
  .food{
    position:fixed;
    left:0;
    top:0;
    right:0;
    bottom:48px;
    background:#fff;
    z-index:5;
    overflow:hidden;
    .food-content{
      overflow-y:scroll;
      width:102%;
      height:100%;
      .image-header{
        position:relative;
        box-sizing: content-box;
        width:100%;
        height:0;
        padding-bottom:100%;
        /*border:1px solid red;*/
        img{
          position:absolute;
          top:0;
          width:100%;
          height:100%;
        }
        .icon-wrapper{
          position:absolute;
          left:15px;
          top:20px;
          .icon-arrow_lift{
            font-size:24px;
            color:white;
          }
        }
      }

      .content{
        position:relative;
        padding:18px;
        .name{
          font-size:14px;
          font-weight:700;
          color:rgb(7,17,27);
          line-height:14px;
        }
        .description{
          margin-top:8px;
          font-size:0;
          color:rgb(147,153,159);
          .sellCount{
            margin-right:12px;
            display:inline-block;
            font-size:10px;
          }
          .rating{
            display:inline-block;
            font-size:10px;
          }
        }
        .price{
          margin-top:16px;
          padding-bottom:16px;
          font-size:0;
          .nowPrice{
            display:inline-block;
            line-height:24px;
            margin-right:8px;
            font-size:14px;
            font-weight:normal;
            color:rgb(240,20,20);
          }
          .oldPrice{
            display:inline-block;
            vertical-align: top;
            line-height: 24px;
            font-size:10px;
            font-weight: normal;
            color:rgb(147,153,159);
            text-decoration:line-through;
          }
        }
        .addToCart{
          position:absolute;
          right:18px;
          bottom:0;
          width:85px;
          .add{
            position:absolute;
            right:0;
            bottom:15px;
            vertical-align:top;
            text-align:center;
            width:74px;
            height:24px;
            /*line-height:10px;*/
            border-radius:12px;
            font-size:10px;
            color:rgb(255,255,255);
            background:rgb(0,160,220);
            border:none;
            z-index:1;
            &:focus{
              outline:none;
            }
            &.button-enter-acitve, &.button-leave-active {
              transition:all 1s;
            }
            &.button-enter, &.button-leave-to{
              opacity:0;
            }
          }

        }
      }
      .information{
        padding:18px;
        .title{
          line-height:14px;
          font-size:14px;
          font-weight:300;
          color:rgb(7,17,27);
        }
        .text{
          padding:6px 8px 0 8px;
          font-size:12px;
          font-weight:200;
          color:rgb(77,85,93);
          line-height:24px;
        }
      }
      .ratings-wrapper{
        ul{
          .rating-item{
            position:relative;
            margin:16px 18px 0;
            padding-bottom:16px;
            border-bottom:1px solid rgba(7,17,27,0.1);
            .comment{
              .comment-time{
                line-height:12px;
                margin-bottom:6px;
                font-size:10px;
                color:rgb(147,153,159);
              }
              .comment-text{
                line-height:16px;
                font-size:12px;
                color:rgb(7,17,27);
              }
            }
            .user{
              position:absolute;
              right:0;
              top:0;
              font-size:0;
              .user-name{
                display:inline-block;
                vertical-align:top;
                margin-right:6px;
                font-size:10px;
                line-height:12px;
                color:rgb(147,153,159);
              }
              .avatar{
                display:inline-block;
                vertical-align:top;
                width:12px;
                height:12px;
                border-radius:50%;
                img{
                  width:100%;
                  height:100%;
                  border-radius:50%;
                }
              }
            }
          }
        }
      }
    }
  }

</style>
