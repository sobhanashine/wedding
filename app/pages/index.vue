<script setup lang="ts">
const menuOpen = ref(false)
const formSent = ref(false)
const activeMood = ref(0)

const navItems = [
  { label: 'حال‌وهوا', href: '#moodboard' },
  { label: 'خدمات', href: '#services' },
  { label: 'نمونه‌ها', href: '#gallery' },
  { label: 'رزرو', href: '#start' }
]

const moods = [
  {
    label: 'باغ رویایی',
    title: 'گل‌های آزاد، میزهای بلند، غروب نرم.',
    text: 'برای جشنی که صمیمی، نفس‌دار و شبیه عکس‌های ذخیره‌شده Pinterest شماست.',
    color: 'bg-[#dfe9d9]',
    image: 'https://images.unsplash.com/photo-1523438885200-e635ba2c371e?auto=format&fit=crop&w=1200&q=88'
  },
  {
    label: 'عمارت کلاسیک',
    title: 'نور شمع، پارچه‌های لطیف، ورود سینمایی.',
    text: 'برای زوج‌هایی که یک شب لوکس، مرتب و بی‌نقص می‌خواهند.',
    color: 'bg-[#f2dddf]',
    image: 'https://images.unsplash.com/photo-1519167758481-83f550bb49b3?auto=format&fit=crop&w=1200&q=88'
  },
  {
    label: 'ساحل و مقصد',
    title: 'مراسمی که شبیه یک سفر عاشقانه شروع می‌شود.',
    text: 'برای جشن‌های کوچک‌تر، خاص‌تر و خاطره‌سازتر در شهرهای مقصد.',
    color: 'bg-[#d8e8ef]',
    image: 'https://images.unsplash.com/photo-1522673607200-164d1b6ce486?auto=format&fit=crop&w=1200&q=88'
  }
]

const activeMoodItem = computed<(typeof moods)[number]>(() => moods[activeMood.value] ?? moods[0]!)

const services = [
  { icon: 'i-lucide-sparkles', title: 'طراحی کانسپت', text: 'پالت رنگ، گل‌آرایی، میزآرایی، نور و روایت شب در یک مسیر واحد.' },
  { icon: 'i-lucide-map-pinned', title: 'انتخاب لوکیشن', text: 'پیشنهاد باغ، عمارت یا مقصد بر اساس تعداد مهمان، فصل و بودجه.' },
  { icon: 'i-lucide-wallet-cards', title: 'مدیریت بودجه', text: 'هر تصمیم قبل از اجرا با هزینه روشن و اولویت‌بندی مشخص جلو می‌رود.' },
  { icon: 'i-lucide-headphones', title: 'هماهنگی اجرا', text: 'تیم‌ها، زمان‌بندی، مهمان‌داری و اتفاقات روز مراسم از یک مرکز مدیریت می‌شود.' }
]

const gallery = [
  {
    title: 'شام زیر ریسه‌ها',
    meta: '۱۲۰ مهمان · باغ',
    image: 'https://images.unsplash.com/photo-1478146896981-b80fe463b330?auto=format&fit=crop&w=900&q=88',
    class: 'md:col-span-2 md:row-span-2'
  },
  {
    title: 'ورودی گل‌پوش',
    meta: 'عمارت · تهران',
    image: 'https://images.unsplash.com/photo-1507504031003-b417219a0fde?auto=format&fit=crop&w=900&q=88',
    class: ''
  },
  {
    title: 'اولین رقص',
    meta: 'نورپردازی زنده',
    image: 'https://images.unsplash.com/photo-1519225421980-715cb0215aed?auto=format&fit=crop&w=900&q=88',
    class: ''
  },
  {
    title: 'میز شام شاعرانه',
    meta: 'جزئیات اختصاصی',
    image: 'https://images.unsplash.com/photo-1520854221256-17451cc331bf?auto=format&fit=crop&w=900&q=88',
    class: 'md:col-span-2'
  }
]

const timeline = [
  { step: '۰۱', title: 'جلسه کشف', text: 'سلیقه، خانواده، مهمان‌ها و چیزهایی که نمی‌خواهید را دقیق می‌شنویم.' },
  { step: '۰۲', title: 'Moodboard و بودجه', text: 'تصویر مراسم، انتخاب‌های اصلی و هزینه‌ها را قبل از شروع اجرا شفاف می‌کنیم.' },
  { step: '۰۳', title: 'تولید و اجرا', text: 'از تأمین‌کننده‌ها تا لحظه ورود مهمان‌ها، همه چیز با یک برنامه واحد جلو می‌رود.' }
]

