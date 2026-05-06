# Qualixar Media CDN

Public CDN for Qualixar marketing media. Serves video files via `raw.githubusercontent.com` for IG Reels Graph API and any tool that requires unauthenticated public URLs.

## Why this exists

IG Reels Graph API requires `video_url` to be publicly fetchable with no authentication. FB Page CDN URLs are token-gated and rejected (error `2207076`). This repo solves that with free public GitHub raw URLs.

## Structure

- `shorts/agentassert/` — AgentAssert ABC explainer Shorts/Reels (60s, 9:16)
- `shorts/<topic>/` — additional content as it ships

## URL pattern

```
https://raw.githubusercontent.com/qualixar/media-cdn/main/shorts/<topic>/<filename>
```

## Pushing media — use `publish-reel.mjs`

Don't push manually. Use:
```bash
node Agentic_official/social-media-varun/ops/meta/publish-reel.mjs \
  --account qualixar --file ./your-video.mp4 \
  --caption "..." --topic <topic-folder>
```

It encodes for IG specs, pushes here, and posts the Reel in one command.

## Encode recipe (if pushing manually)

```bash
ffmpeg -y -i INPUT.mp4 \
  -c:v libx264 -profile:v main -level 4.0 -pix_fmt yuv420p \
  -color_primaries bt709 -color_trc bt709 -colorspace bt709 -color_range tv \
  -movflags +faststart -brand mp42 \
  -c:a aac -b:a 128k -ar 44100 -ac 2 \
  OUTPUT.mp4
```

## Limits

GitHub raw is fine for files ≤ 100 MB. For larger files, use Azure Blob (`gtic-resource` Foundry sub) — see `social-media-varun/ops/meta/REELS-PLAYBOOK.md`.

## License

Public marketing content. Not for commercial redistribution.
