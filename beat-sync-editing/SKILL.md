---
name: beat-sync-editing
description: This skill should be used when the user asks to "cut to the beat", "fix the pacing", "edit this sequence", "sync cuts to music/BPM", "add a match cut / J-cut / L-cut", "build a speed ramp", or "plan a shot sequence / edit timeline". Covers editing rhythm, transitions, retiming, and translating an edit plan to an NLE or a code timeline (Remotion/AE frames).
version: 0.1.0
---

# Beat-Sync Editing (Cuts & Rhythm)

The timeline craft: how clips and beats assemble into a piece that *feels* right. Editing is temporal design — when a cut lands matters as much as what is on screen. This skill turns musical and narrative structure into concrete cut points, transitions, and retimes, expressible in an NLE or as frame numbers in code.

## When to use

- Assembling a sequence or cutting footage to a music track.
- Pacing drags or feels choppy; transitions feel arbitrary.
- Building speed ramps, match cuts, or tension/release structure.
- Converting an edit plan into a frame-accurate timeline (Remotion, AE, NLE).

## Core workflow

### 1. Find the grid: BPM → frames

Cut on the music's rhythmic grid, not on arbitrary timecodes. Convert BPM to frames so cut points are frame-accurate.

```
frames_per_beat = (60 / BPM) * fps
```

At 120 BPM, 30 fps: `(60/120)*30 = 15` frames per beat — a cut every 15 frames is on-beat. Cut on *phrases* (every 2/4/8 beats), not every beat, or the edit feels frantic. Land the single biggest visual moment on the musical drop.

```js
// Generate on-beat frame numbers (JS / Remotion / AE-friendly)
const bpm = 120, fps = 30;
const framesPerBeat = (60 / bpm) * fps;       // 15
const phrase = 4;                              // cut every 4 beats
const cuts = Array.from({length: 16}, (_, i) =>
  Math.round(i * framesPerBeat * phrase));     // [0,60,120,180,...]
```

### 2. Hide the seam: cut on action / motion

A cut is invisible when motion carries the eye across it. Cut *during* a movement (a turn, a hand reaching, a whip) so the brain tracks the action through the cut. Match the motion's direction and speed across the cut. For motion-graphics, cut on a transform peak — at the moment a shape is mid-travel, not at rest.

### 3. Choose transitions with intent

Default to the **hard cut**. Reach for anything else only when motivated:

- **Match cut** — outgoing and incoming frames share a shape, position, or motion vector, so the cut reads as a transformation (a circle becomes the moon; a wipe of an arm reveals the next scene). The strongest transition in motion design.
- **J-cut** — audio of the *next* shot starts *before* its picture (sound leads). Pulls the viewer forward; great for dialogue and reveals.
- **L-cut** — audio of the *current* shot continues *under* the next picture (sound trails). Smooths scene changes.
- **Whip / motion blur transition** — fast pan blends two shots; needs matched blur direction.
- **Luma/wipe/dissolve** — signals time passing or a softer mood; overuse reads as amateur.

### 4. Retime: speed ramps

Speed ramps (gradual time-warp) add impact: ramp *into* a hit with slow-motion, then snap back to speed. Keep motion blur consistent through the ramp or the slow section looks strobed.

```
Speed ramp on an impact (frames, 30fps):
  0–20    100% speed   (run-up)
  20–28   ease down to 25%  (anticipation, slow-mo on the hit)
  28      IMPACT lands on the beat
  28–40   ease back to 100% (release)
```

In an NLE use time-remap keyframes with smooth interpolation and enable frame-blending/optical-flow for clean slow-mo. In Remotion/AE, drive a `timeRemap`/`valueAtTime` curve with eased interpolation and keep a consistent shutter/motion-blur setting.

### 5. Shape the arc: sequence pacing

A piece needs a shape, not one constant intensity:

```
Establish → Develop → Climax → Resolve
  longer holds   shortening cuts   fastest + the drop   release/breathe
```

Vary shot length deliberately: fast cuts build energy, longer holds let key moments land. Build toward the climax by shortening average shot length, then give a beat of rest after it. The classic mistake is uniform pacing — it reads as flat regardless of content.

## Quick reference

| Goal | Move |
|---|---|
| On-beat cut | `(60/BPM)*fps` frames per beat; cut on phrases |
| Land the drop | biggest visual on the musical drop |
| Invisible cut | cut on action / matched motion |
| Transform feel | match cut (shared shape/motion) |
| Pull forward | J-cut (audio leads) |
| Smooth scene change | L-cut (audio trails) |
| Impact | speed ramp into the hit, snap out |
| Build energy | shorten average shot length |
| Let it breathe | hold after the climax |

## Gotchas

- Cutting on *every* beat is busy; cut on 2/4/8-beat phrases and accent sparingly.
- Speed ramps without consistent motion blur look strobed; enable optical-flow/frame-blending for slow-mo.
- Crossfades/dissolves are not a fix for a bad cut — fix the timing first.
- Audio sync drift: in code timelines verify `frame/fps` against detected beat seconds; in NLEs lock audio to a frame-accurate guide track.
- One pacing throughout = flat. Always shape an arc.

## Reference files

- `references/editing-techniques.md` — BPM→frame math with phrase tables, cut-on-action breakdowns, building a match cut step by step, J-cut/L-cut audio-lead/trail diagrams with frame offsets, full speed-ramp recipe with motion-blur/optical-flow settings, sequence-arc templates with shot-length curves, and mapping an edit plan between an NLE timeline and a code timeline (Remotion `<Sequence>` / AE time-remap).
