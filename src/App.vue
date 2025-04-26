<script setup lang="ts">
import { ref } from 'vue'
//создаю модель инпута высокого и низкого. изначально она пустая строка.

const inputTaskTextHigh = ref<string>('') //не знаю, тут надо типизировать?????!!!!!!!
const inputTaskTextLow = ref('')

// так как список задач - это массив - то нужно создать константу в которой будет пустой массив, в него будут пушиться задачи при нажатии на +
const tasksTextHigh = ref<string[]>([])
const tasksTextLow = ref<string[]>([])

function addTaskHigh(): void {
  const text = inputTaskTextHigh.value.trim()
  if (text) {
    const capitalizedText = text.charAt(0).toUpperCase() + text.slice(1)
    tasksTextHigh.value.push(capitalizedText)
    inputTaskTextHigh.value = ''
  }
}

function addTaskLow(): void {
  const text = inputTaskTextLow.value.trim()
  if (text) {
    const capitalizedText = text.charAt(0).toUpperCase() + text.slice(1)
    tasksTextLow.value.push(capitalizedText)
    inputTaskTextLow.value = ''
  }
}

function deleteTaskHigh(index: number): void {
  tasksTextHigh.value.splice(index, 1)
}

function deleteTaskLow(index: number): void {
  tasksTextLow.value.splice(index, 1)
}
</script>

<template>
  <div class="tasks">
    <h1 class="title">HIGH</h1>
    <!-- при нажатии на + сработала функция и текст из инпута попал в массив задач, но мне нужно чтоб создался новый ли для каждой задачи.Для этого я на li вешаю директиву vi-for которая будет проходиться по каждой задачи из массива  -->
    <form class="form" @submit.prevent="addTaskHigh">
      <input
        class="form__input"
        type="text"
        name="task"
        required
        placeholder="Добавить важных дел"
        v-model="inputTaskTextHigh"
      />
      <button class="form__button" type="submit">+</button>
    </form>
    <ul class="list">
      <li class="list__item" v-for="(task, index) in tasksTextHigh" :key="task">
        <input class="list__checkbox" type="checkbox" />
        <p class="list__text">{{ task }}</p>
        <button class="list__button-delete" @click="deleteTaskHigh(index)">+</button>
      </li>
    </ul>

    <h1 class="title">LOW</h1>
    <form class="form" @submit.prevent="addTaskLow">
      <input
        class="form__input"
        type="text"
        name="task"
        required
        placeholder="Не очень важные дела"
        v-model="inputTaskTextLow"
      />
      <button class="form__button" type="submit">+</button>
    </form>
    <ul class="list">
      <li class="list__item" v-for="(task, index) in tasksTextLow" :key="task">
        <input class="list__checkbox" type="checkbox" />
        <p class="list__text">{{ task }}</p>
        <button class="list__button-delete" @click="deleteTaskLow(index)">+</button>
      </li>
    </ul>
  </div>
</template>

<style>
input,
button,
li,
p {
  font-size: 25px;
  border: none;
  cursor: pointer;
  padding: 0;
  margin: 0;
}

button {
  background-color: white;
  width: 30px;
  height: 30px;
}

body {
  min-height: 100vh;
  background-color: gray;
  padding-top: 20px;
}

.tasks {
  max-width: 500px;
  height: 650px;
  margin: 0 auto;
  padding: 20px;
  background-color: white;
}

.title {
  width: 90px;
  height: 37px;
  margin: 0 auto 20px;
}

.form,
.list__item {
  max-width: 100%;
  height: 45px;
  border: 1px solid black;
  border-radius: 5px;
  margin-bottom: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 5px;
}

.form__input {
  width: 100%;
  height: 30px;
  outline: 0;
}

.list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.list__checkbox {
  width: 20px;
  height: 20px;
  margin-right: 10px;
}

.list__text {
  display: flex;
  flex-grow: 1;
  align-items: center;
  height: 30px;
}

.list__button-delete {
  rotate: 45deg;
}
</style>
