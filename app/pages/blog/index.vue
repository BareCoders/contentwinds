<script setup lang="ts">
// 使用 useFetch 获取数据，并提供一个初始的默认对象
// 这样即使接口 404 或报错，page 也不会是 undefined
const { data: page, error } = await useFetch('/api/cms/home', {
  default: () => ({
    title: '默认首页标题' // 这里可以改成你想要的默认显示内容
  })
})

// 可选：在控制台打印一下错误，方便以后排查（不会影响页面显示）
if (error.value) {
  console.warn('获取 CMS 数据失败，已使用默认数据:', error.value)
}
</script>

<template>
  <!-- 使用可选链 ?. 和 OR 运算符 || 作为双重保险 -->
  <h1>{{ page?.title || '首页' }}</h1>
  
  <!-- 删掉或者注释掉这行 -->
  <!-- 
  <NuxtLink to="/blog/hello-world">
    Go to blog post
  </NuxtLink>  -->
  
</template>
