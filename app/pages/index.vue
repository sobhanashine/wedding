<script setup lang="ts">
const menuOpen = ref(false)
const formSent = ref(false)
let scrollHandler: (() => void) | undefined
let revealObserver: IntersectionObserver | undefined

const services = [
  { icon: 'i-lucide-gem', title: 'طراحی کانسپت', text: 'از پالت رنگ و گل‌آرایی تا چیدمان میزها؛ یک هویت منسجم که فقط برای شما طراحی می‌شود.' },
  { icon: 'i-lucide-map-pin', title: 'انتخاب لوکیشن', text: 'گلچین باغ‌ها و سالن‌های ممتاز، بازدید تخصصی و انتخاب فضایی متناسب با تعداد مهمان و فصل.' },
  { icon: 'i-lucide-utensils', title: 'پذیرایی و منو', text: 'طراحی منوی اختصاصی، تست غذا و اجرای بی‌نقص پذیرایی با تیم آموزش‌دیده.' },
  { icon: 'i-lucide-camera', title: 'تصویر و روایت', text: 'هماهنگی تیم‌های منتخب عکاسی و فیلم‌سازی برای ثبت صادقانه و سینمایی لحظه‌ها.' },
  { icon: 'i-lucide-music-2', title: 'موسیقی و اجرا', text: 'انتخاب گروه موسیقی، نورپردازی و طراحی ریتم شب؛ از ورود مهمان‌ها تا آخرین رقص.' },
  { icon: 'i-lucide-list-checks', title: 'مدیریت کامل مراسم', text: 'یک مدیر اختصاصی، یک برنامه دقیق و کنترل تمام جزئیات از ماه‌ها قبل تا پایان مراسم.' }
]

const projects = [
  { title: 'شبِ روشن', meta: 'باغ خصوصی · تهران', image: 'https://images.unsplash.com/photo-1519741497674-611481863552?auto=format&fit=crop&w=1400&q=88', class: 'project-tall' },
  { title: 'عهدِ زیتون', meta: 'عمارت تاریخی · کاشان', image: 'https://images.unsplash.com/photo-1507504031003-b417219a0fde?auto=format&fit=crop&w=1200&q=88', class: 'project-wide' },
  { title: 'سپیدار', meta: 'جشن مینیمال · لواسان', image: 'https://images.unsplash.com/photo-1464366400600-7168b8af9bc3?auto=format&fit=crop&w=1200&q=88', class: '' },
  { title: 'طلوعِ ما', meta: 'عروسی صمیمی · شمال', image: 'https://images.unsplash.com/photo-1606800052052-a08af7148866?auto=format&fit=crop&w=1200&q=88', class: '' }
]

const process = [
  { no: '۰۱', title: 'یک گفت‌وگوی واقعی', text: 'درباره شما، سلیقه‌تان و شبی که در ذهن دارید حرف می‌زنیم؛ بدون کاتالوگ و نسخه آماده.' },
  { no: '۰۲', title: 'طراحی روایت مراسم', text: 'کانسپت، بودجه، زمان‌بندی و تیم‌های اجرایی را در یک پیشنهاد شفاف و تصویری کنار هم می‌چینیم.' },
  { no: '۰۳', title: 'ساختن جزئیات', text: 'نمونه‌ها را می‌بینید، منو را تست می‌کنید و هر انتخاب تا رسیدن به نسخه نهایی با شما هماهنگ می‌شود.' },
  { no: '۰۴', title: 'فقط زندگی‌اش کنید', text: 'روز مراسم، همه چیز دست ماست. شما فقط وارد می‌شوید و شبی را زندگی می‌کنید که برایتان ساخته‌ایم.' }
]

