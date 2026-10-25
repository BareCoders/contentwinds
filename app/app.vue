<script setup lang="ts">
const { cover } = useAppConfig()

useSeoMeta({
  ogImage: cover,
  twitterCard: 'summary_large_image',
})
</script>

<template>
  <div class="min-h-screen flex flex-col sm:pt-6 sm:pb-10">
    <Html lang="en" />
    <NuxtLoadingIndicator />
    
    <AppNavbar />
    
    <main class="flex-grow">
      <!-- 统一限制为 80% -->
      <div class="w-[80%] mx-auto">
        <NuxtPage />
      </div>
    </main>
    
    <AppFooter />
  </div>
</template>

<style>
/* 强制覆盖：只要页面里还有 max-w-3xl，统统让它变成 100% 宽 */
main .max-w-3xl {
  max-width: 100% !important;
}
</style>
