# 700S Style Synth

## Working name

**700S Study Synth**

Use this name internally while building.

## Design goal

Build a compact 4-voice polyphonic browser synth that captures the core experience of a 700S-style instrument while allowing simple chords and held intervals from the beginning.

Core experience:

- immediate playable sound
- 4-voice polyphonic note handling
- simple controls
- raw oscillator tone
- useful lead, bass, interval, and chord sounds
- quirky tone shaping
- dual high-pass / low-pass Traveller-style filtering later
- second oscillator and effect modes later
- no preset-first workflow
- no broad workstation behaviour

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
- 4-voice polyphonic voice allocation from the first implementation slice
- up to four notes can sound together, with gain safety and clean note release.
