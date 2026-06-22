---
name: shot-composition
description: This skill should be used when the user asks to "compose a frame", "set up a grid", "plan shot composition", "place the focal point", "decide where elements enter and exit", "add a parallax/camera move", "adapt a layout for 16:9 / 9:16 / 1:1", or "keep content inside safe areas". Covers grids, safe areas, focal hierarchy, and 2D/3D camera framing.
version: 0.1.0
---

# Shot Composition

Stage the frame and move the camera: grid systems, safe areas, focal hierarchy, negative space, parallax layering, and how layout guides the eye over time. Produces composition notes, layer structure (bg/mid/fg), and concrete camera-move plans with numbers.

## When to use

- Defining a frame's layout, focal point, and negative space.
- Planning element enter/exit directions and motion paths.
- Designing camera push/pull, pan, follow, and parallax moves.
- Adapting one composition across 16:9, 9:16, and 1:1 with safe areas.

## Core workflow

### 1. Establish a grid

A grid gives alignment and rhythm. Default to a **12-column grid** for landscape web/video (it divides into halves, thirds, quarters, sixths) with gutters and outer margins. For a 1920px-wide frame: outer margin 80-120px, 12 columns, gutter 24-32px.

```
content_width = frame_width - 2 * margin
column_width  = (content_width - (cols - 1) * gutter) / cols
```

Example (1920 wide, margin 100, gutter 24, 12 cols): content 1720, column = (1720 - 11*24)/12 = 121.3px.

A **baseline grid** (vertical rhythm) of 8px is the standard spacing unit; size and space everything in multiples of 8 (8/16/24/32/48/64) for visual cohesion.

### 2. Place the focal point

Use the **rule of thirds**: divide the frame into a 3x3 grid; place the primary subject on a third-line or at an intersection (the "power points"), not dead center — unless the piece deliberately uses symmetry for stillness/formality. The intersections roughly fall at 33%/67% horizontally and vertically.

Build **visual hierarchy** in this priority order to direct the eye: **size > contrast > color > position**. The biggest, highest-contrast element wins attention first. Establish exactly one primary focal point per frame; secondary elements support, never compete.

### 3. Use negative space as rhythm

Do not fill the frame. Generous negative space around the focal point makes it read as premium and confident, and gives motion room to breathe. Whitespace is an active compositional element, not leftover.

### 4. Layer the scene (bg / mid / fg) for depth and parallax

Split the composition into depth layers: **background** (slowest, lowest contrast, often desaturated/blurred), **midground** (the subject), **foreground** (fastest, may blur as it crosses). Parallax = moving layers at different speeds to simulate depth.

Parallax speed factors relative to camera/scroll movement:
- Background: 0.1-0.3x
- Midground: 0.5-0.7x
- Foreground: 1.0-1.5x (can exceed 1 for elements meant to read as very close)

### 5. Camera language (works in 2D and 3D)

- **Push / pull** — scale into or away from the subject (dolly). Builds intimacy or reveals context. 800-2000ms, ease-in-out.
- **Pan / tilt / move** — translate the frame across the scene. Reveals space, follows action.
- **Follow** — keep a moving subject framed (often slightly off-center, leading the motion).
- **Parallax** — layered move that sells depth.

**One shot, one intent.** Don't push + pan + rotate simultaneously — combined moves read as chaotic. Pick the single move that serves the beat. Inline timing essentials: camera moves use ease-in-out (`cubic-bezier(0.65, 0, 0.35, 1)`); never `linear` except continuous loops; slower is more cinematic.

### 6. Enter/exit direction carries meaning

Motion that follows reading direction (left-to-right, top-to-bottom in LTR locales) feels natural and smooth; reversing it creates tension or signals "back/undo." Match the direction to the intent:
- New content arriving -> enter from the right / from below.
- Dismissing / going back -> exit right or down, or reverse the entrance.
- Drilling deeper -> scale up / push in.

Elements should usually enter and exit through the nearest frame edge with a slight overshoot on enter (ease-out) and acceleration on exit (ease-in).

### 7. Safe areas and multi-aspect adaptation

Keep all critical content (text, logos, faces, CTAs) inside a **title-safe margin** so nothing gets clipped or hidden behind platform UI.

| Aspect | Use | Title-safe margin | Notes |
|---|---|---|---|
| 16:9 | YouTube, web hero | ~5% all sides | broadcast title-safe historically 10% |
| 9:16 | Reels, TikTok, Stories | top ~12-14%, bottom ~18-20%, sides ~6% | platform UI eats top and bottom |
| 1:1 | feed posts | ~6-8% all sides | symmetric |
| 4:5 | feed (taller) | ~6% sides, ~8% top/bottom | maximizes feed real estate |

Design the **focal point centered enough** that it survives all crops, then let secondary elements reflow per aspect. For 16:9 -> 9:16, restack horizontal arrangements vertically rather than just cropping the sides off.

## Quick reference

| Need | Default |
|---|---|
| Grid | 12 columns, 8px baseline, multiples of 8 spacing |
| Focal placement | rule-of-thirds power point, not dead center |
| Hierarchy order | size > contrast > color > position |
| Depth layers | bg / mid / fg, parallax 0.2x / 0.6x / 1.2x |
| Camera | one move at a time, ease-in-out, 800-2000ms |
| Enter / exit | follow reading direction; ease-out in, ease-in out |
| 9:16 safe | top 12%, bottom 18%, sides 6% |

## Reference files

- `references/camera-and-grids.md` — grid math with worked numbers, full safe-area percentage tables per aspect, parallax layering code (CSS scroll + JS pointer + Three.js), and named camera-move recipes (push-in, whip-pan, orbit, dolly-zoom) with keyframe values.
