<template>
  <div class="page">
    <canvas id="confetti"></canvas>
    <div class="page__inner">
      <h1 class="title">Do you want some hugs?</h1>
      <p class="subtitle">Just take it =)</p>
      <img class="hugs-img" :src="hupImgComputed" alt="HUGS!" />
      <button type="button" @click="refreshHugs" class="hugs-button">Get hugs</button>
    </div>
  </div>
</template>

<script lang="ts" setup>
import JSConfetti from 'js-confetti'
import { computed, onMounted, ref } from 'vue'

// Get random number
const getRandomInt = (min: number, max: number) => {
  min = Math.ceil(min)
  max = Math.floor(max)
  return Math.floor(Math.random() * (max - min + 1)) + min
}

// Handle shake event
const handleMotion = (event: DeviceMotionEvent) => {
  const acceleration = event.accelerationIncludingGravity

  const accelerationMagnitude = Math.sqrt(
    Math.pow(acceleration?.x ?? 0, 2) +
      Math.pow(acceleration?.y ?? 0, 2) +
      Math.pow(acceleration?.z ?? 0, 2)
  )

  const shakeThreshold = 30

  if (accelerationMagnitude > shakeThreshold) {
    refreshHugs()
  }
}

onMounted(() => {
  if (window.DeviceMotionEvent) {
    window.addEventListener('devicemotion', handleMotion)
  }
})

const canvas: HTMLCanvasElement = document.getElementById('confetti') as HTMLCanvasElement

const jsConfetti = new JSConfetti({ canvas })

const hugNumber = ref(1)
const emojiArray = ref(['❤️', '🤗', '🐶', '😺', '🐀', '🥰', '🦊'])

const refreshHugs = () => {
  hugNumber.value = getRandomInt(1, 22)
  const emoji = emojiArray.value[getRandomInt(0, emojiArray.value.length)]
  navigator.vibrate([10, 100, 30])
  jsConfetti.addConfetti({
    emojis: [emoji],
    emojiSize: 100,
    confettiNumber: 60
  })
}
const hupImgComputed = computed(() => {
  return `/img/hugs/hugs-${hugNumber.value}.webp`
})
</script>

<style lang="scss">
#confetti {
  width: 100%;
  height: 100vh;
  position: absolute;
  overflow: hidden;
  top: 0;
  left: 0;
  z-index: -1;
}

.page {
  padding-left: 30px;
  padding-right: 30px;
  overflow: hidden;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;

  &__inner {
    text-align: center;
  }
}

.title {
  font-size: 40px;
}

.subtitle {
  font-size: 30px;
}

.button {
  position: relative;
  margin: 20px;
}

.hugs-button {
  color: white;
  font-weight: bold;
  font-size: 30px;
  text-align: center;
  text-decoration: none;
  background-color: #07485b;
  display: block;
  position: relative;
  padding: 20px 30px;
  width: 100%;
  max-width: 400px;

  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
  text-shadow: 0 1px 0 #000;
  filter: dropshadow(color=#000, offx=0px, offy=1px);

  -webkit-box-shadow: inset 0 1px 0 #8ec7d2, 0 10px 0 #8ec7d2;
  -moz-box-shadow: inset 0 1px 0 #8ec7d2, 0 10px 0 #8ec7d2;
  box-shadow: inset 0 1px 0 #8ec7d2, 0 10px 0 #8ec7d2;

  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  border-radius: 5px;

  &:active {
    top: 10px;
    background-color: #0d6986;

    -webkit-box-shadow: inset 0 1px 0 #8ec7d2, inset 0 -3px 0 #8ec7d2;
    -moz-box-shadow: inset 0 1px 0 #8ec7d2, inset 0 -3pxpx 0 #8ec7d2;
    box-shadow: inset 0 1px 0 #8ec7d2, inset 0 -3px 0 #8ec7d2;
  }

  &:after {
    content: '';
    height: 100%;
    width: 100%;
    padding: 4px;
    position: absolute;
    bottom: -15px;
    left: -4px;
    z-index: -1;
    background-color: #2b1800;
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    border-radius: 5px;
  }
}

.hugs-img {
  width: 300px;
  height: 250px;
  object-fit: cover;
  -webkit-border-radius: 16px;
  -moz-border-radius: 16px;
  border-radius: 16px;
  margin-bottom: 32px;
}
</style>
