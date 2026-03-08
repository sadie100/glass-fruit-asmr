# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Glass Fruit ASMR Slicer — a browser-based interactive experience where users drag-slice glass fruit with procedural ASMR audio. Single-file app (`index.html`), no build step.

## Running

Open `index.html` directly in a browser, or use the dev server:

```bash
npm run dev    # serves on http://localhost:8080
```

No dependencies to install — Three.js r160 is loaded via CDN import map.

## Architecture

Everything lives in one `index.html` file with inline `<style>` and `<script type="module">`:

- **ASMRAudio class** (~lines 151–340): Procedural Web Audio API sound engine. Generates glass slice, crystal resonance, shard tinkle, and ambient sounds. Each fruit type has unique audio parameters (baseFreq, resonance, color).
- **FRUITS config** (~lines 343–421): Defines 5 fruit types (apple, orange, grape, watermelon, strawberry) with custom geometry generators, colors, and sizes. Grape uses a cluster mode.
- **Glass shaders** (~lines 424–505): Custom GLSL vertex/fragment shaders for the glass material (fresnel, specular highlights, internal glow) and cross-section surfaces (ring pattern, shimmer).
- **GlassParticle class** (~lines 580–648): Physics-based glass shard particles with gravity, ground collision, and landing sounds.
- **Slicing system** (~lines 753–900): Converts 2D mouse drag into a 3D cutting plane, clips meshes, creates two halves with cross-section caps, and spawns particles.
- **Input system** (~lines 903–970): Pointer event handling for slash gesture detection with raycasted hit testing against fruit meshes.
- **Animation loop** (~lines 1017–1097): Updates fruit rotation/floating, sliced piece physics/fading, particles, and 2D slash trail overlay.

## Key Technical Notes

- Uses Three.js `ShaderMaterial` with custom GLSL — not `MeshPhysicalMaterial` — for the glass effect
- Slicing uses `THREE.Plane` clipping planes, not actual mesh boolean operations
- Audio is fully procedural (no audio files) — initialized on first user interaction per browser autoplay policy
- Korean UI text (lang="ko")
- Custom cursor replaces native cursor (`cursor: none` on body)

## Playwright MCP

When using `--output-mode file`, always set `--output-dir .playwright-output/`.
