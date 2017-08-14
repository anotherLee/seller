<template xmlns:v-bind="http://www.w3.org/1999/xhtml">
  <div class="shopCart">
    <div class="content-left">
      <div class="logo-wrapper" @click="showList">
        <div class="logo" v-bind:class="{highLight: totalCount > 0}"><span class="icon-shopping_cart" v-bind:class="{highLight: totalCount > 0}"></span></div><div class="totalCount" v-show="totalCount > 0">{{totalCount}}</div></div><div class="price">￥{{totalPrice}}</div> <div class="description border-1px-left"> 另需配送费{{4}}元</div>

    </div>
    <div class="content-right" v-bind:class="{pay: totalPrice >= minPrice}">
      {{payDescription}}
    </div>
    <div class="ball-container">
      <div v-for="(ball, index) in balls" v-bind:key="index">
        <transition name="drop" @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter">
          <div class="ball" v-show="ball.show">
            <div class="inner"></div>
          </div>
        </transition>
      </div>
    </div>

    <div class="shopcart-list" v-show="listShow">
      <div class="shopcart-content" v-bind:class="{ show:fold===false }">
        <div class="title">
          <span class="Cart">购物车</span>
          <span class="removeAll">清空</span>
        </div>
        <div class="food-list">
          <ul>
            <li v-for="food in selectFoods" class="everyFood">
              <span class="name">{{food.name}}</span>
              <span class="price">￥{{food.price}}</span>
              <div class="cartcontrol">
                <Cartcontrol v-bind:food="food" class="control" @addListCount="addListCount" @removeListCount="removeListCount"></Cartcontrol>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
  import Cartcontrol from '../cartcontrol/Cartcontrol.vue'
  export default {
    components: {
      Cartcontrol
    },
    props: ['selectFoods'],
    data () {
      return {
        minPrice: 20,
        balls: [
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          }
        ],
        dropBalls: [],
        fold: true
      }
    },
    computed: {
      totalPrice () {
        let total = 0
        this.selectFoods.forEach((food) => {
          total += food.price * food.count
        })
        return total
      },
      totalCount () {
        let count = 0
        this.selectFoods.forEach((food) => {
          count += food.count
        })
        return count
      },
      payDescription () {
        if (this.totalPrice === 0) {
          return `￥${this.minPrice}起送`
        } else if (this.totalPrice < this.minPrice) {
          let difference = this.minPrice - this.totalPrice
          return `还差${difference}元起送`
        } else {
          return '去结算'
        }
      },
      listShow () {
        if (this.totalCount <= 0) {
          this.fold = true
          return false
        }
        if (this.fold === false) {
          return true
        }
      }
    },
    methods: {
      drop (element) {
        console.log(element)
        for (let i = 0; i < this.balls.length; i++) {
          let ball = this.balls[i]
          if (ball.show === false) {
            ball.show = true
            ball.element = element
            this.dropBalls.push(ball)
            return
          }
        }
      },
      beforeEnter (el) {
        console.log(1)
        for (let i = 0; i < this.balls.length; i++) {
          let ball = this.balls[i]
          if (ball.show === true) {
            let rect = ball.element.getBoundingClientRect()
            console.log(rect)
            let x = rect.left - 32
            let y = -(window.innerHeight - rect.top - 22)
            el.style.display = ''
            el.style.webkitTransform = `translate3d(0,${y}px,0)`
            el.style.transform = `translate3d(0,${y}px,0)`
            let inner = el.getElementsByClassName('inner')[0]
            inner.style.webkitTransform = `translate3d(${x}px,0,0)`
            inner.style.transform = `translate3d(${x}px,0,0)`
          }
        }
      },
      enter (el) {
        /* eslint-disable no-unused-vars */
        let rf = el.offsetHeight
        this.$nextTick(() => {
          el.style.webkitTransform = 'translate3d(0,0,0)'
          el.style.transform = 'translate3d(0,0,0)'
          let inner = el.getElementsByClassName('inner')[0]
          inner.style.webkitTransform = 'translate3d(0,0,0)'
          inner.style.transform = 'translate3d(0,0,0)'
//          el.addEventListener('transitioned', done)
        })
      },
      afterEnter (el) {
        for (let i = 0; i < this.balls.length; i++) {
          let ball = this.balls[i]
          if (ball.show === true) {
            ball.show = false
            el.style.display = 'none'
          }
        }
//        let ball = this.dropBall.shift()
//        if (ball) {
//          ball.show = false
//          el.style.display = 'none'
//        }
      },
      showList () {
        if (!this.totalCount) {
          return
        }
        this.fold = !this.fold
      },
      addListCount (foodName) {
        this.$emit('addListCount', foodName)
      },
      removeListCount (foodName) {
        this.$emit('removeListCount', foodName)
      }
    },
    created () {
      console.log(this.selectFoods)
    }
  }

</script>

