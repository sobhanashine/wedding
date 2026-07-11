<script setup lang="ts">
useHead({
  htmlAttrs: {
    lang: 'fa',
    dir: 'rtl'
  },
  meta: [
    { name: 'viewport', content: 'width=device-width, initial-scale=1' },
    { name: 'theme-color', content: '#f7f4ee' }
  ],
  link: [
    { rel: 'icon', href: '/favicon.ico' },
    { rel: 'preconnect', href: 'https://fonts.googleapis.com' },
    { rel: 'preconnect', href: 'https://fonts.gstatic.com', crossorigin: '' },
    { rel: 'stylesheet', href: 'https://fonts.googleapis.com/css2?family=Lalezar&family=Vazirmatn:wght@300;400;500;600;700&display=swap' }
  ]
})

const title = 'خانه سپید | تشریفات لوکس عروسی'
const description = 'طراحی و اجرای صفر تا صد مراسم عروسی لوکس؛ از اولین ایده تا آخرین رقص، با یک تیم همراه و یک روایت منحصربه‌فرد.'

useSeoMeta({
  title,
  description,
  ogTitle: title,
  ogDescription: description,
  ogType: 'website',
  twitterCard: 'summary_large_image'
})
</script>

<template>
  <div id="app-root">
    <NuxtPage />
  </div>
</template>
