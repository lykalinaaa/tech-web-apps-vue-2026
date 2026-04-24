<template>
  <div class="task3">
    <div class="input-block">
      <input
        type="text"
        v-model="kmVal"
        placeholder="Введите число"
      />
      <label>км/ч</label>
    </div>
    <div class="input-block">
      <input
        type="text"
        v-model="mVal"
        placeholder="Введите число"
      />
      <label>м/с</label>
    </div>
  </div>
</template>

<script setup>
import {ref, watch} from "vue";

const kmVal = ref();
const mVal = ref();

let isUpdating = false;

watch(kmVal, (newKmh) => {
  if (isUpdating) return;
  isUpdating = true;
  mVal.value = newKmh / 3.6;
  isUpdating = false;
});

watch(mVal, (newMs) => {
  if (isUpdating) return;
  isUpdating = true;
  kmVal.value = newMs * 3.6;
  isUpdating = false;
});
</script>

<style scoped>
.task3 {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

input {
  height: 40px;
  background-color: transparent;
  border: 3px solid #F6D5EE;
  border-radius: 12px;
  padding: 5px;
  color: #F6D5EE;
  margin-bottom: 5px;
}

input:focus {
  background-color: #F6D5EE;
  outline: none;
  color: #000000;
}

.input-block {
  display: flex;
  align-items: center;
  gap: 10px;
}

label {
  color: #F6D5EE;
}
</style>