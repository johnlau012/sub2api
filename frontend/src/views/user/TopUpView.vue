<template>
  <AppLayout>
    <div class="mx-auto max-w-4xl space-y-6">
      <header class="text-center">
        <p class="text-sm font-medium uppercase tracking-[0.18em] text-primary-600 dark:text-primary-400">CheapAPI</p>
        <h1 class="mt-2 text-3xl font-bold text-gray-900 dark:text-white">{{ t('topUp.title') }}</h1>
        <p class="mt-2 text-sm text-gray-500 dark:text-gray-400">{{ t('topUp.description') }}</p>
      </header>

      <div class="grid gap-6 lg:grid-cols-[minmax(0,1fr)_minmax(280px,360px)]">
        <section class="card overflow-hidden">
          <div class="border-b border-gray-100 p-2 dark:border-dark-700">
            <div class="grid grid-cols-3 gap-1 rounded-xl bg-gray-100 p-1 dark:bg-dark-800">
              <button
                v-for="method in methods"
                :key="method.key"
                type="button"
                class="rounded-lg px-2 py-3 text-sm font-semibold transition"
                :class="activeKey === method.key ? `${method.activeClass} bg-white shadow-sm dark:bg-dark-700` : 'text-gray-500 hover:text-gray-900 dark:text-gray-400 dark:hover:text-white'"
                @click="activeKey = method.key"
              >
                {{ method.label }}
              </button>
            </div>
          </div>

          <div class="p-6 text-center sm:p-8">
            <div class="mx-auto max-w-[360px] rounded-2xl bg-white p-4 shadow-sm ring-1 ring-gray-100 dark:ring-dark-600">
              <img :src="activeMethod.image" :alt="`${activeMethod.label} QR Code`" class="mx-auto aspect-square w-full object-contain" />
            </div>
            <p class="mt-4 text-sm font-medium text-gray-700 dark:text-gray-200">{{ activeMethod.label }}</p>
            <p class="mt-1 text-sm text-gray-500 dark:text-gray-400">{{ t('topUp.scanHint') }}</p>
          </div>
        </section>

        <aside class="card flex flex-col justify-between p-6">
          <div>
            <div class="flex h-11 w-11 items-center justify-center rounded-2xl bg-primary-50 text-primary-600 dark:bg-primary-900/30 dark:text-primary-300">
              <Icon name="creditCard" size="lg" />
            </div>
            <h2 class="mt-5 text-lg font-bold text-gray-900 dark:text-white">{{ t('topUp.stepsTitle') }}</h2>
            <p class="mt-2 text-sm text-gray-500 dark:text-gray-400">{{ t('topUp.dmHint') }}</p>
            <ol class="mt-4 space-y-3 text-sm text-gray-700 dark:text-gray-200">
              <li v-for="(item, index) in details" :key="item" class="flex gap-3">
                <span class="flex h-6 w-6 flex-shrink-0 items-center justify-center rounded-full bg-gray-100 text-xs font-bold text-gray-600 dark:bg-dark-700 dark:text-gray-300">{{ index + 1 }}</span>
                <span>{{ item }}</span>
              </li>
            </ol>
            <p class="mt-5 rounded-xl bg-gray-50 p-3 text-xs leading-5 text-gray-500 dark:bg-dark-800 dark:text-gray-400">{{ t('topUp.manualNote') }}</p>
          </div>
          <a class="btn btn-primary mt-6 w-full" :href="paymentConfig?.manual_top_up_contact_url || 'https://carousell.app.link/GrHNniPpd5b'" target="_blank" rel="noopener noreferrer">
            <Icon name="link" size="md" class="mr-2" />
            {{ t('topUp.dmButton') }}
          </a>
        </aside>
      </div>
    </div>
  </AppLayout>
</template>

<script setup lang="ts">
import { computed, ref, onMounted } from 'vue'
import { useI18n } from 'vue-i18n'
import AppLayout from '@/components/layout/AppLayout.vue'
import Icon from '@/components/icons/Icon.vue'
import { paymentAPI } from '@/api/payment'
import type { PaymentConfig } from '@/types/payment'

const { t } = useI18n()
const activeKey = ref('payme')
const paymentConfig = ref<PaymentConfig | null>(null)

const methods = computed(() => [
  { key: 'payme', label: t('topUp.payMe'), image: paymentConfig.value?.manual_top_up_payme_url || '/top-up-payme.jpg', activeClass: 'text-red-600 dark:text-red-400' },
  { key: 'alipay', label: t('topUp.alipay'), image: paymentConfig.value?.manual_top_up_alipay_url || '/top-up-alipayhk.jpg', activeClass: 'text-indigo-600 dark:text-indigo-400' },
  { key: 'wechat', label: t('topUp.wechat'), image: paymentConfig.value?.manual_top_up_wechat_url || '/top-up-wechatpayhk.jpg', activeClass: 'text-emerald-600 dark:text-emerald-400' }
])

const activeMethod = computed(() => methods.value.find((method) => method.key === activeKey.value) || methods.value[0])
const details = computed(() => [t('topUp.amount'), t('topUp.username'), t('topUp.screenshot')])

onMounted(async () => {
  try {
    paymentConfig.value = (await paymentAPI.getConfig()).data
  } catch {
    // Keep the bundled QR fallback available when the public config is unavailable.
  }
})
</script>
