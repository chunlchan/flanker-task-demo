# Flanker Task Demo

A browser-based implementation of the Eriksen Flanker Task, built as a demo for the **Student Training & Engagement Program** at Florida International University, Miami FL.

## About the Task

The Flanker Task is a classic cognitive psychology paradigm used to measure selective attention and inhibitory control. Participants respond to the direction of a central arrow while ignoring surrounding "flanker" arrows that may point in the same (congruent) or opposite (incongruent) direction.

**Trial types:**
- **Congruent** — flankers match the target (e.g. `<<<<<`)
- **Incongruent** — flankers conflict with the target (e.g. `>><>>`)
- **Neutral** — flankers are non-directional (e.g. `--<--`)

Response time and accuracy are recorded for each trial.

## Tech Stack

- [Vue 3](https://vuejs.org/) + [Vite](https://vite.dev/)
- Trial stimuli loaded from `public/trials.json`

## Running Locally

```sh
npm install
npm run dev
```
