<template>
  <div class="wrapper">
    <h1>{{ taskTitle }}</h1>
    <Task1
      v-if="taskNumber === 1"
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
</style>
