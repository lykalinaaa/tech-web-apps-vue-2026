<template>
  <div class="wrapper">
    <h1>{{ taskTitle }}</h1>
    <Task1
      v-if="taskNumber === 1"
    />
    <Task2
      v-if="taskNumber === 2"
    />
    <Task3
      v-if="taskNumber === 3"
    />
    <div class="controls">
      <button
        v-if="taskNumber !== 1"
        class="button"
        @click="clickPrev"
      >
        Назад
      </button>
      <button
        v-if="taskNumber !== 3"
        class="button"
        @click="clickNext"
      >
        Далее
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import {computed, ref} from "vue";
import Task1 from "@/tasks/Task1.vue";
import Task2 from "@/tasks/Task2.vue";
import Task3 from "@/tasks/Task3.vue";

  const taskTitle = computed(() => {
    return 'Задание ' + taskNumber.value
  })

  const taskNumber = ref(1)

  const clickNext = () => {
    taskNumber.value++
  }

  const clickPrev = () => {
    taskNumber.value--
  }
</script>

<style scoped>
  .wrapper {
    background-color: #000000;
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-direction: column;
    min-height: 100vh;
    padding: 50px;
  }

  h1 {
    color: #F6D5EE;
    font-weight: 400;
  }

  .controls {
    display: flex;
    gap: 20px;
  }

  .button {
    background-color: #F6D5EE;
    padding: 12px 18px;
    border-radius: 12px;
    font-size: 14px;
    border: none;
    margin: auto;
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
</style>