const faqs = [
  { q: 'بهتر است چند ماه قبل از مراسم با شما تماس بگیریم؟', a: 'برای طراحی کامل و دسترسی بهتر به لوکیشن‌ها، شروع همکاری بین ۶ تا ۱۰ ماه قبل ایده‌آل است. برای مراسم‌های نزدیک‌تر هم پس از بررسی ظرفیت تیم، راه‌حل مناسب پیشنهاد می‌کنیم.' },
  { q: 'آیا امکان اجرای مراسم خارج از تهران وجود دارد؟', a: 'بله. خانه سپید مراسم‌های مقصدی را در سراسر ایران طراحی و اجرا می‌کند. برنامه سفر، اقامت تیم و لجستیک از ابتدا در پیشنهاد شفاف می‌شود.' },
  { q: 'بودجه مراسم چگونه مدیریت می‌شود؟', a: 'پیش از هر قرارداد، بودجه به بخش‌های روشن تقسیم می‌شود. هر تغییر با تأثیر مالی آن به تأیید شما می‌رسد و گزارش هزینه‌ها در تمام مسیر در دسترس است.' },
  { q: 'می‌توانیم فقط بخشی از خدمات را انتخاب کنیم؟', a: 'تمرکز ما بر مدیریت صفر تا صد است چون کیفیت نهایی به هماهنگی تمام اجزا وابسته است؛ با این حال برای طراحی کانسپت یا مدیریت روز مراسم، پروژه‌ها را موردی بررسی می‌کنیم.' }
]

onMounted(() => {
  const reducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  if (reducedMotion) return

  revealObserver = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add('is-visible')
        revealObserver?.unobserve(entry.target)
      }
    })
  }, { threshold: 0.12 })

  document.querySelectorAll('.reveal').forEach(el => revealObserver?.observe(el))

  scrollHandler = () => {
    document.documentElement.style.setProperty('--scroll-y', `${window.scrollY}px`)
    const progress = window.scrollY / Math.max(document.documentElement.scrollHeight - window.innerHeight, 1)
    document.documentElement.style.setProperty('--page-progress', `${progress}`)
  }
  scrollHandler()
  window.addEventListener('scroll', scrollHandler, { passive: true })
})

onBeforeUnmount(() => {
  revealObserver?.disconnect()
  if (scrollHandler) window.removeEventListener('scroll', scrollHandler)
})

function submitForm() {
  formSent.value = true
}
</script>

