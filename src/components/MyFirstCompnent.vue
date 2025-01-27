<script setup>
defineProps({
  id: {
    type: Number,
    required: true,
  },
  msg: {
    type: String,
    required: true,
  },
})
defineEmits(['deleteClicked'])
</script>

<template>
  <div class="greetings">
    <img src="./icons/iconCommunity.svg" alt="" />
    <h1 class="green">{{ msg }}</h1>
    <button @click="$emit('deleteClicked', id)">delete</button>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
}

.greetings img {
  flex-shrink: 0;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
