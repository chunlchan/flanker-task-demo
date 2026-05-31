Build a minimal Eriksen Flanker task as a Vue 3 SPA (Composition API, <script setup>).

You are working in an existing skeleton Vue app created with `npm create vue@latest`.
Modify src/App.vue in place ? clear its default contents and replace entirely.
Do not create new components or modify any other files except where noted below.
Do no install other dependencies.

## Setup
In index.html add to <head>:
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@picocss/pico@2/css/pico.min.css">

## public/trials.json
Create this file. Flat array of 12 pre-shuffled trial objects:
  { "id": 1, "type": "congruent", "direction": "left", "stimulus": "<<<<<" }

Three conditions (5-char monospace):
  congruent left  <<<<<   congruent right  >>>>>
  incongruent left  >><>>   incongruent right  <<><<
  neutral left  --<--   neutral right  -->--

4 trials per condition, 2 left + 2 right each, pre-shuffled.

## src/App.vue ? 4 phases only
loading ? instructions ? fixation ? stimulus ? (loop) ? complete

State:
  const phase = ref('loading')
  const trials = ref([])
  const index = ref(0)
  const stimStart = ref(null)
  const responded = ref(false)

Flow:
1. onMounted: fetch('/trials.json'), store in trials, set phase = 'instructions'
2. "Begin" button ? phase = 'fixation'
3. Fixation: show + for 500ms via setTimeout, then phase = 'stimulus',
   stimStart = performance.now(), responded = false
   (reset responded here in the setTimeout callback, not in the response handler)
4. Stimulus: show trials[index].stimulus + two click buttons
5. On click: if responded return; responded = true; record rt = performance.now() - stimStart;
   if index + 1 < trials.length: 
      - Call startFixation() to move to fixation state and then to stimulus state
   else phase = 'complete'
6. Show Fixation again for 500ms before moving on to next trial. Loop through all trials before moving on to next step.
7. Complete: show a simple success message

## Template structure
- Instructions screen: brief task description + "Begin" <button>
- Fixation screen: large + character
- Stimulus screen: large monospace stimulus string above two <button class="secondary"> side by side
- Complete screen: <h2>Task complete!</h2>

## Styling (scoped, minimal ? lean on PicoCSS for buttons/typography)
.app  ? min-height:100vh; display:flex; align-items:center; justify-content:center
.screen  ? flex column, align-items:center, gap:1rem, max-width:480px, text-align:center
.stim  ? font-family:monospace; font-size:clamp(3rem,10vw,5rem); letter-spacing:0.15em
.fixation  ? font-size:4rem
.buttons  ? display:flex; gap:1rem  (each button min-height:64px; min-width:120px)

No keyboard handling. No stats. No practice. Mouse clicks only.

Do not run `npm run dev` upon completion.