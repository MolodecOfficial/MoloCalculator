<script setup>

import {ref} from "vue";

const calculate = (prev, operator, current) => {
  const a = parseFloat(prev);
  const b = parseFloat(current);

  switch (operator) {
    case '+': return a + b;
    case '-': return a - b;
    case '*': return a * b;
    case '/':
      if (b === 0) throw new Error('Division by zero');
      return a / b;
    default: return current;
  }
};

const calcValue = ref('')
const calcButtons =  ['C', '%', '=', '+', 7, 8, 9,'-', 4, 5, 6,'*', 1, 2, 3, '/', 0, '.']
const calcOperator = ref(null)
const calcPrevValue = ref('')

const calcAction = (btn) => {

  if (!isNaN(btn) || btn === '.') {
    calcValue.value += btn;
  }

  if (btn === 'C') {
    calcValue.value = '';
    calcPrevValue.value = '';
    calcOperator.value = '';
  }
  if (btn === '%') {
    calcValue.value = (parseFloat(calcValue.value) / 100).toString();
  }

  if (['/', '+', '-', '*'].includes(btn)) {
    calcOperator.value = btn
    calcPrevValue.value = calcValue.value
    calcValue.value = ''
  }


  if (btn === "=") {
    try {
      calcValue.value = calculate(calcPrevValue.value, calcOperator.value, calcValue.value).toString();
    } catch (error) {
      calcValue.value = 'Error';
    }
    calcPrevValue.value = '';
    calcOperator.value = null;
  }
}

</script>

<template>
  <section>
    <div class="body">
      <div class="value">{{ calcValue }}</div>
      <div class="buttons">
        <button class="custom-button"
                :class="{'bg-vue-green': ['C', '*', '/', '+', '-', '=', '%'].includes(btn)}"
                v-for="btn in calcButtons"
                :key="btn"
                @click="calcAction(btn)">{{ btn }}</button>
      </div>
    </div>
  </section>
</template>

<style>

.body {
  background-color: #133748;
  display: flex;
  flex-direction: column;
  width: 15%;
  height: 44%;
  gap: 12px;
  justify-content: center;
  align-items: center;
  border-radius: 30px;
}
.value {
  background-color: #094c58;
  color: white;
  font-size: 20px;
  height: 50px;
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 20px;
  width: 90%;
}
.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}

.custom-button {
  font-weight: bold;
  color: white;
  text-align: center;
  margin: 0.25rem;
  padding-top: 1rem;
  padding-bottom: 1rem;
  background-color: #2c4d64;
  border-radius: 8px;
  border: none;
  transition: background-color 0.3s;
}

.custom-button:hover {
  background-color: #1c727d;
}

.bg-vue-green {
  background-color: #248e3f;
}

button {
  padding: 20px;
  margin: 2px;
  font-size: 18px;
}

</style>