<template>
  <div
    class="task2"
  >
    <div class="section">
      <label>Имя</label>
      <input
        type="text"
        placeholder="Введите имя"
        class="input input2"
        v-model="formData.name"
      />
    </div>
    <div class="section">
      <label>Фамилия</label>
      <input
        type="text"
        placeholder="Введите фамилию"
        class="input"
        v-model="formData.lastName"
      />
    </div>
    <div class="section">
      <label>Почта</label>
      <input
        type="email"
        placeholder="example@mail.ru"
        class="input"
        v-model="formData.email"
      />
      <span v-if="emailError && formData.email" class="error">
        {{ emailError }}
      </span>
    </div>
    <div class="section">
      <label>Телефон</label>
      <input
        type="text"
        placeholder="+7 (___) ___-__-__"
        class="input"
        v-model="formData.phone"
        @input="onPhoneInput"
      />
    </div>
    <button
      class="button transparent"
    >
      Отправить
    </button>
  </div>
</template>

<script setup>
import {computed, nextTick, ref} from "vue";

const formData = ref({
  name: '',
  lastName: '',
  email: '',
  phone: ''
})

const emailError = computed(() => {
  const email = formData.value.email
  if (!email) return ''

  const emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/

  if (!emailRegex.test(email)) {
    return 'Неверный формат email'
  }
  return ''
})

const formatPhone = (value) => {
  let numbers = value.replace(/\D/g, '')

  if (numbers.length > 11) {
    numbers = numbers.slice(0, 11)
  }

  if (numbers.length === 0) return ''

  if (numbers.length === 1) {
    return `+${numbers}`
  } else if (numbers.length <= 4) {
    return `+${numbers[0]} (${numbers.slice(1)}`
  } else if (numbers.length <= 7) {
    return `+${numbers[0]} (${numbers.slice(1, 4)}) ${numbers.slice(4)}`
  } else if (numbers.length <= 9) {
    return `+${numbers[0]} (${numbers.slice(1, 4)}) ${numbers.slice(4, 7)}-${numbers.slice(7)}`
  } else {
    return `+${numbers[0]} (${numbers.slice(1, 4)}) ${numbers.slice(4, 7)}-${numbers.slice(7, 9)}-${numbers.slice(9, 11)}`
  }
}

const onPhoneInput = (event) => {
  const cursorPosition = event.target.selectionStart
  const oldValue = formData.value.phone
  const newValue = formatPhone(event.target.value)

  formData.value.phone = newValue

  nextTick(() => {
    const diff = newValue.length - oldValue.length
    event.target.setSelectionRange(cursorPosition + diff, cursorPosition + diff)
  })
}
</script>

<style>
.task2 {
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding: 50px;
  border-radius: 18px;
  border: 3px solid #F6D5EE;
  width: 100%;
  max-width: 700px;
  min-width: 200px;
  margin: 30px 0;
}

.input2 {
  height: 40px;
  background-color: transparent;
  border: 3px solid #F6D5EE;
  border-radius: 12px;
  padding: 5px;
  color: #F6D5EE;
  margin-bottom: 5px;
}

.section {
  display: flex;
  flex-direction: column;
}
</style>