<template>
  <div class="site-shell">
    <div
      class="scroll-progress"
      aria-hidden="true"
    />

    <header class="site-header">
      <a
        class="brand"
        href="#top"
        aria-label="خانه سپید، صفحه اصلی"
      >
        <span
          class="brand-mark"
          aria-hidden="true"
        >
          <svg viewBox="0 0 44 44"><circle
            cx="22"
            cy="22"
            r="19"
          /><path d="M14 26c4-1 6-5 8-10 2 5 4 9 8 10M14 29h16" /></svg>
        </span>
        <span><strong>خانه سپید</strong><small>تشریفات و طراحی مراسم</small></span>
      </a>

      <button
        class="menu-button"
        type="button"
        :aria-expanded="menuOpen"
        aria-controls="main-nav"
        aria-label="باز کردن منو"
        @click="menuOpen = !menuOpen"
      >
        <span /><span />
      </button>

      <nav
        id="main-nav"
        class="main-nav"
        :class="{ open: menuOpen }"
        aria-label="منوی اصلی"
      >
        <a
          href="#story"
          @click="menuOpen = false"
        >درباره ما</a>
        <a
          href="#services"
          @click="menuOpen = false"
        >خدمات</a>
        <a
          href="#portfolio"
          @click="menuOpen = false"
        >روایت‌ها</a>
        <a
          href="#process"
          @click="menuOpen = false"
        >مسیر همکاری</a>
      </nav>

      <a
        class="header-cta"
        href="#consultation"
      >رزرو گفت‌وگو <UIcon name="i-lucide-arrow-up-left" /></a>
    </header>

    <main>
      <section
        id="top"
        class="hero section-pad"
      >
        <div class="hero-copy">
          <p class="eyebrow hero-eyebrow">
            <span /> هر عشق، یک روایت بی‌تکرار
          </p>
          <h1>
            ما فقط یک مراسم<br>
            <em>نمی‌سازیم؛</em><br>
            یک شب را ماندگار می‌کنیم.
          </h1>
          <p class="hero-lead">
            طراحی و اجرای صفر تا صد عروسی‌های لوکس؛ با جزئیاتی که از قصه شما شروع می‌شوند و تا آخرین لحظه، دقیق می‌مانند.
          </p>
          <div class="hero-actions">
            <a
              class="button button-primary"
              href="#consultation"
            >رزرو مشاوره خصوصی <UIcon name="i-lucide-arrow-left" /></a>
            <a
              class="text-link"
              href="#portfolio"
            >دیدن روایت‌ها <span>↙</span></a>
          </div>
          <div class="hero-note">
            <div
              class="avatar-stack"
              aria-hidden="true"
            >
              <span /><span /><span />
            </div>
            <p><strong>بیش از ۸۰ روایت</strong><br>از اولین «بله» تا آخرین رقص</p>
          </div>
        </div>

        <div
          class="hero-visual"
          aria-label="مراسم عروسی لوکس در فضای باز"
        >
          <div class="hero-image-wrap">
            <img
              src="https://images.unsplash.com/photo-1519225421980-715cb0215aed?auto=format&fit=crop&w=1800&q=90"
              alt="زوجی در مراسم عروسی لوکس و فضای باز"
              fetchpriority="high"
            >
            <div class="image-wash" />
          </div>
          <div
            class="orbit orbit-one"
            aria-hidden="true"
          />
          <div
            class="orbit orbit-two"
            aria-hidden="true"
          />
          <div
            class="hero-seal"
            aria-hidden="true"
          >
            <svg viewBox="0 0 120 120"><defs><path
              id="sealPath"
              d="M60,60 m-43,0 a43,43 0 1,1 86,0 a43,43 0 1,1 -86,0"
            /></defs><text><textPath href="#sealPath">KHANEH SEFID • SINCE 2017 • </textPath></text><path d="M47 64c6-2 9-8 13-18 4 10 7 16 13 18M47 69h26" /></svg>
          </div>
          <div class="hero-caption">
            <span>۰۱</span><p>هر جزئیات،<br>بخشی از قصه شماست.</p>
          </div>
        </div>
        <a
          class="scroll-cue"
          href="#story"
          aria-label="رفتن به بخش بعد"
        ><span>آرام پایین بروید</span><i /></a>
      </section>

      <section
        id="story"
        class="story section-pad"
      >
        <div class="section-index reveal">
          <span>فصل اول</span><b>قصه از شما شروع می‌شود</b>
        </div>
        <div class="story-grid">
          <div class="story-sticky reveal">
            <p class="eyebrow">
              <span /> خانه سپید
            </p>
            <h2>مراسم شما<br><em>نباید شبیه هیچ‌کس</em><br>دیگر باشد.</h2>
          </div>
          <div class="story-content reveal">
            <p class="story-intro">
              ما قبل از اینکه گل، رنگ یا لوکیشن پیشنهاد کنیم، شما را می‌شناسیم؛ چون زیباترین مراسم‌ها از یک کانسپت آماده شروع نمی‌شوند، از یک رابطه واقعی شروع می‌شوند.
            </p>
            <p>خانه سپید یک تیم طراحی و مدیریت مراسم است. از انتخاب مکان و ساخت هویت بصری تا چشیدن منو، هماهنگی موسیقی و مدیریت ثانیه‌های روز عروسی، تمام مسیر با یک استاندارد و یک تیم جلو می‌رود.</p>
            <div class="story-stats">
              <div><strong>۸</strong><span>سال تجربه<br>ساختن لحظه‌ها</span></div>
              <div><strong>۸۰+</strong><span>مراسم<br>منحصربه‌فرد</span></div>
              <div><strong>۱</strong><span>تیم همراه<br>از صفر تا صد</span></div>
            </div>
          </div>
        </div>
      </section>

      <section
        id="services"
        class="services section-pad"
      >
        <div class="section-heading reveal">
          <div>
            <p class="eyebrow eyebrow-light">
              <span /> آنچه برایتان می‌سازیم
            </p><h2>همه‌چیز، زیر یک سقف.<br><em>همه‌چیز، با یک نگاه.</em></h2>
          </div>
          <p>شما یک نفر را می‌بینید؛ پشت صحنه، تیمی از بهترین‌ها هر جزئیات را دقیق و هماهنگ جلو می‌برد.</p>
        </div>
        <div class="services-list">
          <article
            v-for="(service, index) in services"
            :key="service.title"
            class="service-row reveal"
          >
            <span class="service-no">{{ String(index + 1).padStart(2, '0').replace(/\d/g, d => '۰۱۲۳۴۵۶۷۸۹'.charAt(Number(d))) }}</span>
            <UIcon
              :name="service.icon"
              class="service-icon"
            />
            <h3>{{ service.title }}</h3>
            <p>{{ service.text }}</p>
            <span class="service-arrow">↙</span>
          </article>
        </div>
      </section>

      <section
        id="portfolio"
        class="portfolio section-pad"
      >
        <div class="portfolio-head reveal">
          <div>
            <p class="eyebrow">
              <span /> روایت‌های واقعی
            </p><h2>هر شب،<br><em>یک جهان تازه.</em></h2>
          </div>
          <p>چند قاب از قصه‌هایی که افتخار داشتیم کنارشان باشیم؛ متفاوت در ظاهر، مشترک در دقت و احساس.</p>
        </div>
        <div class="project-grid">
          <a
            v-for="(project, index) in projects"
            :key="project.title"
            href="#consultation"
            class="project-card reveal"
            :class="project.class"
          >
            <img
              :src="project.image"
              :alt="`مراسم ${project.title}`"
              loading="lazy"
            >
            <span class="project-shade" />
            <span class="project-number">۰{{ index + 1 }}</span>
            <span class="project-info"><small>{{ project.meta }}</small><strong>{{ project.title }}</strong></span>
            <span class="project-open"><UIcon name="i-lucide-arrow-up-left" /></span>
          </a>
        </div>
        <a
          class="button button-outline"
          href="#consultation"
        >برای دیدن آرشیو کامل، قرار بگذاریم <UIcon name="i-lucide-arrow-left" /></a>
      </section>

      <section
        id="process"
        class="process section-pad"
      >
        <div class="process-head reveal">
          <p class="eyebrow">
            <span /> از رویا تا واقعیت
          </p>
          <h2>چهار قدم تا شبی<br><em>که فقط باید زندگی‌اش کنید.</em></h2>
        </div>
        <div class="process-track">
          <div
            class="gold-thread"
            aria-hidden="true"
          >
            <span />
          </div>
          <article
            v-for="(item, index) in process"
            :key="item.no"
            class="process-step reveal"
            :class="{ 'step-left': index % 2 }"
          >
            <span class="step-dot"><i /></span>
            <span class="step-no">{{ item.no }}</span>
            <div><h3>{{ item.title }}</h3><p>{{ item.text }}</p></div>
          </article>
        </div>
      </section>

      <section class="testimonial section-pad">
        <div class="quote-mark reveal">
          “
        </div>
        <blockquote class="reveal">
          شب عروسی، برای اولین بار در تمام آن ماه‌ها به ساعت نگاه نکردیم. می‌دانستیم همه‌چیز دست آدم‌هایی‌ست که قصه‌مان را فهمیده‌اند.
        </blockquote>
        <div class="couple reveal">
          <div class="couple-photo">
            <img
              src="https://images.unsplash.com/photo-1529636798458-92182e662485?auto=format&fit=crop&w=300&q=85"
              alt="رها و آرمان"
              loading="lazy"
            >
          </div>
          <div><strong>رها و آرمان</strong><span>مراسم شهریور ۱۴۰۴ · لواسان</span></div>
        </div>
        <div
          class="quote-lines"
          aria-hidden="true"
        >
          <i /><i /><i />
        </div>
      </section>

      <section class="faq section-pad">
        <div class="faq-title reveal">
          <p class="eyebrow">
            <span /> پیش از شروع
          </p><h2>چند سؤال<br><em>که شاید در ذهن شماست.</em></h2>
        </div>
        <div class="faq-list reveal">
          <details
            v-for="(faq, index) in faqs"
            :key="faq.q"
            :open="index === 0"
          >
            <summary><span>{{ faq.q }}</span><i /></summary>
            <p>{{ faq.a }}</p>
          </details>
        </div>
      </section>

      <section
        id="consultation"
        class="consultation section-pad"
      >
        <div class="consultation-copy reveal">
          <p class="eyebrow eyebrow-light">
            <span /> شروع یک روایت تازه
          </p>
          <h2>از شبِ شما<br><em>برایمان بگویید.</em></h2>
          <p>یک گفت‌وگوی ۳۰ دقیقه‌ای، بدون تعهد؛ برای شناختن شما، شنیدن ایده‌ها و دیدن اینکه چطور می‌توانیم کنارتان باشیم.</p>
          <div class="contact-row">
            <span><UIcon name="i-lucide-phone" /></span><div><small>تماس مستقیم</small><a href="tel:+982122640918">۰۲۱ ۲۲۶۴ ۰۹۱۸</a></div>
          </div>
          <div class="contact-row">
            <span><UIcon name="i-lucide-instagram" /></span><div><small>اینستاگرام</small><a href="#">khanehsefid.events</a></div>
          </div>
        </div>
        <form
          class="consultation-form reveal"
          @submit.prevent="submitForm"
        >
          <div class="field-row">
            <label><span>نام و نام خانوادگی</span><input
              required
              type="text"
              name="name"
              placeholder="مثلاً سارا احمدی"
            ></label>
            <label><span>شماره تماس</span><input
              required
              type="tel"
              name="phone"
              inputmode="tel"
              placeholder="۰۹۱۲ ۱۲۳ ۴۵۶۷"
            ></label>
          </div>
          <div class="field-row">
            <label><span>تاریخ تقریبی مراسم</span><input
              type="text"
              name="date"
              placeholder="مثلاً مهر ۱۴۰۵"
            ></label>
            <label><span>تعداد مهمان‌ها</span><input
              type="number"
              name="guests"
              inputmode="numeric"
              placeholder="حدود ۲۵۰ نفر"
            ></label>
          </div>
          <label><span>کمی از مراسمی که در ذهن دارید بگویید</span><textarea
            name="message"
            rows="3"
            placeholder="فضا، حال‌وهوا یا هر چیزی که برایتان مهم است..."
          /></label>
          <button
            class="button form-submit"
            type="submit"
          >
            درخواست مشاوره خصوصی <UIcon name="i-lucide-arrow-left" />
          </button>
          <p
            v-if="formSent"
            class="form-success"
            role="status"
          >
            <UIcon name="i-lucide-circle-check" /> درخواست شما ثبت شد؛ خیلی زود با شما تماس می‌گیریم.
          </p>
          <small class="privacy"><UIcon name="i-lucide-lock-keyhole" /> اطلاعات شما نزد خانه سپید محفوظ می‌ماند.</small>
        </form>
      </section>
    </main>

    <footer class="footer section-pad">
      <div class="footer-top">
        <a
          class="brand brand-footer"
          href="#top"
        ><span class="brand-mark"><svg viewBox="0 0 44 44"><circle
          cx="22"
          cy="22"
          r="19"
        /><path d="M14 26c4-1 6-5 8-10 2 5 4 9 8 10M14 29h16" /></svg></span><span><strong>خانه سپید</strong><small>تشریفات و طراحی مراسم</small></span></a>
        <p>برای هر عشقی که سزاوار<br>یک شب بی‌تکرار است.</p>
        <a
          class="footer-up"
          href="#top"
          aria-label="بازگشت به بالای صفحه"
        ><UIcon name="i-lucide-arrow-up" /></a>
      </div>
      <div class="footer-bottom">
        <span>© ۱۴۰۵ خانه سپید. تمام حقوق محفوظ است.</span>
        <nav><a href="#story">درباره ما</a><a href="#services">خدمات</a><a href="#portfolio">نمونه‌کارها</a><a href="#consultation">تماس</a></nav>
        <span>طراحی‌شده برای شب‌های ماندگار</span>
      </div>
    </footer>
  </div>
</template>
