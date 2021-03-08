<script setup lang="ts" >
import { Suspense, defineProps, onMounted, ref } from 'vue'
import { useRouter } from 'vue-router'
import { useI18n } from 'vue-i18n'

const name = ref('')
const router = useRouter()
const go = () => {
  if (name.value) router.push(`/hi/${encodeURIComponent(name.value)}`)
}
const { t } = useI18n()
const props = defineProps({
  message: String,
})

const users = ref([])
onMounted(async () => {
  const usersResponse = await fetch('https://reqres.in/api/users?delay=2')
  users.value = await usersResponse.json()
  return { users }
})

console.log('Vuetubers!!! This works')
</script>

<template>
  <Suspense>
    <div>
      <p class="text-4xl">
        <carbon-campsite class="inline-block" />
      </p>
      <p>
        <a
          rel="noreferrer"
          href="https://github.com/frandiox/vitesse-ssr-template"
          target="_blank"
        >
          Vitesse SSR
        </a>
      </p>
      <p>
        <em class="text-sm opacity-75">{{ t('intro.desc') }}</em>
      </p>

      <div class="py-4" />

      <input
        id="input"
        v-model="name"
        :placeholder="t('intro.whats-your-name')"
        :aria-label="t('intro.whats-your-name')"
        type="text"
        autocomplete="false"
        class="px-4 py-2 text-sm text-center bg-transparent border border-gray-200 rounded outline-none active:outline-none dark:border-gray-700"
        style="width: 250px"
        @keydown.enter="go"
      />
      <label class="hidden" for="input">{{ t('intro.whats-your-name') }}</label>

      <div>
        <button class="m-3 text-sm btn" :disabled="!name" @click="go">
          {{ t('button.go') }}
        </button>
      </div>
      <div v-for="user in users.data" :key="user.id">
        {{ user.first_name }}{{ user.last_name }}
      </div>
      Message from API: {{ props.message }}
    </div>
  </Suspense>
</template>

<route lang="yaml">
meta:
  layout: home
</route>
