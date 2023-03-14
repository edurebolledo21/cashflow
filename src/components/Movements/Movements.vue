<script setup>
import { computed, toRefs, defineEmits } from 'vue'
import { currencyFormater } from '../../utils'

const props = defineProps({
  title: String,
  id: Number,
  description: String,
  amount: Number
})

const { title, id, description, amount } = toRefs(props)

const remove = () => {
  emit('remove', id.value)
}

const amountCurrency = computed(() => currencyFormater.format(amount.value))

const emit = defineEmits(['remove'])
</script>

<template>
  <div class="movement">
    <div class="content">
      <h4>{{ title }}</h4>
      <p>{{ description }}</p>
    </div>
    <div class="action">
      <img src="@/assets/trash-icon.699e5aec.svg" alt="" @click="remove" />
      <p :class="amount < 0 ? 'red' : 'green'">{{ amountCurrency }}</p>
    </div>
  </div>
</template>

<style scoped>
.movement {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 16px;
  background-color: #e6f9ff;
  border-radius: 8px;
  box-sizing: border-box;
}
.movement .content {
  width: 100%;
}
.movement .action {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-direction: column;
}
h4,
p {
  margin: 0;
  padding: 0;
}
h4 {
  margin-bottom: 8px;
}
.movement .action img {
  margin-bottom: 16px;
}
.red {
  color: red;
}
.green {
  color: green;
}

.action > img {
  width: 30px;
}
</style>
