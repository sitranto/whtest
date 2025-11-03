<script setup lang="ts">
import {computed, ref} from 'vue'

const props = defineProps({
  date: { type: Date, default: new Date(), required: false },
})

const monthNames = [
  'Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь',
  'Ноябрь', 'Декабрь'
]
const weekDayNames = [
  'Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс'
]

const date = ref(props.date)

const currentYear = ref(date.value.getFullYear())
const currentMonth = ref(date.value.getMonth())
const isCalendarOpened = ref(false)

const currentMonthName = computed(() => monthNames[currentMonth.value])
const daysInMonth = computed(() => {
  return new Date(currentYear.value, currentMonth.value + 1, 0).getDate()
})
const firstDayOfMonthIndex = computed(() => {
  const firstDayIndex =
    new Date(currentYear.value, currentMonth.value, 1).getDay()
  return (firstDayIndex + 6) % 7
})

</script>

<template>
  <div>
    <input
      type="text"
      @click="isCalendarOpened = true"
      readonly
      placeholder="Выберите дату"
    />
    <div v-if="isCalendarOpened" class="calendar">
      <div> <!-- Заголовок компонента -->
        <button> < </button>
        <span>{{ currentMonthName }} {{ currentYear }}</span>
        <button> > </button>
      </div>

      <div class="weekdays"> <!-- Строка с днями недели -->
        <div v-for="weekDayName in weekDayNames" :key="weekDayName">
          {{ weekDayName }}
        </div>
      </div>

      <div class="days"> <!-- Матрица с днями недели -->
        <div v-for="i in firstDayOfMonthIndex" :key="'empty-' + i"></div>
        <div
          v-for="day in daysInMonth"
          :key="day"
        >
          {{ day }}
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.calendar {
  width: 400px
}
.weekdays {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}
.days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}
</style>
