<script setup>
import { ref, onMounted } from 'vue'
import Card from "./components/Card.vue"

const info = ref(null)
const name = ref('')
const selectedStatus = ref('')
const nextPageUrl = ref(null)
const prevPageUrl = ref(null)

async function getData(url) {
  try {
    const response = await fetch(url)
    const data = await response.json()
    info.value = data
    nextPageUrl.value = data.info.next
    prevPageUrl.value = data.info.prev
  } catch (error) {
    console.error('Ошибка при получении данных:', error)
  }
}

function findCharacters() {
  getData(`https://rickandmortyapi.com/api/character/?name=${name.value}&status=${selectedStatus.value}`)
}

onMounted(() => {
  getData('https://rickandmortyapi.com/api/character/')
})

function goToNextPage() {
  if (nextPageUrl.value) {
    getData(nextPageUrl.value)
  }
  if (nextPageUrl.value === null) {
    return
  }
}

function goToPrevPage() {
  if (prevPageUrl.value) {
    getData(prevPageUrl.value)
  }
}

</script>

<template>
  <div class="container">
    <div class="wrapper">
      <h1 className="title">The Rick and Morty</h1>
      <div class="input-wrapper">
        <label for="status-select">Choose by status:</label>
        <select id="status-select" class="select" v-model="selectedStatus">
          <option value="">- Select status -</option>
          <option value="alive">Alive</option>
          <option value="dead">Dead</option>
          <option value="unknown">Unknown</option>
        </select>
        <input type="text" class="input" v-model:="name" placeholder="Insert name">
        <button class="button" @click="findCharacters">Apply</button>
      </div>
    </div>

    <div class="cards-wrapper">
      <div class="buttons">
        <button class="button" @click="goToPrevPage" :class="{ 'disabled-button': !prevPageUrl }">Prev</button>
        <button class="button" @click="goToNextPage" :class="{ 'disabled-button': !nextPageUrl }">Next</button>
      </div>
      <div class="cards">
        <Card v-if="info != null" :info="info.results"></Card>
      </div>
    </div>
  </div>

</template>


<style scoped>
.container {
  width: 100%;
}

.wrapper {
  background-color: #fff;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.cards-wrapper {
  display: flex;
  flex-direction: column;
  padding: 30px 10px;
  align-items: center;
}

.cards {
  display: flex;
  gap: 20px;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
}

.title {
  font-size: 90px;
  text-align: center;
  margin-bottom: 20px;
}

.input {
  background-color: rgb(182, 182, 182);
  border-radius: 10px;
  border: 1px solid rgb(135, 135, 135);
  font-size: 14px;
  outline: none;
  padding: 10px;
  box-shadow: 5px 5px 10px rgba(0, 0, 0, 0.5);
}

.select {
  background-color: rgb(34, 144, 24);
  border-radius: 10px;
  border: 1px solid rgb(135, 135, 135);
  font-size: 14px;
  outline: none;
  padding: 10px;
  box-shadow: 5px 5px 10px rgba(0, 0, 0, 0.5);
  margin-right: 50px;
}

.input-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  column-gap: 10px;
  margin-bottom: 30px;
}

.button {
  background-color: rgb(202, 140, 18);
  border-radius: 10px;
  border: 1px solid rgb(175, 122, 15);
  font-size: 14px;
  outline: none;
  padding: 10px;
  box-shadow: 5px 5px 10px rgba(0, 0, 0, 0.5);
  cursor: pointer;
}

.disabled-button {
  background-color: rgb(119, 83, 11);
  border: 1px solid rgb(96, 67, 9);
  cursor: not-allowed;
}

.buttons {
  display: flex;
  column-gap: 10px;
  margin-bottom: 30px;

}
</style>
