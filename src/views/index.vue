<script setup>
import { useRouter } from 'vue-router'
const router = useRouter()

// 过滤出所有自动化路由中带有 title 的页面
const menuList = router.getRoutes()
  .filter(r => r.meta && r.meta.title)
  .map(r => ({
    path: r.path,
    title: r.meta.title
  }))
</script>

<template>
  <nav>
    <ul>
      <li v-for="item in menuList" :key="item.path">
        <router-link :to="item.path">{{ item.title }}</router-link>
      </li>
    </ul>
  </nav>
</template>