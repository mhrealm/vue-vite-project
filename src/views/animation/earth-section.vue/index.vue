<template>
  <section class="floor3-container floor-container" ref="containerRef">
    <div class="sticky">
      <div id="chart__container" ref="chartRef"></div>
      <div class="text">
        <p class="title">No distance is too far, and our steps will never cease.</p>
        <p class="desc">
          This map flashes not only geographical coordinates, but also the days we've spent together. Every stop across borders is a step towards a better future; our story is taking root and sprouting in every corner of the globe.
        </p>
      </div>
    </div>
  </section>
</template>

<script setup>
import { onMounted, ref, onBeforeUnmount } from 'vue';
import Globe from 'globe.gl';
import earthImg from '@/images/earth-night.jpg';
import skyImg from '@/images/night-sky.png';

const chartRef = ref(null);
const highlightIndex = ref(-1);
let world = null;

const initData = [
  { name: "Switzerland", value: [8.227512, 46.818188, 0], zIndex: 0 },
  { name: "Estonia", value: [25.013607, 58.595272], zIndex: 1 },
  { name: "Siberia", value: [82.9357, 55.0084], zIndex: 2 },
  { name: "The Middle East", value: [53.847818, 23.424076], zIndex: 3 },
  { name: "Malaysia", value: [101.975766, 4.210484], zIndex: 4 },
  { name: "South Korea", value: [127.766922, 35.907757], zIndex: 5 },
  { name: "Japan", value: [140.252924, 36.204824], zIndex: 6 },
];


onMounted(() => {
  const width = window.innerWidth;
  const height = window.innerHeight

  // 1. 初始化地球
  world = Globe()(chartRef.value)
    .width(width) // 设置地球画布的宽度
    .height(height)
    .globeImageUrl(earthImg) // 设置地球表面的贴图
    .backgroundImageUrl(skyImg)  // 设置背景图
    .atmosphereColor("#ffb4ff") // 设置地球周围“大气层”的光晕颜色
    .atmosphereAltitude(0.06) // 设置大气层的厚度
    .pointOfView({
      lat: 36.818188, // 设置相机初始化时正对着的经纬度
      lng: 12.227512,
      altitude: 3, // 相机距离地表的高度
    })
    .labelsData(initData)  // 注入数据源
    .labelLat(d => d.value[1]) // 数据里的纬度在 value 数组的第 2 个位置
    .labelLng(d => d.value[0])
    .labelText(d => d.name) // 显示在地球上的文字内容
    .labelSize(d => initData.indexOf(d) === highlightIndex.value ? 2.5 : 1.6) // 文字的大小
    .labelColor(() => "rgba(255, 165, 0, 0.75)") // 文字的颜色
    .labelDotRadius(d => {
      return initData.indexOf(d) === highlightIndex.value ? 1.2 : 0.5;
    })
    .enablePointerInteraction(true); // 开启鼠标交互

  const controls = world.controls(); // 交互控制器
  controls.enableZoom = false; // 禁用缩放
  controls.enablePan = false; // 禁用平移
  controls.autoRotate = true; // 开启自动旋转
  controls.autoRotateSpeed = -1; // 设置旋转速度和方向 负值代表是逆时针

  // 2. 自动高亮循环 
  const interval = setInterval(() => {
    window.requestIdleCallback(() => {
      highlightIndex.value = (highlightIndex.value + 1) % initData.length;
      world.labelsData([...initData]);
    });
  }, 2000);

  // 4. 监听窗口变化
  const handleResize = () => {
    world.width(window.innerWidth);
    world.height(window.innerHeight);
  };
  window.addEventListener("resize", handleResize);

  // 清理函数
  onBeforeUnmount(() => {
    if (interval) clearInterval(interval);
    window.removeEventListener("resize", handleResize);
    if (world) world._destructor?.(); // 销毁地球实例防止内存泄漏
  });
});
</script>

<style scoped>
:root {
  --base: 1920;
}

html {
  font-size: calc(10 / var(--base) * 100vw);
}

#chart__container {
  width: 100%;
  height: 100%;
}

.sticky {
  position: sticky;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
}

.text {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
}

.title {
  font-size: 2rem;
}

.desc{
  font-size: 1.2rem;
}

.floor-container {
  width: 100%;
  height: 200vh;
  position: relative;
  color: #fff;
  background-color: #000;
}
</style>



<route lang="json">{
  "meta": {
    "title": "全球足迹分布图",
    "category": "animation",
    "tag": "动效"
  }
}</route>