let revealObserver: IntersectionObserver | undefined

onMounted(() => {
  revealObserver = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add('is-visible')
        revealObserver?.unobserve(entry.target)
      }
    })
  }, { threshold: 0.14, rootMargin: '0px 0px -40px' })

  document.querySelectorAll('.motion-reveal').forEach(element => revealObserver?.observe(element))
})

onBeforeUnmount(() => revealObserver?.disconnect())

function closeMenu() {
  menuOpen.value = false
}

function submitForm() {
  formSent.value = true
}
</script>

<template>
  <div class="min-h-screen overflow-x-clip bg-[#fff8f5] text-[#2c2a27] selection:bg-[#e85f76] selection:text-white">
    <header class="fixed inset-x-0 top-0 z-50 px-4 pt-3 sm:px-6">
      <div class="mx-auto flex h-16 max-w-[1500px] items-center justify-between border border-[#2c2a27]/10 bg-[#fffdf9]/85 px-3 shadow-[0_18px_60px_rgba(83,48,54,0.10)] backdrop-blur-2xl sm:px-5">
        <a
          href="#top"
          class="group flex min-w-0 items-center gap-3"
          aria-label="رویا مراسم"
          @click="closeMenu"
        >
          <span class="grid size-11 shrink-0 place-items-center rounded-full bg-[#2c2a27] text-lg font-black text-white transition-transform duration-300 group-hover:rotate-[-8deg]">ر</span>
          <span class="grid min-w-0 leading-none">
            <strong class="text-lg font-black">رویا مراسم</strong>
            <small class="mt-1 text-[10px] font-semibold uppercase tracking-[0.28em] text-[#8b746e] ltr">wedding house</small>
          </span>
        </a>

        <nav
          class="hidden items-center gap-1 lg:flex"
          aria-label="منوی اصلی"
        >
          <a
            v-for="item in navItems"
            :key="item.href"
            :href="item.href"
            class="px-4 py-3 text-sm font-bold text-[#6f625c] transition-colors hover:text-[#2c2a27]"
          >
            {{ item.label }}
          </a>
        </nav>

        <a
          href="#start"
          class="hidden items-center gap-2 border border-[#2c2a27]/15 bg-[#2c2a27] px-5 py-3 text-sm font-black text-white transition-transform hover:-translate-y-0.5 lg:inline-flex"
        >
          رزرو مشاوره
          <UIcon
            name="i-lucide-arrow-up-left"
            class="text-lg"
          />
        </a>

        <button
          type="button"
          class="grid size-11 place-items-center rounded-full border border-[#2c2a27]/20 text-[#2c2a27] lg:hidden"
          :aria-expanded="menuOpen"
          aria-controls="mobile-menu"
          aria-label="باز کردن منو"
          @click="menuOpen = !menuOpen"
        >
          <UIcon
            :name="menuOpen ? 'i-lucide-x' : 'i-lucide-menu'"
            class="text-2xl"
          />
        </button>
      </div>

      <div
        id="mobile-menu"
        class="mx-auto mt-2 grid max-w-[1500px] overflow-hidden border border-[#2c2a27]/10 bg-[#fffdf9] shadow-[0_24px_70px_rgba(83,48,54,0.14)] transition-all duration-300 lg:hidden"
        :class="menuOpen ? 'max-h-80 opacity-100' : 'max-h-0 opacity-0'"
      >
        <a
          v-for="item in navItems"
          :key="item.href"
          :href="item.href"
          class="border-b border-[#2c2a27]/10 px-5 py-4 text-sm font-black"
          @click="closeMenu"
        >
          {{ item.label }}
        </a>
      </div>
    </header>

    <main id="top">
      <section class="relative isolate overflow-hidden px-4 pb-10 pt-28 sm:px-6 lg:pb-12 lg:pt-32">
        <div class="mx-auto grid min-h-[calc(100svh-7rem)] w-full max-w-[1500px] content-end gap-8 lg:min-h-[780px] lg:grid-cols-[minmax(0,0.72fr)_minmax(420px,0.78fr)] lg:items-end">
          <div class="motion-reveal z-10 pb-2 lg:pb-20">
            <p class="mb-5 inline-flex items-center gap-2 border border-[#2c2a27]/12 bg-white/70 px-3 py-2 text-xs font-black text-[#9a6b5f] backdrop-blur">
              <UIcon
                name="i-lucide-sparkles"
                class="text-base"
              />
              اجرای عروسی از صفر تا صد
            </p>
            <h1 class="max-w-[11ch] text-[44px] font-black leading-[1.12] text-[#2c2a27] sm:text-[64px] lg:text-[88px]">
              عروسی آرام، دقیق و کاملاً شما.
            </h1>
            <p class="mt-5 max-w-[560px] text-[15px] font-medium leading-8 text-[#625650] sm:text-lg sm:leading-9">
              طراحی، برنامه‌ریزی و اجرای مراسم عروسی با یک تیم واحد؛ از انتخاب حال‌وهوا و لوکیشن تا مدیریت روز مراسم.
            </p>
            <div class="mt-8 flex flex-col gap-3 sm:flex-row sm:flex-wrap">
              <a
                href="#start"
                class="inline-flex min-h-13 items-center justify-center gap-2 bg-[#2c2a27] px-6 text-sm font-black text-white transition-transform hover:-translate-y-0.5"
              >
                رزرو مشاوره
                <UIcon
                  name="i-lucide-arrow-up-left"
                  class="text-lg"
                />
              </a>
              <a
                href="#gallery"
                class="inline-flex min-h-13 items-center justify-center gap-2 border border-[#2c2a27]/18 bg-white/60 px-6 text-sm font-black text-[#2c2a27] backdrop-blur transition-colors hover:bg-white"
              >
                دیدن نمونه کار
                <UIcon
                  name="i-lucide-images"
                  class="text-lg"
                />
              </a>
            </div>
          </div>

          <div class="motion-reveal relative min-h-[430px] overflow-hidden bg-[#eadfd9] sm:min-h-[560px] lg:min-h-[690px]">
            <img
              src="https://images.unsplash.com/photo-1519741497674-611481863552?auto=format&fit=crop&w=1800&q=90"
              alt="لحظه‌ای آرام از عروس و داماد در روز عروسی"
              class="hero-image size-full object-cover"
              fetchpriority="high"
            >
            <div class="absolute inset-0 bg-[linear-gradient(180deg,rgba(44,42,39,0.02)_0%,rgba(44,42,39,0.34)_100%)]" />
            <div class="absolute inset-x-4 bottom-4 grid gap-3 bg-[#fffdf9]/90 p-4 backdrop-blur sm:inset-x-6 sm:bottom-6 sm:grid-cols-3 sm:p-5">
              <div>
                <span class="block text-[11px] font-black text-[#9a6b5f]">سبک</span>
                <strong class="mt-1 block text-sm font-black">مینیمال عاشقانه</strong>
              </div>
              <div>
                <span class="block text-[11px] font-black text-[#9a6b5f]">اجرا</span>
                <strong class="mt-1 block text-sm font-black">صفر تا صد</strong>
              </div>
              <div>
                <span class="block text-[11px] font-black text-[#9a6b5f]">ظرفیت</span>
                <strong class="mt-1 block text-sm font-black">محدود هر فصل</strong>
              </div>
            </div>
          </div>
        </div>

        <div class="mx-auto mt-8 flex w-full max-w-[1500px] items-center justify-between border-t border-[#2c2a27]/12 pt-5 text-xs font-bold text-[#6f625c]">
          <span>طراحی · برنامه‌ریزی · اجرای روز مراسم</span>
          <a
            href="#moodboard"
            class="grid size-11 place-items-center rounded-full border border-[#2c2a27]/18 bg-white/70"
            aria-label="رفتن به بخش بعد"
          >
            <UIcon
              name="i-lucide-arrow-down"
              class="text-lg"
            />
          </a>
        </div>
      </section>

      <section
        id="moodboard"
        class="px-4 py-20 sm:px-6 lg:py-28"
      >
        <div class="mx-auto grid max-w-[1500px] gap-8 lg:grid-cols-[0.82fr_1.18fr] lg:items-end">
          <div class="motion-reveal">
            <p class="mb-4 text-xs font-black uppercase tracking-[0.28em] text-[#e85f76]">
              choose the feeling
            </p>
            <h2 class="max-w-[10ch] text-4xl font-black leading-tight sm:text-6xl lg:text-7xl">
              اول حس شب را انتخاب می‌کنیم.
            </h2>
            <p class="mt-5 max-w-[520px] text-sm font-medium leading-8 text-[#625650] sm:text-base sm:leading-9">
              زوج‌ها معمولاً با صدها عکس ذخیره‌شده می‌آیند. ما آن‌ها را به یک زبان اجرایی تبدیل می‌کنیم: پالت، لوکیشن، نور، گل، موسیقی، بودجه و زمان‌بندی.
            </p>
          </div>

          <div class="motion-reveal grid gap-4">
            <div class="grid grid-cols-1 gap-2 sm:grid-cols-3">
              <button
                v-for="(mood, index) in moods"
                :key="mood.label"
                type="button"
                class="group flex min-h-16 items-center justify-between border border-[#2c2a27]/15 px-4 text-right text-sm font-black transition-all hover:-translate-y-1"
                :class="activeMood === index ? 'bg-[#2c2a27] text-white shadow-[5px_5px_0_#e85f76]' : 'bg-white text-[#2c2a27]'"
                @click="activeMood = index"
              >
                {{ mood.label }}
                <UIcon
                  name="i-lucide-arrow-up-left"
                  class="text-lg transition-transform group-hover:-translate-x-1"
                />
              </button>
            </div>

            <div class="relative min-h-[520px] overflow-hidden border border-[#2c2a27]/10 bg-[linear-gradient(135deg,#f2dddf,#dff0ed_52%,#fff8f5)] p-3 sm:min-h-[620px]">
              <Transition
                name="mood"
                mode="out-in"
              >
                <div
                  :key="activeMoodItem.label"
                  class="absolute inset-3 grid content-end overflow-hidden"
                >
                  <img
                    :src="activeMoodItem.image"
                    :alt="activeMoodItem.label"
                    class="absolute inset-0 size-full object-cover"
                  >
                  <div class="absolute inset-0 bg-[linear-gradient(0deg,rgba(44,42,39,0.78)_0%,rgba(44,42,39,0.08)_64%)]" />
                  <div class="relative z-10 max-w-[620px] p-5 text-white sm:p-8">
                    <span class="inline-block bg-white px-3 py-2 text-xs font-black text-[#2c2a27]">{{ activeMoodItem.label }}</span>
                    <h3 class="mt-4 text-3xl font-black leading-tight sm:text-5xl">
                      {{ activeMoodItem.title }}
                    </h3>
                    <p class="mt-4 max-w-[500px] text-sm font-medium leading-8 text-white/85 sm:text-base">
                      {{ activeMoodItem.text }}
                    </p>
                  </div>
                </div>
              </Transition>
            </div>
          </div>
        </div>
      </section>

      <section
        id="services"
        class="bg-[#2c2a27] px-4 py-20 text-white sm:px-6 lg:py-28"
      >
        <div class="mx-auto max-w-[1500px]">
          <div class="motion-reveal flex flex-col justify-between gap-6 md:flex-row md:items-end">
            <div>
              <p class="mb-4 text-xs font-black uppercase tracking-[0.28em] text-[#ffb2bf]">
                full service wedding
              </p>
              <h2 class="max-w-[12ch] text-4xl font-black leading-tight sm:text-6xl">
                همه چیز زیر یک سقف.
              </h2>
            </div>
            <p class="max-w-[520px] text-sm font-medium leading-8 text-white/70 sm:text-base">
              خروجی فقط عکس زیبا نیست. هدف این است که خانواده‌ها کمتر درگیر شوند، بودجه قابل کنترل بماند و روز مراسم بدون سؤال‌های پراکنده اجرا شود.
            </p>
          </div>

          <div class="mt-12 grid gap-3 sm:grid-cols-2 xl:grid-cols-4">
            <article
              v-for="service in services"
              :key="service.title"
              class="motion-reveal group border border-white/14 bg-white/[0.04] p-5 transition-all hover:-translate-y-2 hover:bg-white/[0.08]"
            >
              <div class="mb-10 grid size-12 place-items-center rounded-full bg-white text-[#2c2a27] transition-transform group-hover:rotate-[-10deg]">
                <UIcon
                  :name="service.icon"
                  class="text-2xl"
                />
              </div>
              <h3 class="text-xl font-black">
                {{ service.title }}
              </h3>
              <p class="mt-3 text-sm leading-8 text-white/68">
                {{ service.text }}
              </p>
            </article>
          </div>
        </div>
      </section>

      <section
        id="gallery"
        class="px-4 py-20 sm:px-6 lg:py-28"
      >
        <div class="mx-auto max-w-[1500px]">
          <div class="motion-reveal mb-10 flex flex-col gap-5 md:flex-row md:items-end md:justify-between">
            <div>
              <p class="mb-4 text-xs font-black uppercase tracking-[0.28em] text-[#e85f76]">
                real visual direction
              </p>
              <h2 class="text-4xl font-black leading-tight sm:text-6xl">
                یک پین‌بورد قابل اجرا.
              </h2>
            </div>
            <a
              href="#start"
              class="inline-flex items-center gap-2 self-start border-b border-[#2c2a27] pb-2 text-sm font-black"
            >
              ساختن مودبورد مراسم شما
              <UIcon
                name="i-lucide-arrow-up-left"
                class="text-lg"
              />
            </a>
          </div>

          <div class="grid auto-rows-[250px] gap-3 sm:auto-rows-[310px] md:grid-cols-4 md:auto-rows-[240px] lg:auto-rows-[300px]">
            <article
              v-for="item in gallery"
              :key="item.title"
              class="gallery-card motion-reveal group relative overflow-hidden bg-[#eadfd9]"
              :class="item.class"
            >
              <img
                :src="item.image"
                :alt="item.title"
                loading="lazy"
                class="size-full object-cover transition duration-700 group-hover:scale-105"
              >
              <div class="absolute inset-x-0 bottom-0 bg-[linear-gradient(0deg,rgba(44,42,39,0.82),transparent)] p-4 pt-24 text-white">
                <h3 class="text-xl font-black">
                  {{ item.title }}
                </h3>
                <p class="mt-1 text-xs font-bold text-white/75">
                  {{ item.meta }}
                </p>
              </div>
            </article>
          </div>
        </div>
      </section>

      <section class="px-4 py-20 sm:px-6 lg:py-28">
        <div class="mx-auto grid max-w-[1500px] gap-8 lg:grid-cols-[0.95fr_1.05fr] lg:items-center">
          <div class="motion-reveal relative min-h-[520px] overflow-hidden bg-[#f2dddf] p-4">
            <img
              src="https://images.unsplash.com/photo-1511285560929-80b456fea0bc?auto=format&fit=crop&w=1300&q=88"
              alt="جزئیات میز عروسی"
              loading="lazy"
              class="absolute inset-4 size-[calc(100%-32px)] object-cover"
            >
            <div class="absolute bottom-8 right-8 max-w-[270px] bg-white p-5 shadow-[7px_7px_0_#e85f76]">
              <p class="text-xs font-black text-[#e85f76]">
                روایت واقعی
              </p>
              <strong class="mt-2 block text-2xl font-black leading-snug">«ما فقط مهمان شب خودمان بودیم.»</strong>
            </div>
          </div>

          <div class="motion-reveal">
            <p class="mb-4 text-xs font-black uppercase tracking-[0.28em] text-[#e85f76]">
              from idea to afterparty
            </p>
            <h2 class="max-w-[11ch] text-4xl font-black leading-tight sm:text-6xl">
              مسیر اجرا باید آرام باشد.
            </h2>
            <div class="mt-8 grid gap-3">
              <article
                v-for="item in timeline"
                :key="item.step"
                class="grid grid-cols-[58px_1fr] gap-4 border-t border-[#2c2a27]/15 py-5"
              >
                <span class="text-2xl font-black text-[#e85f76]">{{ item.step }}</span>
                <div>
                  <h3 class="text-xl font-black">
                    {{ item.title }}
                  </h3>
                  <p class="mt-2 text-sm leading-8 text-[#625650]">
                    {{ item.text }}
                  </p>
                </div>
              </article>
            </div>
          </div>
        </div>
      </section>

      <section
        id="start"
        class="px-4 pb-16 pt-8 sm:px-6 lg:pb-24"
      >
        <div class="mx-auto grid max-w-[1500px] overflow-hidden bg-[#dff0ed] lg:grid-cols-[0.82fr_1.18fr]">
          <div class="motion-reveal p-6 sm:p-10 lg:p-14">
            <p class="mb-4 text-xs font-black uppercase tracking-[0.28em] text-[#39736c]">
              start here
            </p>
            <h2 class="text-4xl font-black leading-tight sm:text-6xl">
              تاریخ مراسمت را بررسی کنیم.
            </h2>
            <p class="mt-5 max-w-[520px] text-sm font-medium leading-8 text-[#4e625f] sm:text-base">
              شهر، فصل و تعداد مهمان‌ها را بفرستید. اگر ظرفیت داشته باشیم، با چند پیشنهاد اولیه برای تماس مشاوره برمی‌گردیم.
            </p>
            <div class="mt-8 inline-flex items-center gap-3 bg-white px-4 py-3 text-sm font-black text-[#39736c] shadow-[5px_5px_0_#2c2a27]">
              <span class="size-2 rounded-full bg-[#e85f76]" />
              پذیرش محدود بهار و تابستان ۱۴۰۵
            </div>
          </div>

          <form
            class="motion-reveal grid gap-3 bg-white p-4 sm:grid-cols-2 sm:p-6 lg:p-8"
            @submit.prevent="submitForm"
          >
            <label class="grid gap-2 text-sm font-black">
              نام شما
              <input
                class="min-h-14 border border-[#2c2a27]/15 bg-[#fff8f5] px-4 font-medium outline-none transition focus:border-[#e85f76]"
                type="text"
                placeholder="مثلاً نازنین"
                required
              >
            </label>
            <label class="grid gap-2 text-sm font-black">
              شماره تماس
              <input
                class="min-h-14 border border-[#2c2a27]/15 bg-[#fff8f5] px-4 font-medium outline-none transition focus:border-[#e85f76]"
                type="tel"
                inputmode="tel"
                placeholder="۰۹۱۲..."
                required
              >
            </label>
            <label class="grid gap-2 text-sm font-black">
              شهر مراسم
              <input
                class="min-h-14 border border-[#2c2a27]/15 bg-[#fff8f5] px-4 font-medium outline-none transition focus:border-[#e85f76]"
                type="text"
                placeholder="تهران، شمال، مقصد..."
                required
              >
            </label>
            <label class="grid gap-2 text-sm font-black">
              تعداد مهمان
              <input
                class="min-h-14 border border-[#2c2a27]/15 bg-[#fff8f5] px-4 font-medium outline-none transition focus:border-[#e85f76]"
                type="text"
                placeholder="حدودی"
              >
            </label>
            <label class="grid gap-2 text-sm font-black sm:col-span-2">
              مراسم رویایی شما چه حسی دارد؟
              <textarea
                class="min-h-32 resize-y border border-[#2c2a27]/15 bg-[#fff8f5] px-4 py-3 font-medium leading-8 outline-none transition focus:border-[#e85f76]"
                placeholder="چند کلمه درباره سبک، فصل، عکس‌های الهام‌بخش یا محدودیت‌ها..."
              />
            </label>
            <button
              class="inline-flex min-h-14 items-center justify-center gap-2 bg-[#2c2a27] px-6 text-sm font-black text-white shadow-[6px_6px_0_#e85f76] transition-transform hover:-translate-x-1 hover:-translate-y-1 sm:col-span-2"
              type="submit"
            >
              ارسال درخواست مشاوره
              <UIcon
                name="i-lucide-send"
                class="text-lg"
              />
            </button>
            <p
              v-if="formSent"
              class="flex items-center gap-2 bg-[#dff0ed] px-4 py-3 text-sm font-black text-[#39736c] sm:col-span-2"
            >
              <UIcon
                name="i-lucide-circle-check"
                class="text-xl"
              />
              درخواست ثبت شد؛ برای هماهنگی تماس با شما ارتباط می‌گیریم.
            </p>
          </form>
        </div>
      </section>
    </main>

    <footer class="px-4 pb-8 sm:px-6">
      <div class="mx-auto flex max-w-[1500px] flex-col gap-4 border-t border-[#2c2a27]/15 pt-6 text-sm font-bold text-[#625650] sm:flex-row sm:items-center sm:justify-between">
        <span>رویا مراسم · طراحی و اجرای صفر تا صد عروسی</span>
        <a
          href="#top"
          class="inline-flex items-center gap-2 text-[#2c2a27]"
        >
          بازگشت به بالا
          <UIcon name="i-lucide-arrow-up" />
        </a>
      </div>
    </footer>
  </div>
</template>
