<script setup>
import MyFirstCompnent from './components/MyFirstCompnent.vue'
import { ref } from 'vue'

const elements = ref([
  { msg: 'messae 1', id: 1 },
  { msg: 'messae 2', id: 2 },
  { msg: 'messae 3', id: 3 },
])

function deleteHandler(id) {
  let copy = elements.value
  let newElements = copy.filter((element) => element.id !== id)
  elements.value = newElements
}
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper"></div>
  </header>

  <main>
    <MyFirstCompnent
      v-for="element in elements"
      :msg="element.msg"
      :id="element.id"
      :key="element.id"
      @deleteClicked="deleteHandler"
    />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
