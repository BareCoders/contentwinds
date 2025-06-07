<script setup lang="ts">
const { data: page } = await useFetch('/api/cms/home')
</script>

<template>
  <h1>{{ page.title }}</h1>
  <NuxtLink to="/blog/hello-world">
    Go to blog post
  </NuxtLink>
</template>
