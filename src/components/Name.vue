<script setup lang="ts">
import { onMounted, ref } from 'vue'

const users = ref([])
onMounted(async () => {
  const usersResponse = await fetch('https://reqres.in/api/users?delay=2')
  users.value = await usersResponse.json()
  return { users }
})
console.log('Vuetubers!!! This works')
</script>

<template>
  <div class="mt-5 mx-auto text-center opacity-25 text-sm">
    <div v-for="user in users.data" :key="user.id">
      {{ user.first_name }}{{ user.last_name }}
    </div>
  </div>
</template>
