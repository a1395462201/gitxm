<template>
  <div class="Bgstar">
    <div class="start-con">
      <div
       class="star" 
       v-for="(item, index) in parseInt(startsCount)" 
       :key="index" 
       ref="star">
       </div>
    </div>
    <slot/>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
    name: 'bgstar',
    props: {
        startsCount: {
            type: Number,
            default: 800
        }
    },
    data() {
        return {
            distance: 800 //间距
        }
    },
    mounted() {
        const _this = this
        const starArr = _this.$refs.star
        starArr.forEach(item => {
            const speed = 0.2 + (Math.random() * 1)
            const thisDistance = _this.distance + (Math.random() * 300)
            item.style.transformOrigin = `0 0 ${thisDistance}px`
            item.style.transform = `
            translate3d(0, 0, -${thisDistance}px) 
            rotateY(${(Math.random() * 360)}deg) 
            rotateX(${(Math.random() * -50)}deg)
            scale(${speed}, ${speed})`
        })
    }
})
</script>

<style scoped lang="scss">
.Bgstar {
  width: 100%;
  height: 100%;
  position: relative;
  background: radial-gradient(220% 105% at top center, #1b2947 10%, #75517d 40%, #e96f92 65%, #f7f7b6);
  overflow: hidden;
  .start-con {
    position: absolute;
    transform: perspective(500px);
    transform-style: preserve-3d;
    perspective-origin: 50% 100%;
    left: 50%;
    bottom: 0;
    animation: rotate 90s infinite linear;
    z-index: 9;
    .star {
      width: 2px;
      height: 2px;
      border-radius: 50%;
      position: absolute;
      top: 0;
      left: 0;
      z-index: 10;
      background: #ffffff;
      backface-visibility: hidden;
    }
  }
  @keyframes rotate {
    0% {
      transform: perspective(400px) rotateZ(20deg) rotateX(-40deg) rotateY(0);
    }
    100% {
      transform: perspective(400px) rotateZ(20deg) rotateX(-40deg) rotateY(-360deg);
    }
  }
}
</style>