<template>
  <!-- 顶部保留你刚加的灰色虚线 -->
  <footer class="mt-16 pt-8 border-t divider-dashed text-center text-xs sm:text-sm text-theme-text/70 dark:text-dark-text/70">
    
    <!-- flex-wrap 保证手机端放不下时能自动折行，电脑端完美一行 -->
    <div class="flex flex-wrap items-center justify-center gap-x-2 gap-y-1 pb-8">
      <span>© {{ new Date().getFullYear() }} 我的博客. All rights reserved.</span>
      
      <!-- 电脑端显示一根极细的灰色竖线作为分割，手机端隐藏 -->
      <span class="hidden sm:inline text-theme-border dark:text-dark-border">|</span>
      
      <span>
        Powered by 
        <a href="https://nuxt.com" target="_blank" class="hover:text-blue-500 transition-colors">Nuxt</a> & 
        <a href="https://pages.cloudflare.com/" target="_blank" class="hover:text-blue-500 transition-colors">Cloudflare Pages</a>
      </span>
    </div>
    
  </footer>
</template>
