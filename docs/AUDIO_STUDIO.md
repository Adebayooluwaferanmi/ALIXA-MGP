# Audio Studio

## Scope
Remotion assembles voice, music and effects in the timeline; a separate TTS system synthesizes voice and optionally a music engine generates instrumental audio. Integrate providers behind a pluggable API rather than hardwire a model.

## Workflow
Scene-level script -> word/pronunciation dictionary -> text normalization -> chosen licensed voice profile -> per-scene TTS generation -> preview and approve -> timing alignment -> soundtrack selection/generation -> FFmpeg mixing/ducking/loudness -> Remotion A/V render -> listening QA.

## Profiles and rights
Store only consented and licensed reference voices; evidence of voice-owner permission and permitted commercial uses. A deep male baritone with Nigerian-neutral English is the ALMS pilot requirement, but no model is pre-approved as achieving it. Benchmark quality, latency, memory, pronunciation and cost before selection. Do not guarantee accent fidelity. Music rights metadata and output model licenses are publication gates.

## Technology direction
Evaluate CPU-oriented Kokoro for affordable prototypes; compare high-quality reference-capable Chatterbox/CosyVoice via GPU worker if approved. Licensed library tracks before generative music to reduce cost and legal ambiguity. Prefer low-cost FFmpeg for mixing and mastering; include actual audio in MP4 and automated probe.

## Determinism and revision
Pin provider/model/voice version, script hash, seed if supported and audio artifact SHA. Regeneration of one scene must preserve approved audio from other scenes. Flag narration that exceeds scene duration and offer approval, copy revision or retiming. Never silently time-stretch the narrator beyond reasonable limits.

## QA
Listening tests, clip audibility, clipping/peaks, background ducking, sync, correct names including ALMS/ALIXA and haematology, silence gaps, rights and export verification. Synthetic audio labeling when appropriate. No public voice generation before privacy/licensing review.
