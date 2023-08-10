<script setup>
import { ref } from 'vue';
let score = ref(0);
let addSize = ref(1);
let totalClicks = ref(0);
let perSecond = ref(0);
let clickUpgradeCost = ref(10);
let secondUpgradeCost = ref(20);
let thirdUpgradeCost = ref(30)
let interval = null;

function addScore() {
  score.value += addSize.value;
  totalClicks.value++;
}
function increaseClick() {
  if (score.value >= clickUpgradeCost.value) {
    score.value -= clickUpgradeCost.value;
    addSize.value++;
    clickUpgradeCost.value = parseInt(clickUpgradeCost.value * 1.75)
  }
}
function startInterval() {
  interval = setInterval(() => {
    score.value += perSecond.value;
  },1000)
}
function firstIncrease() {
  if (score.value >= secondUpgradeCost.value) {
    clearInterval(interval);
    perSecond.value++;
    score.value -= secondUpgradeCost.value;
    secondUpgradeCost.value = parseInt(secondUpgradeCost.value * 2.15)
  }
  if(perSecond.value > 0) {
    startInterval();
  }
}
function secondIncrease() {
  if(score.value >= thirdUpgradeCost.value) {
    clearInterval(interval);
    perSecond.value = parseInt(perSecond.value + 5);
    score.value -= thirdUpgradeCost.value;
    thirdUpgradeCost.value = parseInt(thirdUpgradeCost.value * 3)
  }
  if(perSecond.value > 0) {
    startInterval();
  }
}
</script>

<template>
  <header>
  </header>

  <main>
<div class="stats">
<div class="totalClicks">{{ score }}</div>  
<div class="perSecond"> income: {{ perSecond }}</div>
</div>
<button class="click" @click="addScore()">HIT DMG: {{ addSize }}</button>
<div class="shop">
<button class="shopBuy" @click="increaseClick()">+1 on hit</button>
<span>Cost: {{ clickUpgradeCost }}</span>
<button class="shopBuy" @click="firstIncrease()">+1 hit pr. second</button>  
<span>Cost: {{ secondUpgradeCost }} </span>
<button class="shopBuy" @click="secondIncrease()">+5 hits pr. second</button>
<span>Cost: {{ thirdUpgradeCost }}</span>
</div>
  </main>
</template>
