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
            <button class="add" @click="addFirst" v-show="status">加入购物车</button>
            <Cartcontrol v-bind:food="food" @addListCount="addListCount" @removeListCount="removeListCount" @cartAdd="cartAdd" ref="control" class="control"></Cartcontrol>
          </div>
        </div>
      </div>
    </div>
  </transition>

</template>

<script>
  import Cartcontrol from '../cartcontrol/Cartcontrol.vue'
  import BScroll from 'better-scroll'
  export default{
    props: ['food'],
    data () {
      return {
        showFlag: false,
        status: true
      }
    },
    methods: {
      show () {
        this.showFlag = true
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.foodDetail, {
              click: true
            })
          } else {
            this.scroll.refresh()
          }
        })
      },
      addListCount (foodName) {
        this.$emit('addListCount', foodName)
      },
      removeListCount (foodName) {
        this.$emit('removeListCount', foodName)
      },
      addFirst ($event) {
        this.$refs.control.add(event)
        this.status = false
      },
      cartAdd (target) {
        this.$emit('cartAdd', target)
      }
    },
    components: {
      Cartcontrol
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
    .food-content{
      .image-header{
        position:relative;
        box-sizing: content-box;
        width:100%;
        height:0;
        padding-top:100%;
        /*border:1px solid red;*/
        img{
          position:absolute;
          top:0;
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
          }

        }
      }
    }
  }

</style>
