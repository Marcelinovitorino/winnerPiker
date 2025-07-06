<template>
  <q-page class="flex flex-center">
    <div class="stage fullscreen"></div>
    <div :class="{ show: !showWinnerScreen }" class="curtain fullscreen"></div>

    <q-card class="add-names column bg-red" v-if="!showWinnerScreen">
      <q-card-section>
        <h2 class="text-center q-mt-none q-mb-md text-white text-bold">WINNER PICKER</h2>
        <q-input
          v-model="text"
          filled
          type="textarea"
          bg-color="white"
          placeholder="Enter names, one per line"
          rows="10"
          cols="40"
        />
        <div class="row justify-center q-mt-md">
          <q-btn color="primary" size="xl" @click="pickWinner">
            <q-icon name="check" />
            <span class="q-ml-sm">Pick a winner</span>
          </q-btn>
        </div>
      </q-card-section>
    </q-card>

    <div class="winner-screen column flex flex-center" v-if="showWinnerScreen">
      <h1 class="text-white text-h2 text-center">🎉 {{ winner }} 🎉</h1>
      <q-btn class="q-mt-xl" color="white" text-color="primary" @click="resetPicker">
        Try Again
      </q-btn>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'

const text = ref('')
const winner = ref('')
const showWinnerScreen = ref(false)

function pickWinner() {
  const names = text.value
    .split('\n')
    .map((name) => name.trim())
    .filter((name) => name.length > 0)

  if (names.length > 0) {
    const index = Math.floor(Math.random() * names.length)
    winner.value = names[index]
    showWinnerScreen.value = true
  } else {
    alert('Por favor, insira pelo menos um nome.')
  }
}

function resetPicker() {
  showWinnerScreen.value = false
  winner.value = ''
}
</script>

<style lang="scss">
.stage {
  background-image: url('/palco.jpg');
  background-size: cover;
  background-position: center;
  position: absolute;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.curtain {
  background-image: url('/cortina.jpg');
  background-size: cover;
  background-position: center;
  position: absolute;
  width: 100%;
  height: 100%;
  z-index: 2;
  transform: translateY(-100%);
  transition: transform 0.3s;

  &.show {
    transform: translateY(0%);
  }
}

.add-names {
  z-index: 3;
  position: relative;

  h2 {
    color: #e0dfdc;
    letter-spacing: 0.1em;
    text-shadow:
      0 -1px 0 #fff,
      0 1px 0 #2e2e2e,
      0 2px 0 #2c2c2c,
      0 3px 0 #2a2a2a,
      0 4px 0 #282828,
      0 5px 0 #262626,
      0 6px 0 #242424,
      0 7px 0 #222,
      0 8px 0 #202020,
      0 9px 0 #1e1e1e,
      0 10px 0 #1c1c1c,
      0 11px 0 #1a1a1a,
      0 12px 0 #181818,
      0 13px 0 #161616,
      0 14px 0 #141414,
      0 15px 0 #121212,
      0 22px 30px rgba(0, 0, 0, 0.9);
  }

  .q-textarea {
    textarea {
      font-size: 22px;
      line-height: 1.3em;
    }
  }
}

.winner-screen {
  z-index: 4;
  position: relative;
}
</style>
