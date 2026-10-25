<template>
  <!-- 外层 div 去掉 max-w-2xl，改成 w-full -->
  <div class="px-4 py-10 m-auto sm:px-8 sm:rounded-lg w-full sm:shadow">
    <!-- main 里也去掉 max-w-2xl，改成 max-w-none，让文字彻底铺开 -->
    <main class="px-4 mx-auto max-w-none prose">
      <slot>
        <h1 class="text-2xl">Page not found</h1>
      </slot>
    </main>
  </div>
</template>
