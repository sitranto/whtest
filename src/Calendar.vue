<script setup lang="ts">
import {computed, ref} from 'vue'

const props = defineProps({
  date: { type: Date, default: new Date(), required: false },
})

const monthNames = [
  'Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь',
  'Ноябрь', 'Декабрь'
]

const date = ref(props.date)

const currentYear = ref(date.value.getFullYear())
const currentMonth = ref(date.value.getMonth())
const isCalendarOpened = ref(false)

const currentMonthName = computed(() => monthNames[currentMonth.value])

</script>

<template>
  <div>
    <input
      type="text"
      @click="isCalendarOpened = true"
      readonly
      placeholder="Выберите дату"
    />
    <div v-if="isCalendarOpened">
      <div> <!-- Заголовок компонента -->
        <button> < </button>
        <span>{{ currentMonthName }} {{ currentYear }}</span>
        <button> > </button>
      </div>

      <div> <!-- Строка с днями недели -->
        <div></div>
      </div>

      <div> <!-- Матрица с днями недели -->

      </div>
    </div>
  </div>
</template>

<style scoped></style>
