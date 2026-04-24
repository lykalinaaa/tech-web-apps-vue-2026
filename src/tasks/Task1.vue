<template>
  <div
    class="task"
  >
    <label class="label">1.1</label>
    <div class="input-box">
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
      class="error label"
    >
      Некорректное значение!
    </label>
    <div v-if="task1Result.isShown">
      <p class="result">Количество разрядов: {{ task1Result.digitsNum }}</p>
      <p class="result">Первая цифра: {{ task1Result.firstDigit }}</p>
    </div>
  </div>
  <div
    class="task"
  >
    <label class="label">1.2</label>
    <div class="input-box">
      <input
        type="text"
        placeholder="Введите строку"
        class="input"
        :class="task2Result.isShown ? 'disabled' : ''"
        v-model="task2Value"
        :disabled="task2Result.isShown"
      />
      <button
        class="button transparent"
        @click="task2Perform"
      >
        {{ task2Result.isShown ? "Заново" : "Вывести" }}
      </button>
    </div>
    <div v-if="task2Result.isShown">
      <p class="result">Каждый второй символ с конца: {{ task2Result.str }}</p>
    </div>
  </div>
  <div
    class="task"
  >
    <label class="label">1.3</label>
    <p class="result">Дана матрица:</p>
    <div class="matrix">
      <div
        v-for="(row, i) in matrixData"
        :key="i"
        class="matrix-row"
      >
        <span
          v-for="(item, j) in row"
          :key="j"
          class="matrix-cell"
        >
          {{ item }}
        </span>
      </div>
      <button
        class="button transparent"
        @click="matrixPerform"
      >
        Расчет
      </button>
    </div>
    <p class="result" v-if="matrixResult !== null">
      Сумма квадратов элементов = {{ matrixResult }}
    </p>
  </div>
</template>

<script setup>
  import {computed, ref} from "vue";

  // Задание 1.1
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

  // Задание 1.2
  const task2Value = ref('')
  const task2Result = ref({
    isShown: false,
    str: ''
  })
  const task2Perform = () => {
    if (task2Result.value.isShown) {
      task2Value.value = ''
      task2Result.value.str = ''
      task2Result.value.isShown = false
    } else {
      task2Result.value.str = task2Value.value.split('').reverse().filter((_, i) => i % 2 === 1).join(' ')
      task2Result.value.isShown = true

      console.log('Каждый второй символ с конца: ', task2Result.value.str)
    }
  }

  // Задание 1.3
  const matrixData = [
    [3, 7, 9, 11],
    [5, 8, 0, -4],
    [78, 87, 34, 0]
  ]
  const matrixResult = ref(null)

  const matrixPerform = () => {
    let sum = 0
    for (let i = 0; i < matrixData.length; i++) {
      for (let j = 0; j < matrixData[i].length; j++) {
        sum += matrixData[i][j] ** 2
      }
    }
    matrixResult.value = sum
    console.log('Сумма квадратов элементов = ', matrixResult.value)
  }

</script>

<style>
.label, .result {
  color: #F6D5EE;
  margin-bottom: 10px;
  font-size: 14px;
}

.input-box {
  display: flex;
}

.input {
  flex: 1;
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
  width: 500px;
}

.error {
  color: #a12727;
}

.matrix {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
}

.matrix-row {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
  justify-content: center;
}

.matrix-cell {
  display: inline-flex;
  justify-content: center;
  align-items: center;
  width: 60px;
  height: 40px;
  background-color: transparent;
  border: 2px solid #F6D5EE;
  border-radius: 12px;
  color: #F6D5EE;
  font-size: 14px;
}
</style>