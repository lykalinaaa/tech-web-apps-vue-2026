<template>
  <div
    class="task2"
  >
    <div class="toggle-container">
      <label class="toggle label">
        <input type="checkbox" v-model="isToggled">
        <span class="toggle-slider"></span>
      </label>
      <span class="toggle-label">{{ isToggled ? 'Темная' : 'Светлая' }}</span>
    </div>
    <div class="section">
      <label class="label">Имя</label>
      <input
        type="text"
        placeholder="Введите имя"
        class="input input2"
        v-model="formData.name"
      />
    </div>
    <div class="section">
      <label class="label">Фамилия</label>
      <input
        type="text"
        placeholder="Введите фамилию"
        class="input"
        v-model="formData.lastName"
      />
    </div>
    <div class="section">
      <label class="label">Почта</label>
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
      <label class="label">Телефон</label>
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
      @click="openPopup"
    >
      Отправить
    </button>
    <div
      class="popup"
      v-if="isPopupOpen"
    >
      <div class="popup-wrapper">
        <h2 class="popup-title">Ваши данные</h2>
        <div class="info-grid">
          <div class="grid-label">Имя:</div>
          <div class="grid-value">{{ formData.name || '-' }}</div>

          <div class="grid-label">Фамилия:</div>
          <div class="grid-value">{{ formData.lastName || '-' }}</div>

          <div class="grid-label">Почта:</div>
          <div class="grid-value">{{ formData.email || '-' }}</div>

          <div class="grid-label">Телефон:</div>
          <div class="grid-value">{{ formData.phone || '-' }}</div>
        </div>
        <button
          @click="closePopup"
          class="button"
        >
          Назад
        </button>
      </div>
    </div>
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

const isPopupOpen = ref(false)
const isToggled = ref(true)

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

const openPopup = () => {
  isPopupOpen.value = true
}

const closePopup = () => {
  isPopupOpen.value = false
}
</script>

<style>
.label {
  color: #F6D5EE;
  margin-bottom: 10px;
  font-size: 14px;
}
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

.popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.8);
  z-index: 1000;
  display: flex;
  justify-content: center;
  align-items: center;
}

.popup-wrapper {
  max-width: 600px;
  width: 100%;
  background-color: #F6D5EE;
  color: #000000;
  padding: 30px;
  border-radius: 18px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.info-grid {
  display: grid;
  width: 100%;
  margin: 20px 0;
  grid-template-columns: 100px 1fr;
  gap: 15px 20px;
}

.toggle-container {
  display: flex;
  align-items: center;
  gap: 15px;
}

.toggle input {
  display: none;
}

.toggle-slider {
  display: block;
  width: 60px;
  height: 30px;
  background-color: #444;
  border-radius: 30px;
  position: relative;
  cursor: pointer;
  transition: 0.3s;
}

.toggle-slider::before {
  content: '';
  position: absolute;
  width: 26px;
  height: 26px;
  background-color: #F6D5EE;
  border-radius: 50%;
  top: 2px;
  left: 2px;
  transition: 0.3s;
}

.toggle input:checked + .toggle-slider {
  background-color: #F6D5EE;
}

.toggle input:checked + .toggle-slider::before {
  transform: translateX(30px);
  background-color: #000000;
}

.toggle-label {
  color: #F6D5EE;
}
</style>