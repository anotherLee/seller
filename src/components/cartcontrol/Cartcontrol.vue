<template>
  <div class="cartControl">
    <transition name="move">
      <div class="cart-remove icon-remove_circle_outline" v-if="food.count > 0" @click.stop="remove"></div>
    </transition>
    <div class="cart-count" v-if="food.count > 0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop="add"></div>
  </div>
</template>

<script>
  import Vue from 'vue'
  export default{
    props: ['food'],
    data () {
      return {

      }
    },
    methods: {
      add (event) {
//        this.$emit('addListCount', this.food.name)
//        if (!event._constructed) {
//          return
//        }
        console.log('add is clicked')
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count += 1
          console.log('count加一')
        }
        this.$emit('cartAdd', event.target)
      },
      remove (event) {
        this.$emit('removeListCount', this.food.name)
        if (!event._constructed) {
          return
        } else {
          console.log('remove is clicked')
          this.food.count--
          while (this.food.count < 0) {
            this.food.count = 0
          }
        }
      }
    }
  }
</script>

<style lang="scss">
  .cartControl{
    position:absolute;
    right:0;
    bottom:12px;
    font-size:0;
    .cart-remove{
      display:inline-block;
      vertical-align:top;
      line-height:24px;
      padding:3px;
      font-size:24px;
      color:rgb(0,160,220);
      &.move-enter-active, &.move-leave-active{
        transition:all 0.4s linear;
      }
      &.move-enter, &.move-leave-to{
        opacity:0;
        transform:translate3D(24px, 0, 0) rotate(180deg);
      }
    }
    .cart-count{
      display:inline-block;
      vertical-align:top;
      text-align:center;
      padding:3px 0;
      height:24px;
      width:10px;
      line-height:24px;
      font-size:10px;
      color:rgb(147,153,159);
    }
    .cart-add{
      display:inline-block;
      vertical-align:top;
      padding:3px;
      font-size:24px;
      color:rgb(0,160,220);
    }
  }

</style>
