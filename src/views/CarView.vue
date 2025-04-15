<script setup>
import { useRoute, useRouter, RouterView } from 'vue-router'
import { onMounted, ref } from 'vue'
import cars from '../data.json'

const route = useRoute()
const router = useRouter()
const car = ref(null)
const { id } = route.params

onMounted(() => {
  const c = cars.find((c) => c.id === parseInt(id))
  car.value = c
})
</script>

<template>
  <div class="container">
    <div v-if="car">
      <h1>The Car</h1>
      <p>Make: {{ car.make }}</p>
      <p>Body: {{ car.body }}</p>
      <p>Price: {{ car.price }}</p>
      <p>Year: {{ car.year }}</p>
      <div class="btn">
        <button @click="router.push(`/car/${id}/dealer`)">Contact Dealer</button>
        <button @click="router.back()">Go Back</button>
        <button @click="router.push(`/car/${id}/manufactorer`)">Contact manufactorer</button>
      </div>
      <RouterView />
    </div>
    <div v-else>
      <h1>Car not found</h1>
    </div>
  </div>
</template>

<style scoped>
.container {
  margin: 10px;
  padding: 5px;
}

.container h1 {
  text-align: center;
}

.container p {
  text-align:center;
}

.btn{
    display: flex;
    justify-content: space-around;
    margin-top: 100px
}

.btn button{
    color:white;
    background: blue;
    border:none;
    padding: 6px 12px;
    margin:12px 4px;
    border-radius: 5px;
}

.btn button:hover{
    background: yellow;
    color:black;
    cursor: pointer;
}
</style>
