# RS-4000 — Codex repository instructions

This file governs implementation decisions. Read `README.md` first: it is the authoritative visual-fidelity and screenshot-comparison contract. Read this file and the active build specification before editing. Do not restate or weaken the README's design rules.

## Source-of-truth order
1. Explicit latest user instructions and approved changes.
2. `README.md` and its locked visual references: `rs4000-main.png`, `rs4000-graphic.png`, `rs4000-arp-mode.png`.
3. The complete RS-4000 build specification supplied in the Codex task.
4. Existing, verified working behaviour and tests.

If sources conflict, identify the conflict and ask rather than improvising a redesign. The main image fixes the physical layout; the ARP image fixes the content of the central display in ARP mode. The engine graphic sheet fixes the individual engine graphics. No static reference image is an acceptable substitute for a working interface.

## Working discipline
- Before work: inspect repository files, reference image dimensions and current app state; report a short plan and acceptance criteria.
- Implement in meaningful grouped milestones, not dozens of tiny approval gates. Continue within an approved milestone while tests pass; stop for a true design ambiguity or unavoidable visible change.
- Keep a running checklist distinguishing implemented, verified, partial and blocked. Never claim tests, listening or screenshot comparison that did not occur.
- Preserve functional controls and existing project behaviour when changing styling or DSP. No unrequested controls, renamed labels, generic replacements or layout changes.
- Make focused, reviewable commits with concise descriptions. Do not overwrite or discard user assets.

## Rendering and visual verification
- Measure the reference geometry; use a stable coordinate system and scale the whole instrument proportionally. Do not independently reflow sections.
- Keep the brushed-metal chassis, two matching reels, tape path, control grouping, labels, colours, keybed and screen geometry faithful to `rs4000-main.png`.
- The three supplied PNGs are design references, not backgrounds to hide non-functional controls beneath. Prefer separately rendered assets and real hit targets.
- For each visually significant milestone, capture screenshots at the reference aspect ratio, compare with the relevant PNG, correct discrepancies and retain comparison evidence or report why it was unavailable.
- Ensure ARP mode changes the **central screen only**. Do not turn the ARP into a separate full-instrument redesign.
- Engine graphics are live, low-motion renderings on black, matching the ten reference motifs; never substitute generic oscilloscopes.
- For tape animation, model one continuous path, correct guide tangencies and outer reel winding points; both reels must match visually and rotate consistently. Animation is cosmetic and must never drive audio timing.

## Audio, MIDI and timing
- Use Web Audio with AudioWorklets for real-time DSP where appropriate; keep audio scheduling independent of `requestAnimationFrame` and UI layout.
- All ten engines must produce genuinely different sound using their specified architectures. No single generic oscillator with ten visual skins; no fake controls.
- Use anti-aliasing/band-limited oscillators where needed, stable feedback and filters, interpolation, smoothing, click-free note handling and safe output levels.
- Separate note sources (keyboard, external MIDI, ARP, SEQ16 and MIDI piano roll) from synthesis voices. Route them through one deterministic event/scheduling layer; prevent stuck or duplicated notes.
- Browser audio unlock and Web MIDI permissions must be handled gracefully. Work without external MIDI hardware.
- Reserve a documented shared-clock interface for future Sound Factory integration; do not pretend external sync is implemented in standalone mode.
- Avoid unlicensed samples, proprietary firmware, copied code or assertions of exact OP-1 circuit/algorithm cloning. Implement original, documented, analogue-/OP-1-inspired techniques.

## Popup and state rules
- The physical `MIDI` button opens the piano-roll editor; `ARP` selects the central-screen ARP interface; `FX EDIT` opens effects editing; `FREEZE` performs its audio function; the modifier-section button is labelled `AUTOMATE`, not `EDIT`.
- The automation editor records parameter gestures for CUTOFF, RESONANCE, MODIFY and SPEED; gesture playback must not block live interaction.
- Closing an editor must not stop a running sequence or audio. Preserve editor state, active notes, transport and playback independently of window visibility.
- The ARP screen uses flat, luminous **on-screen graphics**, not photographed physical controls. The approved ARP mode omits ON, PLAY, STOP, REC, PENDULUM and REPEAT; `SPEED` replaces `VELOCITY`.

## Verification gates
- Run available unit and integration tests after each grouped milestone. Add tests for engine parameter mappings, note on/off, note scheduling, ARP modes, MIDI paint/erase, automation playback, freeze and persistence.
- Perform real browser smoke tests for audio unlock, all visible controls, all ten engine sounds, engine graphics, keyboard, SEQ16, MIDI editor, ARP, automation and tape animation.
- Verify there are no console errors, stuck notes, discontinuities, silent outputs, obvious clipping or unbounded CPU/memory growth. Record browser/platform used.
- Where possible perform short offline audio renders and numerical checks (finite samples, peak/RMS, aliasing, tails, silence after release); do not claim subjective sound quality was verified without listening.
- A milestone is not done unless both functionality and visual fidelity have been checked; document residual issues explicitly.

## Delivery
- Maintain GitHub Pages compatibility with relative asset paths and no required server-side runtime.
- Include clear startup, controls, MIDI-permission and deployment instructions without rewriting the README's design manifesto.
- At each milestone report: implemented items; tests and results; screenshot comparisons; limitations; next grouped milestone.
