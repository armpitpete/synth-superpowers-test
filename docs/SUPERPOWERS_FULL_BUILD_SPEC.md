# Superpowers Full Build Spec — 700S Study Synth

## Purpose

This is the full build instruction for Superpowers or another coding agent.

The goal is to complete a working synth from scratch as both:

- a VST plugin
- a standalone desktop application

This is not a browser app and must not be delivered as a web page.

The synth is a new original 4-voice polyphonic instrument inspired by the MiniKorg 700S signal flow and playing character.

This is an educational / study recreation target. It is not a branded commercial clone.

## Product name

Use this internal name while building:

**700S Study Synth**

## Core user decision

The user wants:

- a new synth from scratch
- MiniKorg 700S-inspired architecture and character
- polyphony from the start
- fixed 4-voice polyphony for the first complete build
- a VST plugin build
- a standalone desktop app build
- Superpowers to complete the build from a full spec, not only one tiny issue

## Target formats

Build both targets from the same synth engine:

1. **VST plugin**
   - Treat VST as VST3 unless the user later says otherwise.
   - The plugin should load in a DAW that supports VST3.

2. **Standalone desktop app**
   - The standalone app should run without opening a browser.
   - It should expose the same synth engine and controls as the plugin.

Preferred implementation approach:

- Use a real audio-plugin framework if practical.
- JUCE with CMake is acceptable and likely suitable.
- Another framework is acceptable only if it can produce both a VST plugin and a standalone desktop app cleanly.

The existing Vite/browser scaffold may be replaced if it does not suit the VST/standalone target.

## Completion target

Build a complete first usable version of the synth.

The finished instrument must provide:

- 4-voice polyphonic playing
- shared synth engine used by both plugin and standalone app
- playable notes through the plugin host and/or standalone app
- computer keyboard input in standalone mode if practical
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

### Shared synth engine

The plugin and standalone app must use the same underlying synth engine.

Keep the audio engine separate from UI code.

Suggested architecture:

```text
shared synth engine
→ 4-voice allocator
→ per-voice oscillator/envelope/filter processing
→ summed voice bus
→ master safety gain
→ plugin wrapper
→ standalone wrapper
```

### Full signal path

```text
note input
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
→ audio output
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
- all notes must stop on note-off
- add an all-notes-off panic control or internal safety if practical

## Controls required

### Basic app/plugin controls

- Master Volume or safe fixed output level
- Panic / All Notes Off if practical
- active voice count display or status text if practical

### Play/input behaviour

- MIDI note input for the VST plugin
- playable input for standalone mode
- computer keyboard mapping in standalone mode if practical

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

1. Play / Status
2. Oscillators
3. Traveller / Tone
4. Envelope
5. Performance
6. Output

Avoid making the UI look like a browser demo. It should feel like an instrument/plugin interface.

## Technical requirements

The target is no longer a browser/Vite app.

Use a stack that can build:

- VST plugin
- standalone desktop app

Preferred structure if using JUCE/CMake:

```text
CMakeLists.txt
Source/
  PluginProcessor.h
  PluginProcessor.cpp
  PluginEditor.h
  PluginEditor.cpp
  SynthEngine.h
  SynthEngine.cpp
  Voice.h
  Voice.cpp
  Parameters.h
  Parameters.cpp
README.md
```

Equivalent structure is acceptable if another framework is chosen.

Keep code readable and split by responsibility:

- synth engine separate from UI
- voice handling separate from parameter/UI code
- plugin and standalone targets share the same engine

## Safety requirements

Audio safety matters.

Required:

- safe master gain
- no uncontrolled feedback
- no runaway oscillator creation
- no permanent stuck notes
- fixed 4-voice limit
- cleanup stopped voices
- controls must not produce extreme volume jumps
- four held notes must stay at a safe level

If adding distortion/ring-mod/noise, reduce gain or add safety limiting.

## Build/check requirements

Superpowers must define and document the build commands it uses.

At minimum, the final report must include:

- how to build the VST plugin
- how to build the standalone app
- where the built outputs are located
- which commands were run
- whether the build passed
- known limitations

If the local environment cannot build VSTs because required tooling is missing, Superpowers must still create the correct project structure and explain the missing dependency clearly.

## Manual listening test

The coding agent cannot approve final sound quality.

It must report when the plugin/app is ready for human listening.

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
- standalone app opens without a browser
- VST plugin is produced or the blocker is clearly documented

## Acceptance criteria

The build is complete when:

- a standalone desktop app target exists
- a VST plugin target exists
- both targets share the same synth engine
- the synth plays up to 4 notes at once
- note start/stop works reliably
- all required controls are present and connected
- tone/traveller controls audibly change the sound
- envelope controls audibly shape notes
- VCO2 can be enabled and adjusted
- vibrato works
- portamento/glide works or is clearly deferred with explanation
- output remains safe with 4 notes held
- build commands are documented
- changed files are reported
- known limitations are listed

## Scope handling rule for Superpowers

Superpowers should complete the full build described here.

It may internally break the work into steps, but it should not stop after only the first tone unless blocked by a real error.

Expected working method:

1. Read this full build spec.
2. Inspect the repo.
3. Make a complete implementation plan.
4. Build the VST/standalone project in controlled steps.
5. Run checks/builds repeatedly where possible.
6. Keep scope aligned with this document.
7. Report final status, files changed, commands run, build outputs, and known limitations.

Do not ask the user to approve every tiny internal step unless there is a real design ambiguity or technical blocker.

## Current user decisions that must not be overridden

- Not a browser app.
- VST plugin and standalone app.
- Polyphonic now.
- Fixed 4 voices.
- Full Superpowers build, not only v0.1.
- MiniKorg 700S-inspired study synth.
- New synth from scratch.
- Keep the UI simple.

## Good final result

A user can open the standalone app or load the VST plugin, play up to four notes, shape the tone, hear a clear 700S-inspired subtractive character, adjust envelope/performance controls, and avoid clipping or stuck notes.

That is the completion target for Superpowers.
