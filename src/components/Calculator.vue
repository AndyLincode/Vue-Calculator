<template>
  <div class="pb-3 max-w-[400px] bg-[#31475e]  mx-auto">
    <input v-if="keyboard" type="text" v-model="calculatorValue" @keyup="(e) => handleKey(e.key)" placeholder="0"
      class="result w-full rounded  p-3 text-right leading-3 font-bold text-white bg-cyan-800 h-[36px]" readonly />
    <div v-else class="result w-full rounded  p-3 text-right leading-3 font-bold text-white bg-cyan-800">
      {{ + calculatorValue || 0}}
    </div>

  </div>

  <div class=" w-full">
    <div class="grid grid-cols-4 gap-3 ">
      <div v-for="el in calculatorElements" :key="el"
        class="text-white py-1 bg-cyan-800 rounded hover:cursor-pointer hover:bg-teal-600"
        :class="{ 'bg-sky-600': ['C', '*', '+', '-', '=', '%', '/'].includes(el) }" @click="action(el)">
        {{ el }}</div>
    </div>
  </div>
  <button class="btn-primary mt-2 p-3 text-xs" @click="handleSwitch">切換鍵盤模式</button>
  <br />
  <span v-if="keyboard" class=" text-xs w-2">請鎖定輸入框再輸入</span>
</template>

<script setup>
import { ref, reactive } from 'vue';
const calculatorElements = reactive(['C', '*', '/', '-', 7, 8, 9, '+', 4, 5, 6, '%', 1, 2, 3, '=', 0, '.']);
const calculatorValue = ref('');
const operator = ref(null);
const previousCalculatorValue = ref('');
const keyboard = ref(false);
// 滑鼠
const action = (el) => {
  // 輸入值
  if (!isNaN(el) || el === '.') {
    calculatorValue.value += el + '';
  }
  // 歸 0
  if (el === 'C') {
    calculatorValue.value = '';
  }

  // 百分比
  if (el === '%') {
    calculatorValue.value = calculatorValue.value / 100 + '';
  }

  // 
  if (['/', '*', '-', '+'].includes(el)) {
    operator.value = el;
    previousCalculatorValue.value = calculatorValue.value;
    calculatorValue.value = '';
  }

  if (el === '=') {
    calculatorValue.value = eval(
      previousCalculatorValue.value + operator.value + calculatorValue.value
    );

    previousCalculatorValue.value = '';
    operator.value = null;
  }

}
// 鍵盤
const handleKey = (e) => {
  // console.log(e);
  // 輸入值
  if (!isNaN(e) || e === '.') {
    calculatorValue.value += e + '';
  }
  // 歸 0
  if (e === 'C' || e === 'Backspace') {
    calculatorValue.value = '';
  }

  // 百分比
  if (e === '%') {
    calculatorValue.value = calculatorValue.value / 100 + '';
  }

  // 
  if (['/', '*', '-', '+'].includes(e)) {
    operator.value = e;
    previousCalculatorValue.value = calculatorValue.value;
    calculatorValue.value = '';
  }

  if (e === 'Enter') {
    calculatorValue.value = eval(
      previousCalculatorValue.value + operator.value + calculatorValue.value
    );

    previousCalculatorValue.value = '';
    operator.value = null;
  }
}

const handleSwitch = () => {
  keyboard.value = !keyboard.value
}

</script>

<style lang="scss" scoped>

</style>