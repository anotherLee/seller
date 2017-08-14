<template xmlns:v-bind="http://www.w3.org/1999/xhtml">
  <div class="stars">
    <span v-for="(item,index) in itemClasses" v-bind:class="item"></span>
  </div>
</template>

<script>
  let LENGTH = 5
  let CLASS_ON = 'on'
  let CLASS_HALF = 'half'
  let CLASS_OFF = 'off'

  export default {
    props: ['score'],
    data () {
      return {
      }
    },
    computed: {
      itemClasses () {
        let result = []
        let score = Math.floor(this.score * 2) / 2
        let integer = Math.floor(this.score)
        let hasDecimal = score % 1 !== 0
        for (let i = 0; i < integer; i++) {
          result.push(CLASS_ON)
        }
        if (hasDecimal) {
          result.push(CLASS_HALF)
        }
        while (result.length < LENGTH) {
          result.push(CLASS_OFF)
        }
        return result
      }
    },
    created () {
    }
  }
</script>

<style lang="scss">
  .stars{
    span{
      display:inline-block; width:15px; height:15px; margin-right:22px;
      background-size: 15px 15px; background-repeat:no-repeat;
      &:last-child{
        margin-right:0;
      }
      &.on{
        background-image:url('star24_on@3x.png');
      }
      &.half{
        backgournd-image:url('star24_half@3x.png');
      }
      &.off{
        background-image:url('star24_off@3x.png');
      }
    }
  }

</style>
