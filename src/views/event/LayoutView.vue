<script setup lang="ts">
import { useEventStore } from '@/stores/event'
import { storeToRefs } from 'pinia'

const store = useEventStore()
const { event } = storeToRefs(store)
</script>
<template>
  <div v-if="event">
    <h1>{{ event.title }}</h1>
    <nav class="flex justify-center space-x-4 py-4">
        <RouterLink class="text-blue-500 hover:text-blue-700 font-semibold"
        :to="{ name: 'event-detail-view' }">Details</RouterLink>
        <RouterLink class="text-blue-500 hover:text-blue-700 font-semibold"
        :to="{ name: 'event-register-view' }">Register</RouterLink>
        <RouterLink class="text-blue-500 hover:text-blue-700 font-semibold"
        :to="{ name: 'event-edit-view' }">Edit</RouterLink>
    </nav>
      <RouterView :event="event" />
  </div>
</template>
