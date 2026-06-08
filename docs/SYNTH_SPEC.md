# 700S Style Synth

## Working name

**700S Study Synth**

Use this name internally while building.

## Design goal

Build a compact 4-voice polyphonic synth as both:

- a VST plugin
- a standalone desktop app

It should capture the core experience of a 700S-style instrument while allowing simple chords and held intervals.

This is not a browser app.

Core experience:

- immediate playable sound
- 4-voice polyphonic note handling
- simple controls
- raw oscillator tone
- useful lead, bass, interval, and chord sounds
- quirky tone shaping
- dual high-pass / low-pass Traveller-style filtering
- second oscillator and effect modes
- no preset-first workflow
- no broad workstation behaviour

## Reference behaviour to study

The original-inspired design is based on these behaviours:

- subtractive synth behaviour
- front-panel performance workflow
- first oscillator as the main tone source
- second oscillator as an added 700S feature
- VCO2 duet / ring-mod / noise-style effect behaviour
- high-pass and low-pass Traveller-style tone shaping
- attack and singing/percussion-style envelope behaviour
- sustain/release behaviour
- portamento-style behaviour
- vibrato
- repeat / retrigger behaviour later if useful
- 4-voice polyphonic voice allocation from the first implementation slice
- up to four notes can sound together, with gain safety and clean note release.

## Build target

The first complete build should produce:

- one standalone desktop app
- one VST plugin target, assumed VST3 unless changed later
- one shared synth engine used by both targets
