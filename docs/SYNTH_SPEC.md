# 700S Study Synth — Phase 0 Design Map

## Status

Phase 0 design map for a new-from-scratch browser synth inspired by the MiniKorg 700S signal flow and playing character, extended into a polyphonic browser instrument from the start.

This is an educational / study recreation target. It is not a branded commercial clone.

## Working name

**700S Study Synth**

Use this name internally while building.

Do not use Korg, miniKORG, or 700S as the final product name unless the project remains private/internal.

## Design goal

Build a compact polyphonic browser synth that captures the core experience of a 700S-style instrument while allowing simple chords and held intervals from the beginning.

Core experience:

- immediate playable sound
- polyphonic note handling from v0.1
- simple controls
- raw oscillator tone
- useful lead, bass, interval, and chord sounds
- quirky tone shaping
- dual high-pass / low-pass Traveller-style filtering later
- second oscillator and effect modes later
- no preset-first workflow
- no broad workstation behaviour

The first goal is not perfect emulation. The first goal is to build the architecture in the right order.

Important correction: the original-inspired reference is monophonic, but this project is allowed to become an original polyphonic study instrument. Polyphony is now part of the core project direction, not a late add-on.

## Design rules

1. Build the voice before building the interface fully.
2. Add one feature at a time.
3. Support polyphonic note handling from v0.1.
4. Keep the first polyphonic implementation small: limited voices, simple allocation, safe gain.
5. Avoid modern synth bloat.
6. Do not add presets until the instrument has a character worth saving.
7. Every version must have one audible proof.
8. Tests prove the app works. Listening decides whether the sound belongs.

## Reference behaviour to study

The original-inspired design is based on these behaviours:

- subtractive synth behaviour
- front-panel performance workflow
- first oscillator as the main tone source
- second oscillator as an added 700S feature
- VCO2 duet / ring-mod / noise-style effect behaviour later
- high-pass and low-pass Traveller-style tone shaping
- attack and singing/percussion-style envelope behaviour
- sustain/release behaviour
- portamento-style behaviour later
- vibrato later
- repeat / retrigger behaviour later

Project-specific extension:

- polyphonic voice allocation from the first implementation slice
- several notes can sound together, but voice count must stay limited until gain safety and release behaviour are proven

## What we will implement first

### v0.1 — First polyphonic tone

Purpose: prove that the app can make several safe, playable, raw synth notes at the same time.

Scope:

- browser app loads
- audio unlock works
- limited polyphony works
- several notes can play at once
- each note gets its own oscillator voice
- safe master gain
- one oscillator per voice
- one waveform at first
- one basic tone control shared across voices
- notes can stop cleanly
- simple voice limit to avoid uncontrolled stacking

Do not build the whole 700S-style instrument in v0.1.

Good enough:

> Press or hold several keys/buttons, hear more than one raw synth note at once, change their brightness with one shared tone control, release them safely.

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
- arpeggiator
- reverb
- plugin export
- visual replica of the original hardware
- complex voice stealing modes
- high voice counts

## Planned version path

| Version | Goal | Audible proof |
|---|---|---|
| v0.1 | First polyphonic tone | Several safe raw notes with one shared tone control |
| v0.2 | Small keyboard input | A small range of notes plays polyphonically |
| v0.3 | Basic envelope | Attack/release shape is audible per voice |
| v0.4 | First Traveller pass | High-pass and low-pass tone shaping works across active voices |
| v0.5 | Main oscillator waveforms | Triangle/pulse/saw-style choices sound different |
| v0.6 | Second oscillator | Added oscillator thickens or detunes each voice |
| v0.7 | VCO balance/tuning | VCO2 can sit above/below the main oscillator per voice |
| v0.8 | Vibrato | Pitch movement is controllable |
| v0.9 | Portamento / glide mode | Glide behaviour works where appropriate |
| v1.0 | First complete polyphonic study voice | It feels recognisably 700S-inspired but playable in chords |
| v1.1 | Voice allocation polish | Voice stealing, release tails, and gain safety behave cleanly |

Later versions can add repeat, noise, ring-mod modes, patch saving, MIDI, and UI polish.

## Web Audio signal path

### v0.1 path

```text
user input
→ note tracking
→ simple voice allocator
→ one oscillator voice per active note
→ shared basic tone filter or per-voice basic tone filter
→ per-note simple gate
→ summed voice bus
→ master safety gain
→ output
```

### Target path after staged build

```text
user input
→ note tracking
→ voice allocator
→ one complete study voice per active note
→ per-voice pitch handling
→ per-voice VCO1
→ per-voice VCO2 / noise / ring-mod options
→ per-voice oscillator/effect balance
→ Traveller high-pass filter
→ Traveller low-pass filter
→ per-voice envelope-controlled VCA
→ vibrato / modulation routing
→ summed voice bus
→ master safety limiter / gain control
→ output
```

Polyphony must not bypass the tone, envelope, or safety model.

## Controls map

### v0.1 controls

- Start Audio
- Note buttons / small keyboard
- Tone
- Voice Count display or fixed voice limit

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
- portamento / glide time
- repeat speed
- voice mode / voice count if needed

## Interface principle

The interface should feel like a small performance instrument, not a software control panel.

Use:

- large simple controls
- limited choices
- obvious labels
- no deep menus at first
- no preset browser at first
- a small playable key area from the early build

Avoid:

- modulation matrix
- rack-style complexity
- DAW-plugin overload
- visual copying of the original hardware
- turning polyphony into a workstation-style feature set

## Polyphony principle

Polyphony is now part of the instrument from the start.

Rules:

1. Keep voice count limited at first.
2. Give each note its own voice object.
3. Use safe summed gain.
4. Stop notes cleanly.
5. Add proper release-tail handling before high voice counts.
6. Add voice stealing before allowing complex playing.
7. Do not redesign the instrument around lush modern pads before the raw study voice has character.

Polyphony should make the instrument playable for simple chords and held intervals. It should not turn the synth into a broad preset pad machine.

## First audible proof

The first audible proof is:

> Several raw notes can sound together, remain stable and safe, and share one tone control.

Passes if:

- the app starts cleanly
- audio unlock works
- at least three notes can sound together
- notes start without a long delay
- notes stop without hanging forever
- the tone control clearly changes brightness/darkness
- output stays safe when several notes are held
- no unrelated features appear

Fails if:

- notes clip badly
- notes get stuck
- multiple notes cannot sound together
- the tone control does nothing obvious
- it adds unplanned features
- it starts building the full instrument before the first polyphonic tone is proven

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

## Current implementation issue

Current implementation target:

**Issue #2 — v0.1: First polyphonic tone**

Purpose:

Build the first audible proof only.

Scope:

- app loads
- audio unlock works
- several notes can play together
- each note has its own oscillator voice
- one shared tone control changes brightness/darkness
- safe summed gain
- no second oscillator
- no full Traveller
- no ring mod
- no noise
- no MIDI
- no presets

Stop when several safe raw notes play together and the tone control works.