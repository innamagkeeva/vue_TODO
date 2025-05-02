<script setup lang="ts">
import { ref } from 'vue'
//создаю модель инпута высокого и низкого. изначально она пустая строка.

const inputTaskTextHigh = ref('')
const inputTaskTextLow = ref('')

// так как список задач - это массив - то нужно создать константу в которой будет пустой массив, в него будут пушиться задачи при нажатии на +
// const tasksTextHigh = ref<string[]>([])
// const tasksTextLow = ref<string[]>([])

//надо создать массив объектов с тремя полями: задача, статус(сделано/не сделано) и приоритет (высокий/низкий)

interface Task {
  text: string
  status: 'done' | 'notDone'
  priority: 'high' | 'low'
}

const tasks = ref<Task[]>([])

// function addTask(): void {
//   const textHigh = inputTaskTextHigh.value.trim()
//   const textLow = inputTaskTextLow.value.trim()
//   if (textHigh) {
//     const capitalizedHighText = textHigh[0].toUpperCase()+textHigh.slice(1)
//     tasksText.value.push({ text: capitalizedHighText, status: 'notDone', priority: 'high' })
//     inputTaskTextHigh.value = ''
//   }
// if (textLow) {
//   const capitalizedLowText = textLow[0].toUpperCase()+textLow.slice(1)
//   tasksText.value.push({ text: capitalizedLowText, status: 'notDone', priority: 'low' })
//   inputTaskTextLow.value = ''
// }
// }

function addTaskHigh(): void {
  const textHigh = inputTaskTextHigh.value.trim()
  if (textHigh) {
    const capitalizedHighText = textHigh[0].toUpperCase() + textHigh.slice(1)
    tasks.value.push({ text: capitalizedHighText, status: 'notDone', priority: 'high' })
    inputTaskTextHigh.value = ''
  }
}

function addTaskLow(): void {
  const textLow = inputTaskTextLow.value.trim()
  if (textLow) {
    const capitalizedLowText = textLow[0].toUpperCase() + textLow.slice(1)
    tasks.value.push({ text: capitalizedLowText, status: 'notDone', priority: 'low' })
    inputTaskTextLow.value = ''
  }
}

function changeStatus(task: Task): void {
  task.status = task.status === 'done' ? 'notDone' : 'done'
} // task.status === 'done' это true. если в задачи 'notDone' то это false и когда функция сработает, то 'notDone' поменяется на 'done'

function deleteTask(taskToDelete: Task): void {
  //тип Task потому что taskToDelete-объект.
  const index = tasks.value.findIndex((task) => task === taskToDelete)
  if (index !== -1) {
    tasks.value.splice(index, 1)
  }
}
</script>

<template>
  <div class="tasks">
    <h1 class="title">HIGH</h1>
    <!-- при нажатии на + сработала функция и текст из инпута попал в массив задач, но мне нужно чтоб создался новый ли для каждой задачи.Для этого я на li вешаю директиву vi-for которая будет проходиться по каждой задачи из массива  -->
    <form
      class="form"
      @submit.prevent="addTaskHigh"
    >
      <input
        class="form__input"
        type="text"
        name="task"
        required
        placeholder="Добавить важных дел"
        v-model="inputTaskTextHigh"
      />
      <button
        class="form__button"
        type="submit"
      >
        +
      </button>
    </form>
    <ul class="list">
      <li
        class="list__item"
        v-for="task in tasks.filter((task) => task.priority === 'high')"
        :key="task.text"
      >
        <input
          class="list__checkbox"
          type="checkbox"
          @click="changeStatus(task)"
          :checked="task.status === 'done'"
        />
        <p class="list__text">{{ task.text }}</p>
        <button
          class="list__button-delete"
          @click="deleteTask(task)"
        >
          +
        </button>
      </li>
    </ul>

    <h1 class="title">LOW</h1>
    <form
      class="form"
      @submit.prevent="addTaskLow"
    >
      <input
        class="form__input"
        type="text"
        name="task"
        required
        placeholder="Не очень важные дела"
        v-model="inputTaskTextLow"
      />
      <button
        class="form__button"
        type="submit"
      >
        +
      </button>
    </form>
    <ul class="list">
      <li
        class="list__item"
        v-for="task in tasks.filter((task) => task.priority === 'low')"
        :key="task.text"
      >
        <input
          class="list__checkbox"
          type="checkbox"
          @click="changeStatus(task)"
          :checked="task.status === 'done'"
        />
        <p class="list__text">{{ task.text }}</p>
        <button
          class="list__button-delete"
          @click="deleteTask(task)"
        >
          +
        </button>
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
  appearance: none;
  border: 1px solid black;
  border-radius: 50%;
  padding: 3px;
}

.list__checkbox:checked {
  background-color: #ccc;
  background-clip: content-box;
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
