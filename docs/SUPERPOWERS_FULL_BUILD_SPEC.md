# Superpowers Full Build Spec — 700S Study Synth

## Purpose

This is the full build instruction for Superpowers or another coding agent.

The goal is to complete a working browser synth from scratch, not to stop at a tiny first proof.

The synth is a new original 4-voice polyphonic browser instrument inspired by the MiniKorg 700S signal flow and playing character.

This is an educational / study recreation target. It is not a branded commercial clone.

## Product name

Use this internal name while building:

**700S Study Synth**

Do not use Korg, miniKORG, or 700S as the final public product name unless this remains a private/internal study project.

## Core user decision

The user wants:

- a new synth from scratch
- MiniKorg 700S-inspired architecture and character
- polyphony from the start
- fixed 4-voice polyphony for the first complete build
- Superpowers to complete the build from a full spec, not only one tiny issue

## Completion target

Build a complete first usable version of the synth.

The finished app must provide:

- 4-voice polyphonic playing
- browser-based Web Audio synth engine
- small playable keyboard or note buttons
- computer keyboard input if practical
- audio unlock/start button
- one oscillator per voice at minimum
- selectable main oscillator waveform
- shared tone control
- high-pass and low-pass Traveller-style tone shaping
- basic per-voice envelope
- second oscillator per voice
- VCO2 level/balance
- VCO2 tuning controls
- vibrato
- portamento/glide mode where musically useful
- optional noise mode
- optional ring-mod / metallic mode if feasible within the build
- safe summed output gain
- fixed 4-voice limit
- stuck-note prevention
- simple clear UI
- no preset browser required
- no MIDI required
- no sequencer required
- no plugin export required

## Overall sound target

The sound should feel:

- raw
- direct
- slightly odd
- expressive
- good for leads
- good for basses
- usable for simple chords
- capable of bright/thin and dark/rounded tones
- quirky rather than polished

Do not over-smooth it into a modern soft pad machine.

## Architecture target

### Full signal path

```text
user input
→ note tracking
→ fixed 4-voice allocator
→ per-voice pitch handling
→ per-voice portamento/glide where enabled
→ per-voice VCO1
→ per-voice VCO2 / noise / ring-mod option where enabled
→ per-voice oscillator/effect balance
→ high-pass Traveller-style filter
→ low-pass Traveller-style filter
→ per-voice envelope-controlled gain
→ vibrato/modulation where enabled
→ summed voice bus
→ master gain safety
→ output
```

The implementation may place filters per voice or after the voice bus, but the behaviour must be musically coherent and safe. If uncertain, choose the simpler safe implementation and document the decision.

## Voice model

Use a fixed 4-voice model.

Each active note should have its own voice object.

Each voice should manage:

- note id / pitch
- oscillator 1
- oscillator 2 when enabled
- optional noise/ring-mod path when enabled
- gain envelope
- stop/release lifecycle
- cleanup after release

Voice allocation rules:

- up to 4 simultaneous voices
- pressing a fifth note must not crash or clip
- use a simple voice stealing rule if needed
- oldest active voice stealing is acceptable for the first complete build
- release tails should not hang forever
- all notes must stop on pointer/key release
- add an all-notes-off panic button or internal safety if practical

## Controls required

### Basic app controls

- Start Audio
- Panic / All Notes Off if practical
- Master Volume or safe fixed output level
- active voice count display or status text

### Play controls

- small on-screen keyboard or note buttons
- at least enough notes for simple intervals and chords
- computer keyboard mapping if practical

### Oscillator controls

- VCO1 waveform
- VCO2 enable
- VCO2 waveform if practical
- VCO2 level / balance
- VCO2 coarse tune
- VCO2 fine tune if practical

### Tone controls

- Tone / brightness control
- Traveller High / low-pass style control
- Traveller Low / high-pass style control

The Traveller controls do not need to be a perfect circuit emulation. They must give a clear high-pass / low-pass tone-shaping behaviour that feels central to the instrument.

### Envelope controls

- Attack
- Release or Decay/Release
- a simple sustain/gate behaviour

