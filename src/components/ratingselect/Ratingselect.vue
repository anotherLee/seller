<template>
  <div class="rating-select">
    <div class="food-rating">
      <h1 class="title">商品评价</h1>
      <div class="block-wrapper">
        <div class="block all" @click="clickBlock(0)" :class="{active:click===0}">
          <span class="text">{{description.all}}</span>
          <span class="number">{{ratings.length}}</span>
        </div>
        <div class="block positive" @click="clickBlock(1)" :class="{active:click===1}">
          <span class="text">{{description.positive}}</span>
          <span class="number">{{filterPositive.length}}</span>
        </div>
        <div class="block negative" @click="clickBlock(2)" :class="{active:click===2}">
          <span class="text">{{description.negative}}</span>
          <span class="number">{{filterNegative.length}}</span>
        </div>
      </div>
    </div>

    <div class="only-content">
      <div class="icon-wrapper" @click="changeStatus" :class="{active:only===true}">
        <i class="icon-check_circle"></i>
      </div>
      <div class="text" :class="{active:only===true}">只看有内容的评价</div>
    </div>
  </div>
</template>

<script>
  export default{
    props: ['ratings', 'description', 'click', 'onlyContent'],
    data () {
      return {
//        des: this.description || {all: '总共'},
//        select: this.click,
        only: this.onlyContent
      }
    },
    methods: {
      clickBlock (data) {
//        this.select = data
        this.$emit('clickBlock', data)
      },
      changeStatus () {
        this.only = !this.only
        this.$emit('statusChanged', this.only)
      }
    },
    computed: {
      filterPositive () {
        if (this.ratings) {
          return this.ratings.filter((rating) => {
            return rating.rateType === 0
          })
        } else {
          return []
        }
      },
      filterNegative () {
        if (this.ratings) {
          return this.ratings.filter((rating) => {
            return rating.rateType === 1
          })
        } else {
          return []
        }
      }
    }
  }
</script>

<style lang="scss">
  .rating-select{
    .food-rating{
      margin:18px 18px 0 18px;
      padding-bottom:18px;
      border-bottom:1px solid rgba(7,17,27,0.1);
      .title{
        font-size:14px;
        line-height:14px;
        font-weight:200;
        color:rgb(7,17,27);
      }
      .block-wrapper{
        margin-top:18px;
        font-size:0;
        .block{
          display:inline-block;
          text-align:center;
          width:80px;
          height:45px;
          line-height:45px;
          margin-right:8px;
          border-radius:2px;
          color:rgb(77,85,93);
          background:#ddd;
          &.all{
            background:rgba(0,160,220,0.2);
            &.active{
              background:rgb(0,160,220);
              color:#fff;
            }
          }
          &.positive{
            background:rgba(0,160,220,0.2);
            &.active{
               background:rgb(0,160,220);
               color:#fff;
            }
          }
          &.negative{
            background:rgba(77,85,93,0.2);
            &.active{
              background:rgb(77,85,93);
              color:#fff;
            }
          }
          .text{
            font-size:12px;
            margin-right:4px;
          }
          .number{
            font-size:8px;
          }
        }
      }
    }
    .only-content{
      padding:0 18px;
      border-bottom:1px solid rgba(7,17,27,0.1);
      font-size:0;
      .icon-wrapper{
        line-height:48px;
        display:inline-block;
        font-size:24px;
        margin-right:4px;
        color:rgb(147,153,159);
        &.active{
          color:rgb(0,160,220);
        }
      }
      .text{
        vertical-align:top;
        line-height:48px;
        display:inline-block;
        font-size:12px;
        color:rgb(147,153,159);
        &.active{
          color:rgb(0,160,220);
        }
      }
    }
  }
</style>
