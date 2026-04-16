<template>
  <div class="home-wrapper">
    <nav class="article-nav">
      <ul class="article-list">
        <li v-for="item in menuList" :key="item.path" class="article-item">
          <router-link :to="item.path" class="article-link">
            <span class="badge" :class="item.category.toLowerCase()">
              {{ item.category }}
            </span>
            <span class="title">{{ item.title }}</span>
            <span v-if="item.tag" class="tag">#{{ item.tag }}</span>
          </router-link>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script setup>
import { useRouter } from 'vue-router'
import { computed } from 'vue'

const router = useRouter()

const menuList = computed(() => {
  return router.getRoutes()
    .filter(r => r.meta && r.meta.title)
    .map(r => ({
      path: r.path,
      title: r.meta.title,
      category: r.meta.category || '其它', // 获取分类
      tag: r.meta.tag,                    // 获取标签
    }))
})
</script>

<style lang="less" scoped>
.home-wrapper {
  max-width: 800px;
  margin: 20px auto;
  padding: 0 15px;
}

.article-list {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 12px; // 设置列表项之间的间距
}

.article-item {
  background-color: #f8f9fa; // 基础浅灰色背景
  border-radius: 8px;
  transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1); // 平滑过渡
  cursor: pointer;
  border: 1px solid transparent;

  // 鼠标悬停动效
  &:hover {
    background-color: #ffffff; // 悬停时背景变白
    border-color: #42b883;     // 边框变绿（可选）
    transform: translateY(-4px); // 向上移动 4px
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.08); // 添加阴影，增强“升起”感
    
    .article-link .title {
      color: #42b883; // 悬停时文字变色
    }
  }

  // 激活点击状态（微小缩放）
  &:active {
    transform: translateY(-2px) scale(0.98);
  }
}

.article-link {
  display: flex;
  align-items: center;
  padding: 12px 16px; // 适当增加内边距
  text-decoration: none; // 移除默认下划线
  color: inherit;
  
  .badge {
    font-size: 10px; // 稍微调大了一点点，原 8px 略小
    padding: 2px 8px;
    border-radius: 4px;
    margin-right: 12px;
    font-weight: bold;
    text-transform: uppercase;
    flex-shrink: 0; // 防止徽章被挤压
    
    &.vue {
      background-color: #42b883;
      color: white;
    }
    
    &.react {
      background-color: #61dafb;
      color: #282c34;
    }

    &.其它 {
      background-color: #e0e0e0;
      color: #666;
    }
  }

  .title {
    flex: 1;
    font-weight: 500;
    font-size: 16px;
    transition: color 0.3s ease;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis; // 文字过长显示省略号
  }

  .tag {
    font-size: 12px;
    color: #999;
    margin-left: 10px;
    font-style: italic;
  }
}
</style>