The envelope should be clear and playable. Do not overcomplicate it.

### Performance controls

- Vibrato speed
- Vibrato depth
- Portamento / Glide time
- Portamento on/off or mode if practical

### Character/effect controls

Add if feasible after the core system works:

- Noise mode or noise level
- Ring-mod / metallic mode

If these features create instability or scope risk, leave them as clearly marked TODOs and finish the main synth cleanly.

## UI requirements

The interface should be simple and readable.

Use:

- clear headings
- grouped controls
- obvious labels
- large enough controls for mouse/touch
- simple status messages
- no hidden essential controls

Suggested sections:

1. Play
2. Oscillators
3. Traveller / Tone
4. Envelope
5. Performance
6. Output / Status

Avoid:

- dense modular UI
- small unreadable controls
- decorative hardware-copy interface
- fake screws / labels / brand cloning

## Technical requirements

Use a simple browser app stack already present or easy to run.

Current repo direction uses Vite.

The final build should include:

- `index.html`
- `package.json`
- `src/main.js`
- `src/audio-engine.js`
- `src/styles.css`
- `scripts/check.js`
- any additional small modules if useful

Keep code readable and split by responsibility.

Preferred structure:

```text
src/main.js              UI wiring and event handling
src/audio-engine.js      audio context, voices, controls, safety
src/styles.css           UI styling
scripts/check.js         repository sanity checks
```

Additional modules are allowed if the code becomes clearer, but do not over-engineer.

## Safety requirements

Audio safety matters.

Required:

- safe master gain
- no uncontrolled feedback
- no runaway oscillator creation
- no permanent stuck notes
- fixed 4-voice limit
- cleanup stopped voices
- tone/effect controls must not produce extreme volume jumps
- four held notes must stay at a safe level

If adding distortion/ring-mod/noise, reduce gain or add safety limiting.

## Build/check requirements

At minimum:

- `npm install` works
- `npm run dev` starts the app
- `npm run build` succeeds
- `npm run check` succeeds

`scripts/check.js` should verify basic project structure and fail if key files are missing.

It does not need to prove audio quality.

## Manual listening test

The coding agent cannot approve final sound quality.

It must report when the app is ready for human listening.

Manual listening pass:

- one note plays
- four notes can play together
- tone controls are obvious
- envelope is audible
- Traveller-style controls change tone usefully
- VCO2 thickens/detunes/changes the sound
- vibrato is audible and controllable
- portamento behaves sensibly if included
- no obvious clipping
- no stuck notes

## Acceptance criteria

The build is complete when:

- the browser app runs locally
- the synth plays up to 4 notes at once
- note start/stop works reliably
- all required controls are present and connected
- the tone/traveller controls audibly change the sound
- the envelope controls audibly shape notes
- VCO2 can be enabled and adjusted
- vibrato works
- portamento/glide works or is clearly deferred with explanation
- output remains safe with 4 notes held
- `npm run build` passes
- `npm run check` passes
- changed files are reported
- known limitations are listed

## Scope handling rule for Superpowers

Superpowers should complete the full build described here.

It may internally break the work into steps, but it should not stop after only the first tone unless blocked by a real error.

Expected working method:

1. Read this full build spec.
2. Inspect the repo.
3. Make a complete implementation plan.
4. Build the app in controlled steps.
5. Run checks repeatedly.
6. Keep scope aligned with this document.
7. Report final status, files changed, tests run, and known limitations.

Do not ask the user to approve every tiny internal step unless there is a real design ambiguity or technical blocker.

## Current user decisions that must not be overridden

- Polyphonic now.
- Fixed 4 voices.
- Full Superpowers build, not only v0.1.
- MiniKorg 700S-inspired study synth.
- New synth from scratch.
- Keep the UI simple.
- Do not turn it into a generic workstation synth.

## Good final result

A user can open the app, start audio, play up to four notes, shape the tone, hear a clear 700S-inspired subtractive character, adjust envelope/performance controls, and avoid clipping or stuck notes.

That is the completion target for Superpowers.
