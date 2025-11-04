<script setup lang="ts">
import {computed, ref} from "vue";
import Calendar from "@/Calendar.vue"

const selectedDate = ref(null)

const formattedDate = computed(() => formatDate(selectedDate.value))

const formatDate = (date: Date | null) => {
  if (!date) return ''
  return date.toLocaleDateString("ru-RU")
}
</script>

<template>
  <Calendar v-model:date="selectedDate" />
  <span>Выбранная дата: {{ formattedDate }}</span>
</template>

<style scoped></style>
