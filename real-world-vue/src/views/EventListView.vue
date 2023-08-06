<script setup lang="ts">
import EventCard from '../components/EventCard.vue'
import EventCard2 from '../components/EventCard2.vue'
import type { EventItem } from '@/type'
import { ref, type Ref } from 'vue'
// import axios from 'axios'
import EventService from '@/services/EventService'

const events: Ref<Array<EventItem>> = ref([])

// axios.get<EventItem[]>('http://localhost:3004/events')
// .then((response) => {
//   events.value = response.data
// })
EventService.getEvent().then((response: { data: EventItem[] }) =>{
  events.value = response.data
})
</script>

<template>
  <h1>Events For Good</h1>
  <main class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event"></EventCard>
  </main>
  <!-- <mian class="events2">
    <EventCard2 v-for="event in events" :key="event.id" :event="event"></EventCard2>
  </mian> -->
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.events2 {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: right;
}
</style>
