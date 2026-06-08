# Synth Superpowers Test

A small browser synth repo for testing whether the `obra/superpowers` workflow helps an AI coding agent build synth software without drifting into a generic overbuilt app.

## Purpose

This repo is a controlled test bed.

It is **not** the main Merrin Voice 01 repo.

Use it to test:

- spec-first synth development
- small versioned issues
- Web Audio implementation discipline
- build/check/review habits
- listening checkpoints after code changes

## Current target

Build **v0.1 only**:

- browser app shell
- audio unlock button
- one playable sine note
- safe gain
- no MIDI
- no presets
- no sequencer
- no plugin framework

## Core rule

```text
Tests prove it works.
Ears decide if it belongs.
```

## Run locally

```bash
npm install
npm run dev
```

Then open the local URL shown by Vite.

## Check files

```bash
npm run check
```

## Project structure

```text
.
├─ docs/
│  ├─ SYNTH_SPEC.md
│  ├─ SUPERPOWERS_TEST_PLAN.md
│  └─ LISTENING_CHECKPOINTS.md
├─ scripts/
│  └─ check.js
├─ src/
│  ├─ audio-engine.js
│  ├─ main.js
│  └─ styles.css
├─ index.html
├─ package.json
└─ README.md
```

## Superpowers operating rule

Use Superpowers for process discipline, not taste.

The coding agent may build the code. The synth identity must still be approved by a human listening test.
