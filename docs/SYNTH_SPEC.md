# 700S Study Synth — Phase 0 Design Map

## Status

Phase 0 design map for a new-from-scratch browser synth inspired by the MiniKorg 700S signal flow and playing character.

This is an educational / study recreation target. It is not a branded commercial clone.

## Working name

**700S Study Synth**

Use this name internally while building.

Do not use Korg, miniKORG, or 700S as the final product name unless the project remains private/internal.

## Design goal

Build a compact monophonic browser synth that captures the core experience of a 700S-style instrument:

- immediate monophonic playing
- simple controls
- raw oscillator tone
- useful lead and bass sounds
- quirky tone shaping
- dual high-pass / low-pass Traveller-style filtering
- second oscillator and effect modes later
- no preset-first workflow
- no broad workstation behaviour

The first goal is not perfect emulation. The first goal is to build the architecture in the right order.

## Design rules

1. Build the voice before building the interface fully.
2. Add one feature at a time.
3. Keep the instrument monophonic until the core sound works.
4. Avoid modern synth bloat.
5. Do not add presets until the instrument has a character worth saving.
6. Every version must have one audible proof.
7. Tests prove the app works. Listening decides whether the sound belongs.

## Reference behaviour to study

The original-inspired design is based on these behaviours:

- monophonic subtractive synth behaviour
- front-panel performance workflow
- first oscillator as the main tone source
- second oscillator as an added 700S feature
- VCO2 duet / ring-mod / noise-style effect behaviour later
- high-pass and low-pass Traveller-style tone shaping
- attack and singing/percussion-style envelope behaviour
- sustain/release behaviour
- portamento
- vibrato
- repeat / retrigger behaviour

## What we will implement first

### v0.1 — First monophonic tone

Purpose: prove that the app can make one safe, playable, raw synth note.

Scope:

- browser app loads
- audio unlock works
- one monophonic note plays
- safe master gain
- one oscillator
- one waveform at first
- one basic tone control
- note can stop cleanly

Do not build the whole 700S in v0.1.

Good enough:

> Press a key or button, hear one raw monophonic tone, change it with one tone control, release it safely.

## What we deliberately postpone

Do not add these in v0.1:

- second oscillator
- full Traveller pair
- ring modulation
- white/pink noise
- repeat mode
- vibrato
- portamento
- full envelope panel
- MIDI
- presets
- sequencer
- polyphony
- arpeggiator
- reverb
- plugin export
- visual replica of the original hardware

## Planned version path

| Version | Goal | Audible proof |
|---|---|---|
| v0.1 | First monophonic tone | One safe raw note with one tone control |
| v0.2 | Small keyboard input | A small range of notes plays monophonically |
| v0.3 | Basic envelope | Attack/release shape is audible |
| v0.4 | First Traveller pass | High-pass and low-pass tone shaping works |
| v0.5 | Main oscillator waveforms | Triangle/pulse/saw-style choices sound different |
| v0.6 | Second oscillator | Added oscillator thickens or detunes the tone |
| v0.7 | VCO balance/tuning | VCO2 can sit above/below the main oscillator |
| v0.8 | Vibrato | Pitch movement is controllable |
| v0.9 | Portamento | Notes glide when played legato |
| v1.0 | First complete study voice | It feels recognisably 700S-inspired |

Later versions can add repeat, noise, ring-mod modes, patch saving, MIDI, and UI polish.

## Web Audio signal path

### v0.1 path

```text
user input
→ pitch selection
→ oscillator 1
→ basic tone filter
→ note gain / simple gate
→ master safety gain
→ output
```

### Target path after staged build

```text
user input
→ monophonic note priority
→ pitch + portamento
→ VCO1
→ VCO2 / noise / ring-mod options
→ oscillator/effect balance
→ Traveller high-pass filter
→ Traveller low-pass filter
→ envelope-controlled VCA
→ vibrato / modulation routing
→ master safety gain
→ output
```

## Controls map

### v0.1 controls

- Start Audio
- Note On / Note Off
- Tone

### Early controls

- Waveform
- Attack
- Release or Singing
- Low Traveller
- High Traveller
- Volume

### Later controls

- VCO2 on/off
- VCO2 balance
- VCO2 coarse tune
- VCO2 fine tune
- duet / ring-mod / noise mode
- vibrato speed
- vibrato depth
- portamento time
- repeat speed

## Interface principle

The interface should feel like a small performance instrument, not a software control panel.

Use:

- large simple controls
- limited choices
- obvious labels
- no deep menus at first
- no preset browser at first

Avoid:

- modulation matrix
- rack-style complexity
- DAW-plugin overload
- visual copying of the original hardware

## First audible proof

The first audible proof is:

> One monophonic note that is raw, stable, safe, and shaped by a single tone control.

Passes if:

- the app starts cleanly
- the note starts without a long delay
- the note stops without hanging forever
- the tone control clearly changes brightness/darkness
- output stays safe
- no unrelated features appear

Fails if:

- it clicks badly
- it clips badly
- it adds unplanned features
- it feels like a generic preset synth
- it starts building the full instrument before the first tone is proven

## Superpowers handoff rule

When Superpowers or another coding agent works on this repo, it must follow this sequence:

1. Read this spec.
2. Read the current GitHub issue.
3. Confirm the exact version scope.
4. Make a small implementation plan.
5. Code only the scoped change.
6. Run available checks.
7. Report changed files and verification result.
8. Stop.

Do not let the agent choose the next feature by itself.

## Next issue to create

Create this next:

**Issue #2 — v0.1: First monophonic tone**

Purpose:

Build the first audible proof only.

Scope:

- app loads
- audio unlock works
- one oscillator plays one note
- one tone control changes brightness/darkness
- safe gain
- no second oscillator
- no full Traveller
- no ring mod
- no noise
- no MIDI
- no presets

Stop when one safe raw note plays and the tone control works.
