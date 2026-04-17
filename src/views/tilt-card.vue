<template>
  <div class="container">
    <div 
      class="card"
      ref="cardRef"
      :style="cardStyle"
    >
      <div class="content">
        <span>HELLO</span>
      </div>
      <div class="shine" :style="shineStyle"></div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue';

const cardRef = ref(null);

// 存储旋转角度
const transform = reactive({
  rotateX: 0,
  rotateY: 0,
  glossX: 50,
  glossY: 50,
  opacity: 0
});

// 计算最终的 CSS 样式
const cardStyle = computed(() => {
  const scale = 1;
  return {
    transform: `rotateX(${transform.rotateX}deg) rotateY(${transform.rotateY}deg) scale(${scale})`,
    transition: 'transform 0.5s ease-out'
  };
});

// 计算光影效果
const shineStyle = computed(() => ({
  background: `radial-gradient(circle at ${transform.glossX}% ${transform.glossY}%, rgba(255,255,255,0.5) 0%, transparent 80%)`,
  opacity: transform.opacity
}));

const handleMouseMove = (e) => {
  if (!cardRef.value) return;

  const rect = cardRef.value.getBoundingClientRect();
  const centerX = rect.left + rect.width / 2;
  const centerY = rect.top + rect.height / 2;
  
  // 计算鼠标距离中心点的偏移量 (-1 到 1)
  const percentX = (e.clientX - centerX) / (rect.width / 2);
  const percentY = (e.clientY - centerY) / (rect.height / 2);
  
  const deg = 25; // 最大旋转角度
  transform.rotateY = percentX * deg;
  transform.rotateX = -percentY * deg; // 取反是因为鼠标向上移动时图片应向下倾斜
  
  // 光影跟随
  transform.glossX = (percentX + 1) * 50;
  transform.glossY = (percentY + 1) * 50;
  transform.opacity = 1;
};

const handleMouseLeave = () => {
  transform.rotateX = 0;
  transform.rotateY = 0;
  transform.opacity = 0;
};
</script>

<style scoped>
.container {
  /* 3D 透视的关键 */
  perspective: 1000px; 
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 400px;
  background-color: #0f0f0f;
}

.card {
  position: relative;
  width: 320px;
  height: 200px;
  background: linear-gradient(135deg, #6ee7b7, #3b82f6);
  border-radius: 20px;
  transform-style: preserve-3d;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
  overflow: hidden;
}

.content {
  font-family: 'Arial Black', sans-serif;
  font-size: 4.5rem;
  color: #000;
  /* 让文字在 3D 空间悬浮 */
  transform: translateZ(50px); 
  pointer-events: none;
}

.shine {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  pointer-events: none;
  transition: opacity 0.3s;
}
</style>

<route lang="json">{
  "meta": {
    "title": "鼠标跟随倾斜",
    "category": "animation",
    "tag": "动效"
  }
}</route>