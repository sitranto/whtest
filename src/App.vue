<script setup lang="ts">
import {computed, ref} from "vue";
import Calendar from "@/Calendar.vue"

const currentLocale = ref<'ru' | 'en'>('ru')
const selectedDate = ref(null)

const formattedDate = computed(() => formatDate(selectedDate.value))

const formatDate = (date: Date | null) => {
  if (!date) return ''
  return date.toLocaleDateString("ru-RU")
}
</script>

<template>
  <label>
    <input type="radio" v-model="currentLocale" value="ru"> Русский
  </label>
  <label>
    <input type="radio" v-model="currentLocale" value="en"> English
  </label>

  <Calendar v-model:date="selectedDate" :locale="currentLocale" />
  <span>Выбранная дата: {{ formattedDate }}</span>
</template>

<style scoped></style>
