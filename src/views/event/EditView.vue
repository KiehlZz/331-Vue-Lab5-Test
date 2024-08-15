<script setup lang="ts">
import { ref} from 'vue';
import { useRouter } from 'vue-router';
import { useMessageStore } from '@/stores/message';
import { type Event } from '@/types';

const props = defineProps<{ 
  event: Event
  id: String
}>()
// eslint-disable-next-line @typescript-eslint/no-unused-vars
const router = useRouter();
const store = useMessageStore();
const showMessage = ref(false);
const flashMessage = ref('');

const editEvent = () => {
  store.updateMessage('The data has been updated');
  setTimeout(() => {
    store.resetMessage();
  }, 3000);
  router.push({ name: 'event-detail-view', params: { id: props.event.id }});
};
</script>

<template>
  <div>
    <p class="text-lg font-semibold mb-4">Edit event here</p>
    <button @click="editEvent" 
    class="bg-blue-500 text-white font-bold py-2 px-4 rounded hover:bg-blue-700 focus:outline-none focus:ring-2
    focus:ring-blue-500 focus:ring-opacity-50">Edit</button>
    <div v-if="showMessage">
      {{ flashMessage }}
    </div>
  </div>
</template>
