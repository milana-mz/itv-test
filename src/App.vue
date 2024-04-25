<template>
  <div class="container">
    <!-- ПЕРВОЕ ЗАДАНИЕ -->
    <section class="task-section">
      <h2 class="task-title">Первое задание</h2>
      <p><b>Заданный массив:</b> {{ inputData }}</p>
      <p><b>Результат:</b> {{ task1Result }}</p>
    </section>
    <!-- ВТОРОЕ ЗАДАНИЕ -->
    <section class="task-section">
      <h2 class="task-title">Второе задание</h2>
      <form @submit.prevent="convertNumber()" class="form">
        <input
          v-model="enteredNumber"
          type="text"
          placeholder="Введите число..."
          class="input-field"
        />
        <span class="result">{{ task2Result }}</span>
      </form>
    </section>
  </div>
</template>

<script setup>
import { ref } from "vue";

const task1Result = ref();
const task2Result = ref();
const enteredNumber = ref();
const alphabet = "abcdefghijklmnopqrstuvwxyz";
const inputData = [
  { id: 0, parent_id: null },
  { id: 1, parent_id: 2 },
  { id: 2, parent_id: 0 },
  { id: 4, parent_id: 1 },
  { id: 5, parent_id: 1 },
  { id: 6, parent_id: null },
  { id: 7, parent_id: 6 },
];

// ПЕРВОЕ ЗАДАНИЕ
function createTree(array, parentId = null) {
  const result = array
    .filter((item) => item.parent_id === parentId)
    .map((item) => ({ ...item, childs: createTree(array, item.id) }));
  result.map((item) => delete item["parent_id"]);
  return result;
}

task1Result.value = createTree(inputData);

// ВТОРОЕ ЗАДАНИЕ
function convertNumber() {
  const letters = alphabet.split("");
  const countIndex = Math.floor(enteredNumber.value / 26);
  const letterIndex = enteredNumber.value % 26;
  if (countIndex === 0) {
    task2Result.value = letters[letterIndex - 1].toUpperCase();
  } else if (countIndex === 1 && letterIndex === 0) {
    task2Result.value = letters[25].toUpperCase();
  } else if (countIndex >= 1 && letterIndex === 0) {
    task2Result.value = `${letters[countIndex - 1].toUpperCase()}${letters[25].toUpperCase()}`;
  } else {
    task2Result.value = `${letters[countIndex - 1].toUpperCase()}${letters[letterIndex - 1].toUpperCase()}`;
  }
}
</script>

<style scoped>
.container {
  height: 100vh;
  display: grid;
  gap: 20px;
  text-align: center;
}

.task-section {
  padding: 20px;
  background-color: #d1f5d1; /* light green */
}

.task-title {
  font-weight: bold;
  font-size: 42px;
  margin-bottom: 10px;
}

.form {
  display: flex;
  flex-direction: column;
}

.input-field {
  outline: none;
  padding: 15px;
  margin-bottom: 10px;
  border-radius: 10px;
}

.result {
  font-weight: bold;
  font-size: 20px;
}
</style>