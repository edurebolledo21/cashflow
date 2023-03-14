<script setup>
import Header from './Header.vue'
import Layout from './Layout.vue'
import Resume from './Resume/index.vue'
import Movements from './Movements/index.vue'
import Action from './Action.vue'
import Graphic from './Resume/Graphic.vue'
import { ref, computed, onMounted } from 'vue'

const amount = ref(null)
const label = ref(null)
const movements = ref([])

onMounted(() => {
  const data = JSON.parse(localStorage.getItem('movements'))
  if (data) {
    movements.value = data.map((m) => ({ ...m, time: new Date(m.time) }))
  }
})

const amounts = computed(() => {
  const lastDays = movements.value
    .filter((m) => {
      const today = new Date()
      const oldDate = today.setDate(today.getDate() - 30)
      return m.time > oldDate
    })
    .map((m) => m.amount)

  return lastDays.map((m, i) => {
    const lastMovemets = lastDays.slice(0, i + 1)

    return lastMovemets.reduce((suma, movement) => {
      return suma + movement
    }, 0)
  })
})

const create = (movement) => {
  movements.value.push(movement)
  save()
}
const remove = (id) => {
  movements.value = movements.value.filter((m) => m.id !== id)
  save()
}
const save = () => localStorage.setItem('movements', JSON.stringify(movements.value))

const totalAmount = computed(() =>
  movements.value.reduce((suma, movement) => suma + movement.amount, 0)
)

const select = (el) => {
  console.log(el)
  amount.value = el
}
</script>

<template>
  <Layout>
    <template #header>
      <Header> </Header>
    </template>
    <template #resume>
      <Resume :label="label" :amount="amount" :total-amount="totalAmount">
        <template #graphic> <Graphic :amounts="amounts" @select="select"></Graphic> </template>
        <template #action> <Action @create="create"></Action> </template>
      </Resume>
    </template>
    <template #movements>
      <Movements @remove="remove" :movements="movements"></Movements>
    </template>
  </Layout>
</template>
