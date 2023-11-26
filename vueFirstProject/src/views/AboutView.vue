<script lang="ts" setup>
import { ref } from 'vue';
import calculator from '@/components/Games/Calculator.vue'

const counter = ref(0)

function plus() {
  counter.value++
}

</script>

<template>
  <!-- <div class="about">
    <h1>This is about a calculator</h1>
    <button class="myFirstVueButton" @click="plus">Нажми, когда что-то где-то как-то</button>
    <h2>You clicked me: {{ counter }}</h2>
  </div> -->

  <calculator/> 
</template>

<style lang="scss">
.about {
  min-height: 100vh;
  display: flex;
  align-items: center;
}

.myFirstVueButton {
  font-size: 20px;
  background: #7BE7FF;
  border-radius: 20px;
  color: #000000;
  background: linear-gradient(to right, #a223d1, #4631ce);
  border: none;
  transition: all 0.5s ease-in-out;
  font-family: "Comfortaa", cursive;
  font-size: 1rem;
  padding: 0.85em 1.345em;
  font-weight: bold;
  border-radius: 0.5em;
  margin: auto;
  letter-spacing: 2px;
  cursor: pointer;
  max-width: 10rem;

  &:hover {
    transform: translate(0, -5px);
    box-shadow: rgba(0, 0, 0, 0.25) 0px 14px 28px, rgba(0, 0, 0, 0.22) 0px 10px 10px;
    color: #7BE7FF;
  }
}
</style>
