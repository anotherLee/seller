<template>
  <div class="ratings">
    <div class="ratings-content">
      <div class="overview">
        <div class="overview-left">
          <div class="score">{{seller.score}}</div>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-right">
          <div class="service-score">
            <div class="title">服务态度</div>
            <Star class="star" :score="seller.serviceScore"></Star>
          </div>
          <div class="food-score">
            <div class="title">食物评分</div>
            <Star class="star" :score="seller.foodScore"></Star>
          </div>
          <div class="delivery-time">
            <span class="title">送达时间</span>
            <span class="time">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <Split></Split>

      <Ratingselect :ratings="ratings" :description="description" :click="click" :onlyContent="onlyContent" @clickBlock="changeNumber" @statusChanged="changeBool"></Ratingselect>

      <div class="ratings-wrapper">
        <ul>
          <li class="rating-item" v-for="rating in ratings" v-show="needShow(rating.rateType, rating.text)">
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
</template>

<script>
  import Star from '../star/star.vue'
  import Split from '../split/split.vue'
  import Ratingselect from '../ratingselect/Ratingselect.vue'

  export default{
    props: ['data'],
    data () {
      return {
//        seller: {},
        seller: this.data.seller,
//        ratings: [],
        ratings: this.data.ratings,
        allRatings: {},
        description: {
          all: '全部',
          positive: '满意',
          negative: '不满意'
        },
        click: 0,
        onlyContent: false
      }
    },
    methods: {
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
    created () {
//      this.$http.get('../../../static/data.json').then((response) => {
//        response = response.body
//        this.seller = response.seller
//      })
//      this.$http.get('../../../static/data.json').then((response) => {
//        response = response.body
//        this.ratings = response.ratings
//      })
    },
    components: {
      Star,
      Split,
      Ratingselect
    }
  }

</script>

<style lang="scss">
  .ratings{
    position:absolute;
    top:175px;
    left:0;
    bottom:0;
    width:100%;
    overflow:hidden;
    .ratings-content{
      overflow-y:scroll;
      width:102%;
      height:100%;
      .overview{
        display:flex;
        margin:18px 0;
        .overview-left{
          text-align:center;
          flex-basis:140px;
          padding:12px 10px;
          border-right:1px solid rgba(7,17,27,0.2);
          .score{
            font-size:24px;
            color:rgb(255,153,0);
            line-height:28px;
            margin-bottom:6px;
          }
          .title{
            font-size:12px;
            color:rgb(7,17,27);
            line-height:12px;
            margin-bottom:8px;
          }
          .rank{
            font-size:10px;
            color:rgba(7,17,27,0.5);
            line-height:10px;
          }
        }
        .overview-right{
          flex:1;
          padding:12px 0 12px 18px;
          .service-score{
            font-size:0;
            margin-bottom:8px;
            .title{
              display:inline-block;
              line-height:15px;
              font-size:12px;
              margin-right:10px;
            }
            .star{
              display:inline-block;
              vertical-align:top;
            }
          }
          .food-score{
            @extend .service-score;

          }
          .delivery-time{
            .title{
              display:inline-block;
              line-height:15px;
              font-size:12px;
              margin-right:10px;
            }
            .time{
              font-size:12px;
              color:rgb(147,153,159);
            }
          }
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
