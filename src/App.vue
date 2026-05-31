<script setup>
import { ref, onMounted } from 'vue'

// Application phases: loading → instructions → fixation → stimulus → complete
const phase = ref('loading')
const trials = ref([])
const index = ref(0)
const stimStart = ref(null)
const responded = ref(false)

function startFixation() {
  phase.value = 'fixation'
  setTimeout(() => {
    phase.value = 'stimulus'
    stimStart.value = performance.now()
    responded.value = false
  }, 500)
}

function handleResponse(direction) {
  if (responded.value) return
  responded.value = true
  const rt = performance.now() - stimStart.value
  const correct = direction === trials.value[index.value].direction
  console.log(correct);
  trials.value[index.value] = { ...trials.value[index.value], rt, correct }
  if (index.value + 1 < trials.value.length) {
    index.value += 1
    startFixation()
  } else {
    phase.value = 'complete'
  }
}

onMounted(async () => {
  try {
    const res = await fetch('/trials.json')
    trials.value = await res.json()
    phase.value = 'instructions'
  } catch (e) {
    console.error('Failed to load trials', e)
    phase.value = 'complete'
  }
})
</script>

<template>
  <div class="app">
    <div class="screen" v-if="phase === 'loading'">
      <p>Loading...</p>
    </div>

    <div class="screen" v-else-if="phase === 'instructions'">
      <h2>Flanker Task</h2>
      <p>Click the button corresponding to the direction of the central arrow.</p>
      <button @click="startFixation">Begin</button>
    </div>

    <div class="screen" v-else-if="phase === 'fixation'">
      <div class="fixation">+</div>
    </div>

    <div class="screen" v-else-if="phase === 'stimulus'">
      <div class="stim">{{ trials[index].stimulus }}</div>
      <div class="buttons">
        <button class="secondary" @click="handleResponse('left')">left</button>
        <button class="secondary" @click="handleResponse('right')">right</button>
      </div>
    </div>

    <div class="screen" v-else-if="phase === 'complete'">
      <h2>Task complete!</h2>
    </div>
  </div>
</template>

<style scoped>
.app {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
.screen {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  max-width: 480px;
  text-align: center;
}
.stim {
  font-family: monospace;
  font-size: clamp(3rem, 10vw, 5rem);
  letter-spacing: 0.15em;
}
.fixation {
  font-size: 4rem;
}
.buttons {
  display: flex;
  gap: 1rem;
}
.buttons button {
  min-height: 64px;
  min-width: 120px;
}
</style>
