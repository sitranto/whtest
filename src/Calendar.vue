<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import type { Locale } from '@/types/main'

const locales = {
  ru: {
    months: [
      'Январь',
      'Февраль',
      'Март',
      'Апрель',
      'Май',
      'Июнь',
      'Июль',
      'Август',
      'Сентябрь',
      'Октябрь',
      'Ноябрь',
      'Декабрь',
    ],
    weekdays: ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс'],
  },
  en: {
    months: [
      'January',
      'February',
      'March',
      'April',
      'May',
      'June',
      'July',
      'August',
      'September',
      'October',
      'November',
      'December',
    ],
    weekdays: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
  },
}

const emit = defineEmits<{
  (e: 'update:date', value: Date): void
}>()

const props = defineProps({
  date: { type: [String, Date, null], default: null },
  locale: { type: String as () => Locale, default: 'ru' },
})

const date = ref(new Date())

watch(
  () => props.date,
  (newDate) => {
    let d: Date | null = null
    if (typeof newDate == 'string') {
      d = new Date(newDate)
    } else if (newDate instanceof Date) {
      d = new Date(newDate)
    }

    if (d && !isNaN(d.getTime())) {
      date.value = d
    } else {
      date.value = new Date()
    }
  },
  { immediate: true },
)

const monthNames = computed(() => locales[props.locale].months)
const weekDayNames = computed(() => locales[props.locale].weekdays)
const currentYear = computed(() => date.value.getFullYear())
const currentMonth = computed(() => date.value.getMonth())
const currentMonthName = computed(() => monthNames.value[currentMonth.value])
const daysInMonth = computed(() => {
  return new Date(currentYear.value, currentMonth.value + 1, 0).getDate()
})
const firstDayOfMonthIndex = computed(() => {
  const firstDayIndex = new Date(currentYear.value, currentMonth.value, 1).getDay()
  return (firstDayIndex + 6) % 7
})

const nextMonth = () => {
  date.value = new Date(currentYear.value, currentMonth.value + 1, 1)
}

const prevMonth = () => {
  date.value = new Date(currentYear.value, currentMonth.value - 1, 1)
}

const selectDay = (day: number) => {
  const selectedDate = new Date(currentYear.value, currentMonth.value, day)
  emit('update:date', selectedDate)
}
</script>

<template>
  <div>
    <div class="calendar">
      <div class="header">
        <!-- Заголовок компонента -->
        <button @click="prevMonth" class="button"><</button>
        <span>{{ currentMonthName }} {{ currentYear }}</span>
        <button @click="nextMonth" class="button">></button>
      </div>

      <div class="weekdays">
        <!-- Строка с днями недели -->
        <div v-for="weekDayName in weekDayNames" :key="weekDayName" class="weekday">
          {{ weekDayName }}
        </div>
      </div>

      <div class="days">
        <!-- Матрица с днями недели -->
        <div v-for="i in firstDayOfMonthIndex" :key="'empty-' + i"></div>
        <div v-for="day in daysInMonth" :key="day" @click="selectDay(day)" class="day">
          {{ day }}
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.calendar {
  width: max-content;
  border: gray solid 1px;
  padding: 4px;
  height: max-content;
}
.header {
  display: flex;
  justify-content: space-between;
  padding-top: 4px;
  padding-bottom: 4px;
}
.button {
  background: transparent;
  border: none;
}
.button:hover {
  cursor: pointer;
}
.weekdays {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  padding: 4px;
}
.weekday {
  padding: 2px;
  display: flex;
  justify-content: center;
}
.days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-template-rows: repeat(6, 1fr);
}
.day {
  padding: 4px;
  display: flex;
  justify-content: center;
  border: transparent solid 1px;
}
.day:hover {
  cursor: pointer;
  border: aqua solid 1px;
}
</style>
