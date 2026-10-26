<script setup lang="ts">
const { data: navigation } = await useAsyncData('navigation', () =>
  queryCollectionNavigation('content'),
);

const appConfig = useAppConfig();
</script>

<template>
  <!-- 
    核心修改：
    1. bg-[#dbeafe]/70：使用和背景一致的冰蓝色，但只给 70% 的透明度。
    2. backdrop-blur-md：增加毛玻璃磨砂效果，挡住下面的文字。
    3. border-b border-white/50：底部加一条极细的半透明白线，既能在视觉上分割，又显得很高级。
  -->
  <div class="fixed top-0 left-0 right-0 z-50 w-full bg-[#dbeafe]/70 dark:bg-[#191a1c]/70 backdrop-blur-md border-b border-white/50 dark:border-gray-800/50 shadow-sm transition-all">
    
    <div class="flex justify-between items-center px-4 py-4 mx-auto sm:px-8 w-full">
      
      <!-- 左侧导航 -->
      <div class="text-[#1e293b] dark:text-gray-100 flex gap-4 sm:gap-6 overflow-x-auto whitespace-nowrap font-medium">
        <NuxtLink
          v-for="link in navigation"
          :key="link.path" 
          :to="link.path"
          class="hover:text-[#2563eb] dark:hover:text-[#9fbcd0] transition-colors"
          active-class="font-bold text-[#2563eb] dark:text-[#9fbcd0]"
        >{{ link.title }}</NuxtLink>
      </div>
      
      <!-- 右侧社交图标与暗黑模式 -->
      <div class="text-[#475569] dark:text-gray-300 flex items-center gap-3 sm:gap-4 ml-4">
        <a
          v-if="appConfig.socials?.twitter || appConfig.socials?.x"
          class="hover:text-black dark:hover:text-white transition-colors"
          :href="`https://x.com/${appConfig.socials?.twitter || appConfig.socials?.x}`"
          title="X"
        >
          <Icon class="size-5" name="i-simple-icons-x" />
        </a>
        <a
          v-if="appConfig.socials?.github"
          class="hover:text-black dark:hover:text-white transition-colors"
          :href="`https://github.com/${appConfig.socials?.github}`"
          title="GitHub"
        >
          <Icon class="size-5" name="i-simple-icons-github" />
        </a>
        <a
          v-if="appConfig.socials?.mastodon"
          class="hover:text-black dark:hover:text-white transition-colors"
          :href="`https://elk.zone/${appConfig.socials?.mastodon}`" title="Mastodon"
        >
          <Icon class="size-5" name="i-simple-icons-mastodon" />
        </a>
        <a
          v-if="appConfig.socials?.bluesky"
          class="hover:text-black dark:hover:text-white transition-colors"
          :href="`https://bsky.app/profile/${appConfig.socials?.bluesky}`" title="Bluesky"
        >
          <Icon class="size-5" name="i-simple-icons-bluesky" />
        </a>
        <ColorModeSwitch class="hover:text-black dark:hover:text-white transition-colors" />
      </div>
      
    </div>
  </div>
</template>
