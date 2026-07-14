<script setup lang="ts">
useHead({
  htmlAttrs: {
    lang: 'fa',
    dir: 'rtl'
  },
  meta: [
    { name: 'viewport', content: 'width=device-width, initial-scale=1' },
    { name: 'theme-color', content: '#fff8f5' }
  ],
  link: [
    { rel: 'icon', href: '/favicon.ico' },
    { rel: 'preconnect', href: 'https://fonts.googleapis.com' },
    { rel: 'preconnect', href: 'https://fonts.gstatic.com', crossorigin: '' },
    { rel: 'stylesheet', href: 'https://fonts.googleapis.com/css2?family=Vazirmatn:wght@300;400;500;600;700;800;900&display=swap' }
  ]
})

const title = 'رویا مراسم | اجرای صفر تا صد عروسی'
const description = 'لندینگ فارسی و راست‌چین برای طراحی، برنامه‌ریزی و اجرای کامل مراسم عروسی با تجربه‌ای الهام‌گرفته از Pinterest.'

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
