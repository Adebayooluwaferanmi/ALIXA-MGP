# UI/UX functional specification

## Navigation hierarchy
Dashboard / Projects / Project Detail / Editor / Code Workspace / Capture Studio / Audio Studio / Assets / Render Queue / Results / Templates / Team & Settings.

## Project wizard
Name, description, workspace, mode (trusted-template / custom-code / GitHub import), source revision or assets location, orientation(s), quality preset, duration/FPS, voice/music, quota indicator. Show estimated workload; never fabricate dollar estimates. Project is created as draft, not immediately rendered.

## Editor layout
Desktop: left asset/source tree and storyboard; center responsive Remotion Player with play/scrub/frame snapshot; right properties inspector for scene content, typography, time, transitions, captions and audio tracks. Bottom timeline with scenes, waveform, status, markers and notes. On small displays use tabs and stacked panels.

## Code Workspace
Monaco editor, diagnostics, file explorer, pinned dependencies, build button, build logs, bundle hash, linked asset names; cannot install arbitrary packages in production. Preview must use a build artifact from a controlled environment; allow inline schema edits without rebuild for input-only changes.

## Browser Capture Studio
URL input + allowlist/safety warning; choose local-tab recording or remote Chromium session. Show visible recording badge, elapsed time, Start/Pause/Stop, Screenshot (viewport/full-page/element in remote mode), recording preview, trim/crop/redact, save with tags and source metadata. Remote login is interactive in the session; do not request user password in platform forms or log it. Local capture uses getDisplayMedia permissions and cannot programmatically control other tabs absent extension/remote session.

## Audio Studio
Narration script split by scenes; voice selection/profile, pronunciation dictionary, voice preview, clip regenerate and approve; waveform alignment; track selection with license metadata; speech-first mix preset; flag TTS scene overrun. Remotion handles timeline playback/mixing of existing audio; separate TTS synthesizes it.

## Render workspace
Jobs table with queued/running/QA/complete/failed/cancelled, format, code bundle version, asset snapshot, CPU-time estimate, actual cost telemetry, queue position and retry. Inline streaming logs must redact tokens. Outputs show thumbnails, technical QC report, playable MP4, signed download and export.

## Team workflow
Roles: Owner/Admin, Editor/Developer, Producer, Reviewer, Viewer. Approval required for publishing and optional for high-cost renders. Audit on source imports, asset access, voice-profile use and exports. Users can compare versions and roll back without moving mutable pointers.

## Accessibility and experience
WCAG 2.2 AA design target; keyboard shortcuts, visible focus, high-contrast timeline, no color-only status, captions, screen-reader labels, reduced-motion preference. Asset/video uploads resume when supported; informative empty/loading/error states; Nigeria mobile network considerations.

## UX acceptance
A new producer can create a project, import screenshots, modify text, preview at 480p and request full render using browser UI only. User can identify why a render failed and retry without developer intervention. Desktop and phone review flows must be tested.