<style lang="scss">
  @import '../../common/scss/mixin.scss';
  *{
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
    margin:0; padding:0;
  }
  .shopCart{
    position:fixed;
    left:0;
    bottom:0;
    right:0;
    z-index:10;
    height:48px;
    background:#141d27;
    display:flex;
    .content-left{
      flex:1;
      /*border:1px solid orange;*/
      .logo-wrapper{
        box-sizing: border-box;
        display:inline-block;  /*inline-block经常和vertical-align配合使用*/
        vertical-align:top;
        position:relative;
        top:-10px;
        width:56px;
        height:56px;
        margin:0 12px 0 12px;
        padding:6px;
        border-radius:50%;
        background:#141d27;
        .logo{
          /*box-sizing:content-box;*/
          position:absolute;
          width:44px;
          height:44px;
          border-radius:50%;
          background:#2B343C;
          &.highLight{
            background:rgb(0,160,220);
          }
          /*border:1px solid blue;*/
          .icon-shopping_cart{
            position:absolute;
            left:50%;
            top:50%;
            transform:translate(-50%,-50%);
            font-size:24px;
            line-height:24px;
            color:rgba(255,255,255,0.4);
            &.highLight{
              color:white;
            }
          }
        }
        .totalCount{
          position:absolute;
          top:0;
          right:0;
          width:24px;
          height:16px;
          line-height:16px;
          text-align:center;
          font-size:9px;
          font-weight:700;
          color:rgb(255,255,255);
          border-radius:8px;
          background:rgb(240,20,20);
        }
      }
      .price{
        display:inline-block;
        vertical-align:top;   /*可以把东西提到上面来，很好用，之前写到这里卡住了就是因为没想起来这个属性*/
        /*margin:0 12px 0 0;*/
        height:100%;
        width:37px;
        line-height:48px;
        font-size:16px;
        font-weight:700;
        color:rgba(255,255,255,0.4);
      }
      .description{
        display:inline-block;
        position:relative;
        vertical-align:top;
        height:36px;
        line-height:36px;
        padding-left:12px;
        margin:6px auto;
        font-size:16px;
        font-weight:200;
        color:rgba(255,255,255,0.4);
        /*border-left:1px solid rgba(255,255,255,0.1);*/
        &:after{
          display:block;
          position:absolute;
          left:0;
          top:0;
          content:'';
          width:0;
          height:100%;
          border-left:1px solid rgba(255,255,255,0.1);
        }
      }
    }
    .content-right{
      flex-basis:105px;
      width:105px;
      height:48px;
      line-height:48px;
      text-align:center;
      /*border:1px solid red;*/
      font-size:14px;
      font-weight:300;
      color:rgba(255,255,255,0.4);
      background:#2B343B;
      &.pay{
        background:#00b43c;
        color:rgb(255,255,255);
      }
    }

    .ball-container{
      .ball{
        position:fixed;
        left:32px;
        bottom:22px;
        z-index:10;
        /*width:16px;*/
        /*height:16px;*/
        /*border-radius:50%;*/
        /*background:rgb(0,160,220);*/
        transition:all 0.4s cubic-bezier(.49,-0.29,.75,.41);
        /*&.drop-enter-active{*/
          /*transition: all 0.4 cubic-bezier(.17,-0.39,.96,.18);*/
        /*}*/

          .inner{
            width:16px;
            height:16px;
            border-radius:50%;
            background:rgb(0,160,220);
            transition:all 0.4s;
          }
      }
    }

    .shopcart-list{
      position:fixed;
      top:0;
      left:0;
      right:0;
      bottom:48px;
      background:rgba(7,17,27,0.6);
      z-index:-1;
      .shopcart-content{
        position:absolute;
        left:0;
        right:0;
        bottom:-100%;
        &.show{
          bottom:0;
        }
        .title{
          height:40px;
          line-height:40px;
          padding:0 18px;
          background:#f3f5f7;
          border-bottom:1px solid rgba(7,17,27,0.1);
          &:after{
            display:block;
            content:'';
            width:100%;
            height:0;
            visibility:hidden;
            clear:both;
          }
          .cart{
            font-size:14px;
            font-weight:200;
            color:rgb(7,17,27);
            float:left;
          }
          .removeAll{
            font-size:12px;
            font-weight:200;
            color:rgb(0,160,220);
            float:right;
          }
        }
        .food-list{
          ul{
            background:#fff;
            .everyFood{
              position:relative;
              height:48px;
              line-height:48px;
              margin:0 16px;
              border-bottom:1px solid rgba(7,17,27,0.1);
              .name{
                display:inline-block;
                width:215px;
                font-size:14px;
                color:rgb(7,17,27);
                line-height:48px;
              }
              .price{
                display:inline-block;
                line-height:48px;
                margin-left:15px;
                font-size:14px;
                font-weight:700;
                color:rgb(240,20,20);
              }
              .control{
                position:absolute;
                top:50%;
                right:-3px;
                height:30px;
                transform:translateY(-50%);
              }
            }
          }

        }
      }
    }
  }


</style>
