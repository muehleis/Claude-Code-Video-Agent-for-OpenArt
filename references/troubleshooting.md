# Troubleshooting

## Identity drift (character stops looking like the user)

Symptoms: after several scenes the face is more muscular / older / generic.

Causes & fixes:
- **Frame chaining** (last frame of scene N as start image of N+1) compounds drift.
  Fix: independent scenes, each referencing the character sheet via
  `element2video` + `visualReferences`.
- Weak reference: regenerate the character sheet with concrete facial features
  spelled out in the prompt.
- Too many scenes: prefer 3×10s over 6×5s.
- If one scene drifted badly, regenerate just that scene — never chain a drifted
  frame forward into a morph.

## OpenArt MCP errors

| Error | Fix |
|---|---|
| Param validation error (unknown field / invalid enum) | Call `openart_model_form_get(model, mode)` and use only declared fields. GPT Image 2: `resolutionTier` is `1k/2k/4k`; Seedance video: `resolution` is `480p/720p/1080p/4k`, `duration` 4–15s |
| `mcp__openart__*` tools missing | Add the server: `claude mcp add --transport http openart https://mcp.openart.ai/mcp` |
| Not authenticated / session expired | User runs `/mcp` in Claude Code and re-authenticates with OpenArt |
| Generation stuck / `openart_creation_wait` returns STILL_RUNNING | Not a failure — call `openart_creation_wait` again with the same `historyId`; 10s Seedance clips can take several minutes |
| Generation FAILED | Read the error message from `openart_creation_wait`; often a content-policy or reference-image issue — adjust prompt/reference and resubmit |
| Not enough credits | Check balance with `openart_account_get`; quote jobs first with `openart_model_cost`; consider Seedance 2.0 Fast/Mini or `mode: "fast"` |
| Upload rejected | `size` passed to `openart_upload_sign` must exactly match the bytes you PUT; set the same `Content-Type` header on the PUT |

## Morph looks bad / jumps

- Start and end image must share pose and framing. If the target image changed the
  pose, regenerate it with "keeping their exact same pose ... and camera framing".
- The final story scene must end near the anchor pose; extract the real last frame
  with `ffmpeg -sseof -0.1` (not a random mid-frame) as the morph start.
- Enumerate transformations in the prompt; vague "transforms into X" produces
  cross-dissolves instead of morphs.
- Wrong mode: morphs need `image2video` with `startFrame` + `endFrame`. The
  `element2video` mode treats images as identity references, not frames — it will
  not lock the first/last frame.

## Assembly issues

Clips from different jobs can differ in resolution/fps. Always re-encode on concat
(`-c:v libx264 -crf 18`) instead of `-c copy`.

## Frame extraction

```bash
# true last frame
ffmpeg -y -sseof -0.1 -i clip.mp4 -frames:v 1 -q:v 2 last.png
# first frame
ffmpeg -y -i clip.mp4 -frames:v 1 first.png
```
