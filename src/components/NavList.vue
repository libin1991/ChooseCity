<template>
  <div class="navbody">
    <div class="navList" @touchstart.stop.prevent="start" @touchmove.stop.prevent="move">
      <div :class="navClass(item)" :data-name="item" v-for="item in navList" :key="item">
        {{item}}
      </div>
    </div>
  </div>
</template>

<script>
import { handleDomData, getIndex } from 'common/js/dom'
export default {
  name: 'NavList',
  props: {
    navList: Array,
    flagText: String
  },
  data () {
    return {
      touch: {
        start: 0,
        startIndex: '',
        end: 0,
        endIndex: ''
      }
    }
  },
  methods: {
    // 根据不同的状况赋值不同的样式
    navClass (item) {
      return this.flagText === item ? 'flagItem' : 'navItem'
    },
    // 向上触发scroll滚动事件
    scrollToElement (item) {
      this.$emit('toElement', item)
    },
    // 触摸开始，并向上触发滚动事件
    start (e) {
      let item = handleDomData(e.target, 'data-name')
      this.touch.start = e.touches[0].pageY
      this.touch.startIndex = getIndex(this.navList, item)
      this.scrollToElement(item)
    },
    // 触摸过程中，根据距离变化应计算滚动到的位置
    move (e) {
      this.touch.end = e.touches[0].pageY
      let distance = this.touch.end - this.touch.start
      this.touch.endIndex = Math.min(Math.max(this.touch.startIndex + Math.floor((distance + 10) / 20), 0), 22)
      this.scrollToElement(this.navList[this.touch.endIndex])
    }
  }
}
</script>

<style lang="stylus" scoped>
.navbody
  width 20px
  border-radius 10px
  height 480px
  position absolute
  top 50%
  right 0
  background #eee
  margin-top -230px
  z-index 95
  box-sizing border-box
  padding 10px 0px
  .navItem
    width 20px
    height 20px
    text-align center
    line-height 20px
    font-size 18px
    color #fff
    user-select none
  .flagItem
    width 20px
    height 20px
    text-align center
    line-height 20px
    font-size 18px
    color #4395FF
    user-select none
</style>
