# Project manifest specification · draft 1.0

## Principles
Data-controlled creative revisions must not require engine deployment. Bundle/code changes trigger controlled compilation and cache invalidation. All source, assets, voices and runtime versions are pinned when submitting renders. A manifest is a validated description, **not** a shell script or unrestricted permissions grant.

## Required sections
`schemaVersion`, `project` (ID/title/version), `source` (trusted-template/custom-code/imported GitHub revision), `composition` (name/fps/frame count and props), `assets` (immutable IDs/hash), `scenes` (stable IDs/frame spans), `audio` (voice/music references), `outputs` (variants), `execution` (request profile). Secrets, credentials and provider tokens belong in credential store, never manifest. Worker limits come from server policy, not untrusted request fields.

## Invariants
- Frame lengths positive; scenes sorted, contiguous and cover declared duration.
- Width/height/frame rate supported by template capability registry.
- All asset IDs visible within authorized workspace; SHA256 verified on import.
- Source snapshot pinned to repository commit+path or internal object revision.
- Audio rights, consent and approval markers included before publication.
- A render job references an immutable manifest snapshot and trusted bundle ID.

## Example (illustrative)
```json
{
  "schemaVersion": "1.0",
  "project": {"id": "alms-campaign", "title": "ALMS Film", "version": 1},
  "source": {"mode": "trusted-template", "templateId": "alms-journey", "templateVersion": "1.0.0"},
  "composition": {
    "id": "ALMS-Portrait", "fps": 30, "durationInFrames": 2700,
    "width": 1080, "height": 1920,
    "inputProps": {"product": "ALMS", "parentBrand": "ALIXA"}
  },
  "assets": [{"id": "asset-screenshot-001", "sha256": "REPLACE_WITH_VALID_64_HEX_DIGEST"}],
  "scenes": [
    {"id": "opening", "startFrame": 0, "durationInFrames": 360},
    {"id": "body", "startFrame": 360, "durationInFrames": 2160},
    {"id": "closing", "startFrame": 2520, "durationInFrames": 180}
  ],
  "audio": {"narrationAssetId": null, "musicAssetId": null},
  "outputs": [{"name": "portrait", "codec": "h264", "qualityPreset": "final"}]
}
```
Placeholder asset hashes in this documentation are NOT valid production manifests.

## Custom-code source
Use `source.mode=custom-code` with `snapshotId`, `entryPoint`, `lockfileHash`, `runtimeVersion`, `dependencyPolicyId`. Build must run in isolated unprivileged executor and persist immutable compiled artifact. Never allow uncontrolled remote npm install in renderer.

## Future contracts
A normative JSON Schema and OpenAPI definition are deliverables of Phase 0, with positive/negative fixture tests and backwards-compatibility policy. Do not treat this prose example as executable validation.
