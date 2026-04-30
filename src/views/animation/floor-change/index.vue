<template>
  <div class="floor-container">
    <div class="sticky">
      <!-- <section class="floor1-container">
        <p>REJO CUBE</p>
        <p>Snap, Switch, Click your style</p>
        <p>FlexiCube™ Design | FunTap Display</p>
        <p>Available Soon</p>
      </section> -->
      <section class="floor2-container">
        <ul class="floor2-content-top">
          <li class="item1">
            <p>FlexiCube™ Design,<br /> FunTap Display</p>
            <img src="./p1_1.png" alt="">
          </li>
          <li class="item2">
            <p>World's First Modular<br /> Heating Device</p>
            <img src="./p1_2.png" alt="">
          </li>
          <li class="item3">
            <p>Fashionable colors,<br />up to 9 combinations.</p>
            <img src="./p1_3.png" alt="">
          </li>
        </ul>
        <ul class="floor2-content-bottom">
          <li class="item4">
            <p>OmniHeat 360°</p>
            <img src="./p1_4.png" alt="">
          </li>
          <li class="item5">
            <p>Auto-heating</p>
            <img src="./p1_5.png" alt="">
          </li>
          <li class="item6">
            <p>2 Power Modes</p>
            <img src="./p1_6.png" alt="">
          </li>
          <li class="item7">
            <p>Clean-free</p>
            <img src="./p1_7.png" alt="">
          </li>
        </ul>
      </section>
    </div>
  </div>

</template>

<script setup>
import { onMounted, onUnmounted } from 'vue';
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
gsap.registerPlugin(ScrollTrigger);

onMounted(() => {
  gsap.context(() => {
    const t1 = gsap.timeline({});
    t1.to(".item2 img", { scale: 0.334, x: 0, duration: 2 })
      .to([".item1", ".item3"], { x: 0, y: 0, duration: 2, }, "<",)

    ScrollTrigger.create({
      animation: t1,
      trigger: ".floor-container",
      start: "top top",
      end: "bottom bottom",
      scrub: true,
      markers: true,
    });
  })
})

</script>

<style lang="less" scoped>
// .theme-color {
//   background: linear-gradient(90deg,
//       #0af 1.02%,
//       #19bcf1 24.08%,
//       #32cfe2 51.95%,
//       #64f3c5 100.01%);
//   background-clip: text;
//   -webkit-background-clip: text;
//   -webkit-text-fill-color: transparent;
// }

.floor-container {
  width: 100%;
  height: 200vh;
  background: #9cdeff;
}

.sticky {
  position: sticky;
  display: flex;
  height: 100vh;
  justify-content: center;
  align-items: center;
}

img {
  width: 100%;
  height: auto;
  display: block;
}

.floor2-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.floor2-container li {
  border-radius: 2rem;
  overflow: hidden;
  position: relative;
  background: linear-gradient(180deg, #fff 0%, #cdefff 53.37%, #9cdeff 100%);
}

.floor2-container li p {
  position: absolute;
  white-space: nowrap;
  top: 2rem;
  left: 50%;
  transform: translate(-50%, 0);
  font: 14px Helvetica Neue, Helvetica, PingFang SC, \5FAE\8F6F\96C5\9ED1, Tahoma, Arial, sans-s;
  font-size: 2.6rem;
  text-align: center;
}

.floor2-container .item2 p {
  left: 25%;
  text-align: left;
}

.floor2-content-top {
  display: grid;
  grid-template-columns: 44.5rem 64rem 44.5rem;
  grid-template-rows: auto;
  gap: 2rem;
}

.floor2-content-bottom {
  width: 157rem;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: auto;
  gap: 2rem;
  margin-top: 2rem;
}
</style>

<route lang="json">{
  "meta": {
    "title": "楼层转场动效",
    "category": "animation",
    "tag": "动效"
  }
}</route>