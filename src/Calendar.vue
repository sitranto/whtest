<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps({
  date: { type: Date, default: new Date(), required: false },
})

const date = ref(props.date)
const isCalendarOpened = ref(false)

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
        <button />
        <span></span>
        <button />
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
