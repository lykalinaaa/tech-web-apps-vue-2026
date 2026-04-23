<template>
  <div
    class="task"
  >
    <label>1.1</label>
    <div>
      <input
        type="text"
        placeholder="Введите число"
        class="input"
        :class="isValue1Error ? 'disabled' : ''"
        v-model="task1Value"
        :disabled="task1Result.isShown"
      />
      <button
        class="button transparent"
        @click="task1Analyze"
        :disabled="isValue1Error"
        :class="isValue1Error ? 'disabled' : ''"
      >
        {{ task1Result.isShown ? "Заново" : "Анализ" }}
      </button>
    </div>
    <label
      v-if="isValue1Error"
      class="error"
    >
      Некорректное значение!
    </label>
    <div v-if="task1Result.isShown">
      <p>Количество разрядов: {{ task1Result.digitsNum }}</p>
      <p>Первая цифра: {{ task1Result.firstDigit }}</p>
    </div>
  </div>
</template>

<script setup>
// Задание 1.1
import {computed, ref} from "vue";

const task1Value = ref('')
  const isValue1Error = computed(() => {
    return isNaN(Number(task1Value.value)) || (task1Value.value.length > 1 && task1Value.value.replace('-', '')[0] === '0') || task1Value.value.startsWith('+')
  })
  const task1Result = ref({
    isShown: false,
    digitsNum: 0,
    firstDigit: ''
  })
  const task1Analyze = () => {
    if (!task1Result.value.isShown && task1Value.value !== '') {
      task1Result.value.firstDigit = task1Value.value.replace('-', '')[0] || ''
      task1Result.value.digitsNum = Math.abs(parseFloat(task1Value.value)).toString().replace('.', '').length
      task1Result.value.isShown = true

      console.log('Количество разрядов: ', task1Result.value.digitsNum)
      console.log('Первая цифра: ', task1Result.value.firstDigit)
    } else {
      task1Result.value.firstDigit = ''
      task1Result.value.digitsNum = 0
      task1Result.value.isShown = false

      task1Value.value = ''
    }
  }
</script>

<style>
label, p {
  color: #F6D5EE;
  margin-bottom: 10px;
  font-size: 14px;
}

.input {
  width: 300px;
  height: 40px;
  background-color: transparent;
  border: 3px solid #F6D5EE;
  border-radius: 12px;
  padding: 5px;
  color: #F6D5EE;
  margin-bottom: 5px;
}

.input:focus {
  background-color: #F6D5EE;
  outline: none;
  color: #000000;
}

.button {
  background-color: #F6D5EE;
  padding: 12px 18px;
  border-radius: 12px;
  font-size: 14px;
  border: none;
}

.transparent {
  background-color: transparent;
  color: #F6D5EE;
  border: none;
}

.button:hover {
  cursor: pointer;
  background-color: #b28ca9;
}

.transparent:hover {
  color: #b28ca9;
  background-color: transparent;
}

.task {
  display: flex;
  flex-direction: column;
}

.error {
  color: #a12727;
}

.disabled {
  color: #b28ca9;
  cursor: default;
}
</style>