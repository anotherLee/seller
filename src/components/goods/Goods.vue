<template xmlns:v-bind="http://www.w3.org/1999/xhtml">
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul class="menu-list">
        <li class="menu-item border-1px" v-for="(item,index) in goods" v-bind:class="{current: index === currentIndex}" @click="clickMenu(index, $event)">
          <div class="span-wrapper">
            <span class="icon" v-show="item.type > -1" v-bind:class="classNames[item.type]"></span><span class="text">{{item.name}}</span>
          </div>
        </li>
      </ul>
    </div>
    {{111}}
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li class="food-list food-list-hook" v-for="item in goods">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li class="food-item border-1px" v-for="food in item.foods" @click="chooseFood(food, $event)">
              <div class="icon">
                <img v-bind:src="food.image" alt=""/>
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="description" v-if="food.description">{{food.description}}</p>
                <div class="extra">
                  <span class="sellCount">月售{{food.sellCount}}份</span>
                  <span class="rating">好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="symbol">￥</span>
                  <span class="nowPrice">{{food.price}}</span>
                  <span class="oldPrice" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
              </div>
              <Cartcontrol v-bind:food="food" v-on:cartAdd="_drop"></Cartcontrol>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <Shopcart v-bind:selectFoods="selectedFoods" ref="shopcart" @addListCount="addListCount" @removeListCount="removeListCount"></Shopcart>
    <Food v-bind:food="chosedFood" ref="food" @addListCount="addListCount" @removeListCount="removeListCount" @cartAdd="_drop"></Food>
  </div>

</template>

<script>
  import BScroll from 'better-scroll'
  import Shopcart from '../shopcart/Shopcart.vue'
  import Cartcontrol from '../cartcontrol/Cartcontrol.vue'
  import Food from '../food/food.vue'

  export default{
    components: {
      Shopcart,
      Cartcontrol,
      Food
    },
    data () {
      return {
        goods: [],
        classNames: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
        listHeight: [],
        scrollY: 0,
        test: [
          {
            name: '青椒抱鸡蛋',
            price: 10,
            count: 3
          },
          {
            name: '鱼香肉丝',
            price: 15,
            count: 2
          }
        ],
        chosedFood: null
      }
    },
    created () {
      this.$http.get('../../../static/data.json').then((response) => {
        response = response.body
        this.goods = response.goods
        this.$nextTick(() => {
          this._calculateHeight()
          this._initScroll()
        })
      })
    },
    methods: {
      clickMenu (index, event) {
        if (event._constructed) {
          let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
          let el = foodList[index]
          this.foodsScroll.scrollToElement(el, 10)
        }
      },
      _initScroll () {
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        })
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          probeType: 3,
          click: true
        })
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(pos.y)
          console.log('scrollY: ' + this.scrollY)
        })
      },
      _calculateHeight () {
        let height = 0
        this.listHeight.push(height)
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
        console.log('foodList: ' + foodList)
        for (let i = 0; i < foodList.length; i++) {
          height += foodList[i].clientHeight
          this.listHeight.push(height)
        }
      },
      _drop (target) {
        this.$refs.shopcart.drop(target)
      },
      addListCount (foodName) {
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.name === foodName) {
              food.count += 1
            }
          })
        })
      },
      removeListCount (foodName) {
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.name === foodName) {
              food.count -= 1
            }
          })
        })
      },
      chooseFood (food, event) {
        if (!event._constructed) {
          return
        }
        this.chosedFood = food
        this.$refs.food.show()
      }
    },
    computed: {
      currentIndex () {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i]
          let height2 = this.listHeight[i + 1]
          if (height2) {
            if (this.scrollY >= height1 && this.scrollY < height2) {
              return i
            }
          }
        }
        return 0
      },
      selectedFoods () {
        let foods = []
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food)
            }
          })
        })
        return foods
      }
    }
  }

</script>

<style lang="scss">
  @import '../../common/scss/mixin.scss';

  *{
    box-sizing:border-box;
    padding:0;
  }
  .goods{
    display:flex; position:absolute; top:175px; bottom:46px; width:100%;
    /*border:1px solid red;*/
    .menu-wrapper{
      flex-basis:80px; width:80px; background-color:#f3f5f7; overflow:hidden;
      .menu-list{
        .menu-item{
          -webkit-box-sizing: border-box;
          -moz-box-sizing: border-box;
          box-sizing: border-box;
          width:80px; height:54px;
          padding:0 12px;
          @include border-bottom-1px(rgba(7,17,27,0.2));
          &:after{
          width:54px; left:12px;
          }
          &.current{
            position:relative;
            top:-1px;
            bottom:-1px;
            background:white;
            @include border-bottom-1px-none;
          }
          .span-wrapper{
            position:relative; top:52%; transform:translateY(-50%);
            .icon{
              display:inline-block; vertical-align:top; width:12px; height:12px; line-height:12px;
              background-size:12px 12px; background-repeat:no-repeat;
              @include icon;
            }
            .text{
              vertical-align:top; height:100%; font-size:12px; line-height:12px;
            }
          }

        }
      }
    }

    .foods-wrapper{
      flex:1; overflow:hidden;
      .food-list{
        .title{
          padding-left:14px;
          line-height:26px;
          height:26px;
          background:#f3f5f7;
          border-left:2px solid #d9dde1;
          font-size:12px;
          color:rgb(147,153,159);
        }
        ul{
          .food-item{
            display:flex;
            position:relative;
            margin:18px 18px 0 18px;
            padding-bottom:18px;
            @include border-bottom-1px;
            &:last-child{
              @include border-bottom-1px-none;
            }
            .icon{
              img{
                width:57px; height:57px;
                border-radius:2px;
              }
            }
            .content{
              padding:4px 0 0 10px;
              .name{
                line-height:14px;
                font-size:14px;
                color:rgb(7,17,27);
              }
              .description{
                display:inline-block;
                width:80%;
                line-height:12px;
                margin-top:8px;
                font-size:9px;
                color:rgb(147,153,159);
              }
              .extra{
                line-height:10px;
                font-size:0px;
                color:rgb(147,153,159);
                margin-top:8px;
                .sellCount{
                  display:inline-block;
                  font-size:10px;
                }
                .rating{
                  display:inline-block;
                  font-size:10px;
                  margin-left:12px;
                }
              }
              .price{
                font-weight:700;
                color:rgb(147,153,159);
                font-size:0;
                .symbol{
                  font-size:10px;
                  &:first-child{
                    color:red;
                  }
                }
                .nowPrice{
                  margin-right:8px;
                  font-size:14px;
                  color:red;
                  line-height:24px;
                }
                .oldPrice{
                  display:inline-block;
                  position:relative;
                  bottom:1px;
                  font-size:10px;
                  text-decoration:line-through;
                }
              }
            }
          }
        }
      }
    }
  }

</style>
