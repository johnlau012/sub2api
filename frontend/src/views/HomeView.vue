<template>
  <!-- Custom Home Content: Full Page Mode -->
  <div v-if="homeContent" class="min-h-[100dvh]">
    <iframe
      v-if="isHomeContentUrl"
      :src="homeContent.trim()"
      class="min-h-[100dvh] w-full border-0"
      allowfullscreen
    ></iframe>
    <!-- SECURITY: homeContent is an admin-only setting. -->
    <div v-else v-html="homeContent"></div>
  </div>

  <div v-else class="cheap-home relative min-h-[100dvh] overflow-hidden bg-slate-50 text-slate-950 dark:bg-slate-950 dark:text-slate-50">
    <div class="cheap-home__wash pointer-events-none absolute inset-x-0 top-0 h-[760px]"></div>

    <header class="relative z-20 border-b border-slate-200/70 bg-slate-50/85 backdrop-blur-xl dark:border-slate-800 dark:bg-slate-950/85">
      <nav class="mx-auto flex h-[72px] max-w-7xl items-center justify-between px-5 sm:px-8">
        <router-link to="/home" class="flex min-w-0 items-center gap-3" aria-label="CheapAPI home">
          <span class="flex h-10 w-10 shrink-0 items-center justify-center overflow-hidden rounded-xl bg-slate-900 shadow-sm shadow-slate-300/70 dark:bg-slate-800 dark:shadow-none">
            <img src="/cheapapi-logo.png" alt="CheapAPI" class="h-full w-full object-cover" />
          </span>
          <span class="truncate text-[17px] font-bold tracking-[-0.02em]">{{ brandName }}</span>
        </router-link>

        <div class="flex items-center gap-1 sm:gap-2">
          <LocaleSwitcher />

          <a
            v-if="docUrl"
            :href="docUrl"
            target="_blank"
            rel="noopener noreferrer"
            class="hidden rounded-lg p-2.5 text-slate-500 transition-colors hover:bg-slate-200/60 hover:text-slate-900 focus:outline-none focus:ring-2 focus:ring-primary-500/50 sm:inline-flex dark:text-slate-400 dark:hover:bg-slate-800 dark:hover:text-white"
            :title="t('home.viewDocs')"
          >
            <Icon name="book" size="md" />
          </a>

          <button
            type="button"
            class="rounded-lg p-2.5 text-slate-500 transition-colors hover:bg-slate-200/60 hover:text-slate-900 focus:outline-none focus:ring-2 focus:ring-primary-500/50 dark:text-slate-400 dark:hover:bg-slate-800 dark:hover:text-white"
            :title="isDark ? t('home.switchToLight') : t('home.switchToDark')"
            @click="toggleTheme"
          >
            <Icon v-if="isDark" name="sun" size="md" />
            <Icon v-else name="moon" size="md" />
          </button>

          <router-link
            :to="isAuthenticated ? dashboardPath : '/login'"
            class="ml-1 inline-flex min-h-10 items-center gap-2 whitespace-nowrap rounded-xl bg-slate-900 px-3.5 text-sm font-semibold text-white shadow-sm transition hover:-translate-y-0.5 hover:bg-slate-800 focus:outline-none focus:ring-2 focus:ring-primary-500/50 focus:ring-offset-2 active:translate-y-0 dark:bg-primary-400 dark:text-slate-950 dark:hover:bg-primary-300 dark:focus:ring-offset-slate-950 sm:px-4"
          >
            <span v-if="isAuthenticated" class="flex h-5 w-5 items-center justify-center rounded-md bg-white/15 text-[10px] font-bold dark:bg-slate-950/10">
              {{ userInitial }}
            </span>
            <span>{{ isAuthenticated ? t('home.dashboard') : t('home.login') }}</span>
            <Icon name="arrowRight" size="sm" :stroke-width="2" />
          </router-link>
        </div>
      </nav>
    </header>

    <main class="relative z-10">
      <section class="mx-auto grid min-h-[calc(100dvh-72px)] max-w-7xl items-center gap-12 px-5 py-14 sm:px-8 lg:grid-cols-[1.08fr_0.92fr] lg:gap-20 lg:py-16">
        <div class="cheap-reveal max-w-3xl">
          <p class="mb-6 inline-flex items-center gap-2 text-sm font-semibold text-primary-700 dark:text-primary-300">
            <Icon name="bolt" size="sm" :stroke-width="2" />
            {{ t('home.market.eyebrow') }}
          </p>

          <h1 class="max-w-[760px] text-5xl font-black leading-[0.98] tracking-[-0.055em] text-slate-950 dark:text-white sm:text-6xl lg:text-[68px]">
            {{ t('home.market.headline') }}
          </h1>

          <p class="mt-7 max-w-[600px] text-lg leading-8 text-slate-600 dark:text-slate-300 sm:text-xl">
            {{ t('home.market.description') }}
          </p>

          <div class="mt-9 flex flex-col gap-3 sm:flex-row">
            <router-link
              :to="isAuthenticated ? dashboardPath : '/login'"
              class="inline-flex min-h-12 items-center justify-center gap-2 whitespace-nowrap rounded-xl bg-primary-600 px-6 text-base font-bold text-white shadow-[0_12px_28px_rgba(13,148,136,0.22)] transition hover:-translate-y-0.5 hover:bg-primary-700 focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2 active:translate-y-0 dark:bg-primary-400 dark:text-slate-950 dark:hover:bg-primary-300 dark:focus:ring-offset-slate-950"
            >
              {{ isAuthenticated ? t('home.goToDashboard') : t('home.market.primaryCta') }}
              <Icon name="arrowRight" size="md" :stroke-width="2" />
            </router-link>

            <a
              v-if="docUrl"
              :href="docUrl"
              target="_blank"
              rel="noopener noreferrer"
              class="inline-flex min-h-12 items-center justify-center gap-2 whitespace-nowrap rounded-xl border border-slate-300 bg-white/80 px-6 text-base font-semibold text-slate-800 transition hover:-translate-y-0.5 hover:border-slate-400 hover:bg-white focus:outline-none focus:ring-2 focus:ring-primary-500/50 active:translate-y-0 dark:border-slate-700 dark:bg-slate-900/80 dark:text-slate-100 dark:hover:border-slate-600 dark:hover:bg-slate-900"
            >
              {{ t('home.market.guideCta') }}
              <Icon name="book" size="md" />
            </a>
          </div>

          <div class="mt-9 grid max-w-[650px] gap-3 text-sm text-slate-600 dark:text-slate-300 sm:grid-cols-3">
            <div v-for="benefit in heroBenefits" :key="benefit" class="flex items-center gap-2">
              <span class="flex h-5 w-5 shrink-0 items-center justify-center rounded-md bg-primary-100 text-primary-700 dark:bg-primary-950 dark:text-primary-300">
                <Icon name="check" size="xs" :stroke-width="2.5" />
              </span>
              <span>{{ benefit }}</span>
            </div>
          </div>
        </div>

        <div class="cheap-reveal cheap-reveal--delay relative mx-auto w-full max-w-[520px] lg:mx-0 lg:ml-auto">
          <div class="offer-panel overflow-hidden rounded-2xl border border-slate-200 bg-white/90 shadow-[0_28px_90px_rgba(15,23,42,0.12)] backdrop-blur-xl dark:border-slate-800 dark:bg-slate-900/90 dark:shadow-[0_28px_90px_rgba(0,0,0,0.32)]">
            <div class="border-b border-slate-200 px-6 py-5 dark:border-slate-800 sm:px-7">
              <div class="flex items-center justify-between gap-4">
                <p class="font-semibold text-slate-600 dark:text-slate-300">{{ t('home.market.pricing.title') }}</p>
                <span class="rounded-lg bg-primary-50 px-2.5 py-1 text-xs font-bold text-primary-700 dark:bg-primary-950 dark:text-primary-300">
                  {{ t('home.market.pricing.payAsYouGo') }}
                </span>
              </div>

              <div class="mt-7 grid grid-cols-[1fr_auto_1fr] items-end gap-3">
                <div>
                  <p class="text-xs font-semibold text-slate-500 dark:text-slate-400">{{ t('home.market.pricing.pay') }}</p>
                  <p class="mt-1 text-4xl font-black tracking-[-0.05em] text-slate-950 dark:text-white">HK$1</p>
                </div>
                <Icon name="arrowRight" size="lg" class="mb-2 text-primary-600 dark:text-primary-400" :stroke-width="2" />
                <div class="text-right">
                  <p class="text-xs font-semibold text-slate-500 dark:text-slate-400">{{ t('home.market.pricing.credit') }}</p>
                  <p class="mt-1 text-4xl font-black tracking-[-0.05em] text-primary-600 dark:text-primary-400">US$1</p>
                </div>
              </div>

              <div class="mt-7 flex items-center gap-3 rounded-xl bg-slate-950 p-4 text-white dark:bg-slate-800">
                <span class="flex h-10 w-10 shrink-0 items-center justify-center rounded-lg bg-primary-400 text-slate-950">
                  <Icon name="trendingUp" size="md" :stroke-width="2" />
                </span>
                <div>
                  <p class="font-bold">{{ t('home.market.pricing.discount') }}</p>
                  <p class="mt-0.5 text-sm text-slate-300">{{ t('home.market.pricing.discountNote') }}</p>
                </div>
              </div>
            </div>

            <div class="px-6 py-5 sm:px-7">
              <p class="text-sm font-semibold text-slate-700 dark:text-slate-200">{{ t('home.market.pricing.toolsTitle') }}</p>
              <div class="mt-4 grid grid-cols-2 gap-2.5">
                <div v-for="tool in supportedTools" :key="tool" class="flex items-center gap-2 rounded-xl border border-slate-200 bg-slate-50 px-3 py-2.5 text-sm font-medium text-slate-700 dark:border-slate-700 dark:bg-slate-950/50 dark:text-slate-200">
                  <Icon name="terminal" size="sm" class="text-primary-600 dark:text-primary-400" />
                  {{ tool }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="border-y border-slate-200 bg-white/70 dark:border-slate-800 dark:bg-slate-900/45">
        <div class="mx-auto max-w-7xl px-5 py-16 sm:px-8 lg:py-20">
          <div class="max-w-2xl">
            <h2 class="text-3xl font-black tracking-[-0.035em] text-slate-950 dark:text-white sm:text-4xl">
              {{ t('home.market.models.title') }}
            </h2>
            <p class="mt-4 text-base leading-7 text-slate-600 dark:text-slate-300">
              {{ t('home.market.models.description') }}
            </p>
          </div>

          <div class="mt-10 grid gap-4 md:grid-cols-2 lg:grid-cols-[1.15fr_0.85fr]">
            <div class="grid gap-3 rounded-2xl border border-slate-200 bg-white p-4 shadow-sm dark:border-slate-800 dark:bg-slate-900 sm:grid-cols-2">
              <div v-for="model in featuredModels" :key="model.name" class="group flex items-center gap-3 rounded-xl bg-slate-50 px-4 py-4 transition hover:-translate-y-0.5 hover:bg-primary-50 dark:bg-slate-950/60 dark:hover:bg-primary-950/45">
                <span class="flex h-11 w-11 shrink-0 items-center justify-center rounded-xl border border-slate-200 bg-white dark:border-slate-700 dark:bg-slate-900">
                  <ModelIcon :model="model.icon" size="24px" />
                </span>
                <div class="min-w-0">
                  <p class="truncate font-bold text-slate-900 dark:text-white">{{ model.name }}</p>
                  <p class="mt-0.5 text-xs text-slate-500 dark:text-slate-400">{{ model.provider }}</p>
                </div>
              </div>
            </div>

            <div class="rounded-2xl bg-primary-700 p-6 text-white shadow-[0_18px_50px_rgba(15,118,110,0.2)] dark:bg-primary-800 sm:p-8">
              <span class="flex h-11 w-11 items-center justify-center rounded-xl bg-white/14">
                <Icon name="key" size="lg" :stroke-width="2" />
              </span>
              <h3 class="mt-8 text-2xl font-black tracking-[-0.03em]">{{ t('home.market.models.oneKeyTitle') }}</h3>
              <p class="mt-3 leading-7 text-primary-50">{{ t('home.market.models.oneKeyDescription') }}</p>
            </div>
          </div>
        </div>
      </section>

      <section class="mx-auto max-w-7xl px-5 py-16 sm:px-8 lg:py-24">
        <div class="grid gap-12 lg:grid-cols-[0.78fr_1.22fr] lg:items-start lg:gap-20">
          <div class="lg:sticky lg:top-28">
            <h2 class="text-3xl font-black tracking-[-0.035em] text-slate-950 dark:text-white sm:text-4xl">
              {{ t('home.market.start.title') }}
            </h2>
            <p class="mt-4 max-w-md text-base leading-7 text-slate-600 dark:text-slate-300">
              {{ t('home.market.start.description') }}
            </p>
          </div>

          <div class="space-y-3">
            <div v-for="(item, index) in startItems" :key="item.title" class="start-row grid gap-4 rounded-2xl border border-slate-200 bg-white p-5 shadow-sm dark:border-slate-800 dark:bg-slate-900 sm:grid-cols-[56px_1fr_auto] sm:items-center sm:p-6">
              <span class="flex h-12 w-12 items-center justify-center rounded-xl bg-slate-100 text-base font-black text-slate-700 dark:bg-slate-800 dark:text-slate-200">
                {{ index + 1 }}
              </span>
              <div>
                <h3 class="text-lg font-bold text-slate-950 dark:text-white">{{ item.title }}</h3>
                <p class="mt-1 text-sm leading-6 text-slate-600 dark:text-slate-400">{{ item.description }}</p>
              </div>
              <Icon name="chevronRight" size="md" class="hidden text-slate-300 dark:text-slate-600 sm:block" />
            </div>
          </div>
        </div>

        <div class="mt-16 overflow-hidden rounded-2xl bg-slate-900 text-white dark:bg-slate-900">
          <div class="p-7 sm:p-9 lg:p-11">
            <span class="flex h-11 w-11 items-center justify-center rounded-xl bg-primary-400 text-slate-950">
              <Icon name="chat" size="lg" :stroke-width="2" />
            </span>
            <h2 class="mt-7 text-3xl font-black tracking-[-0.035em]">{{ t('home.market.help.title') }}</h2>
            <p class="mt-3 max-w-2xl leading-7 text-slate-300">{{ t('home.market.help.description') }}</p>
          </div>
        </div>
      </section>
    </main>

    <footer class="relative z-10 border-t border-slate-200 bg-white/60 dark:border-slate-800 dark:bg-slate-950">
      <div class="mx-auto flex max-w-7xl flex-col gap-4 px-5 py-8 text-sm text-slate-500 sm:flex-row sm:items-center sm:justify-between sm:px-8 dark:text-slate-400">
        <p>&copy; {{ currentYear }} {{ brandName }}. {{ t('home.footer.allRightsReserved') }}</p>
        <div class="flex items-center gap-5">
          <span>{{ t('home.market.footerTagline') }}</span>
          <a v-if="docUrl" :href="docUrl" target="_blank" rel="noopener noreferrer" class="font-semibold text-slate-700 transition hover:text-primary-700 dark:text-slate-200 dark:hover:text-primary-300">
            {{ t('home.docs') }}
          </a>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'
import { useI18n } from 'vue-i18n'
import { useAppStore, useAuthStore } from '@/stores'
import LocaleSwitcher from '@/components/common/LocaleSwitcher.vue'
import ModelIcon from '@/components/common/ModelIcon.vue'
import Icon from '@/components/icons/Icon.vue'
import { sanitizeUrl } from '@/utils/url'

const { t } = useI18n()
const authStore = useAuthStore()
const appStore = useAppStore()

const configuredSiteName = computed(() => appStore.cachedPublicSettings?.site_name || appStore.siteName || '')
const brandName = computed(() => {
  const configured = configuredSiteName.value.trim()
  return !configured || configured.toLowerCase() === 'sub2api' ? 'CheapAPI' : configured
})
const docUrl = computed(() => sanitizeUrl(appStore.cachedPublicSettings?.doc_url || appStore.docUrl || ''))
const homeContent = computed(() => appStore.cachedPublicSettings?.home_content || '')

const isHomeContentUrl = computed(() => {
  const content = homeContent.value.trim()
  return content.startsWith('http://') || content.startsWith('https://')
})

const isDark = ref(document.documentElement.classList.contains('dark'))
const isAuthenticated = computed(() => authStore.isAuthenticated)
const isAdmin = computed(() => authStore.isAdmin)
const dashboardPath = computed(() => isAdmin.value ? '/admin/dashboard' : '/dashboard')
const userInitial = computed(() => authStore.user?.email?.charAt(0).toUpperCase() || '')
const currentYear = computed(() => new Date().getFullYear())

const heroBenefits = computed(() => [
  t('home.market.benefits.noMonthlyFee'),
  t('home.market.benefits.noVpn'),
  t('home.market.benefits.freeSetup')
])

const supportedTools = computed(() => ['Codex', 'Claude Code', 'Gemini CLI', 'OpenClaw'])

const featuredModels = computed(() => [
  { name: 'OpenAI 5.6 Sol', provider: 'OpenAI', icon: 'gpt-5' },
  { name: 'Claude Fable 5', provider: 'Anthropic', icon: 'claude' },
  { name: 'Grok 4.5', provider: 'xAI', icon: 'grok' },
  { name: t('home.market.models.geminiSeries'), provider: 'Google', icon: 'gemini' }
])

const startItems = computed(() => [
  { title: t('home.market.start.register.title'), description: t('home.market.start.register.description') },
  { title: t('home.market.start.topUp.title'), description: t('home.market.start.topUp.description') },
  { title: t('home.market.start.createKey.title'), description: t('home.market.start.createKey.description') },
  { title: t('home.market.start.configure.title'), description: t('home.market.start.configure.description') }
])

function toggleTheme() {
  isDark.value = !isDark.value
  document.documentElement.classList.toggle('dark', isDark.value)
  localStorage.setItem('theme', isDark.value ? 'dark' : 'light')
}

function initTheme() {
  const savedTheme = localStorage.getItem('theme')
  if (savedTheme === 'dark' || (!savedTheme && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
    isDark.value = true
    document.documentElement.classList.add('dark')
  }
}

onMounted(() => {
  initTheme()
  authStore.checkAuth()
  if (!appStore.publicSettingsLoaded) {
    appStore.fetchPublicSettings()
  }
})
</script>

<style scoped>
.cheap-home {
  isolation: isolate;
}

.cheap-home__wash {
  background:
    radial-gradient(circle at 78% 24%, rgba(20, 184, 166, 0.13), transparent 34%),
    radial-gradient(circle at 18% 10%, rgba(14, 165, 233, 0.07), transparent 30%);
}

.offer-panel {
  transform: rotate(1deg);
  transition: transform 260ms cubic-bezier(0.16, 1, 0.3, 1);
}

.offer-panel:hover {
  transform: rotate(0deg) translateY(-4px);
}

.start-row {
  transition:
    transform 220ms cubic-bezier(0.16, 1, 0.3, 1),
    border-color 220ms ease;
}

.start-row:hover {
  transform: translateX(4px);
  border-color: rgba(20, 184, 166, 0.55);
}

@media (prefers-reduced-motion: no-preference) {
  .cheap-reveal {
    animation: cheap-reveal 650ms cubic-bezier(0.16, 1, 0.3, 1) both;
  }

  .cheap-reveal--delay {
    animation-delay: 100ms;
  }
}

@keyframes cheap-reveal {
  from {
    opacity: 0;
    transform: translateY(18px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (prefers-reduced-motion: reduce) {
  .offer-panel,
  .start-row {
    transition: none;
  }

  .offer-panel,
  .offer-panel:hover,
  .start-row:hover {
    transform: none;
  }
}

@media (max-width: 767px) {
  .offer-panel {
    transform: none;
  }
}
</style>
