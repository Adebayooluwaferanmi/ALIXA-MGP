# Browser Capture Studio

## User flow
Within a video project, select **Capture** -> Local tab or Remote browser -> enter URL if remote -> authenticate interactively -> capture screenshot/video -> review/trim/redact -> save to project asset library -> insert into scene. Capture rights, purpose and audience scope are recorded.

## Local-first capture
Use browser `navigator.mediaDevices.getDisplayMedia` following user gesture and permission prompt; `MediaRecorder` for tab/window stream. Browser support and supported codecs must be detected at runtime. App cannot silently take over or script arbitrary tabs. Canvas rendering/screenshots of cross-origin tab content are not generally available without extension/remote controlled browser; use a permitted recorded frame for a still when necessary.

## Remote capture (Phase 2)
Isolated Chromium session per user (Playwright and live-stream UI), no reused browser profiles. Capture full-page or viewport screenshots; video session recordings; record approved repeatable navigation flows using stable locators. Never store usernames/passwords in workflow steps. Configure 1 concurrent session initially, idle timeout, memory/CPU/time cap. Allow only approved targets, resolve DNS repeatedly, enforce nonpublic-address/metadata egress blocks and outbound proxy restrictions to prevent SSRF.

## Asset pipeline
Ingest -> virus/mime check -> hash -> metadata -> redact/mask -> optional crop/transcode -> immutable version -> project association. Preserve original in restricted area only when authorized; public-ready assets require privacy approval. Metadata: source URL (strip query secrets), timestamp, viewport, DPR, frame rate, duration, capture method, operator, rights, privacy status.

## Acceptance
Authorized ALMS demo flow can be captured in test account using synthetic patient records, previewed and inserted into a video scene. Stop permission ends stream; session cookies destroyed. No data exfiltration to unauthorized hosts. Capture logs never contain credentials.
