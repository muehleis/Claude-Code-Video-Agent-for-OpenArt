# 🎬 AI Video Agency (OpenArt Edition) — Skill for Claude Code \& Coding Agents

## Adaptiert von https://github.com/Arnie936/ai-video-agency (Higgsfield Version)

## YouTube Video unter https://www.youtube.com/watch?v=ktX_L2b9txI

## Schnellstart ohne Vorwissen (copy \& paste)

Du brauchst **kein technisches Vorwissen**. Öffne deinen Coding Agent (z.B.
[Claude Code](https://claude.com/claude-code)) und füge einfach diesen Text ein:

> Klone das Repo in diesen Projektordner und erklär mir, was es macht und was
> die nächsten Schritte sind. Verbinde den OpenArt MCP Server
> (`claude mcp add --transport http openart https://mcp.openart.ai/mcp`),
> installiere ffmpeg und richte alles so ein, dass es klappt. Wenn ich noch
> keinen OpenArt-Account habe, zeig mir wo ich einen erstelle.

Der Agent erledigt den Rest: Er richtet alles ein, prüft was fehlt und führt
dich Schritt für Schritt zum ersten Video. Einen OpenArt-Account (die
Bild-/Video-KI dahinter) erstellst du hier: 👉 **https://openart.ai**

\---

## How it works

!\[Workflow](workflow-diagram.png)

Why the character sheet matters — the failure mode this skill was built around:

!\[Character Drift](drift-diagram.png)

Turn **one photo** into a complete cinematic AI video story — with a consistent
main character, a real narrative arc, and seamless transformation morphs. This
skill makes your coding agent behave like a full-service AI video agency: it
interviews you, writes the script, generates every asset, and assembles the final
video.

**Example output pipeline (\~45 seconds of video):**

```
You (photo) ──morph──▶ Caveman ──▶ 3 cinematic story scenes ──morph──▶ You (photo)
```

Built on the [OpenArt](https://openart.ai) MCP server (GPT Image 2 + Seedance 2.0)
and battle-tested against the classic failure mode of multi-scene AI video:
**identity drift**.

## ✨ What it does

* **Onboarding like an agency**: give it your start photo — it analyzes pose,
lighting and features, then asks what story you want to tell, how many scenes,
how long the clips should be.
* **Character sheet**: generates a multi-angle identity reference from your photo
(the key trick that keeps your face consistent across every scene).
* **Transformation bookends**: first-frame/last-frame morph videos that transform
you into your character and back at the end.
* **Story scenes**: independent cinematic clips (default 3×10s) with a
hook–struggle–triumph arc, all referencing the character sheet.
* **Assembly**: concatenates everything into one final video with ffmpeg.

Works for personal story videos, YouTube hooks, Shorts/TikTok, ads and brand
stories.

## 🚀 Setup

1. **Get an OpenArt account** (the generation backend):
👉 https://openart.ai
2. **Connect the OpenArt MCP server** to Claude Code:

```bash
   claude mcp add --transport http openart https://mcp.openart.ai/mcp
   ```

Then run `/mcp` inside Claude Code and complete the OpenArt login in your
browser.

3. **Install this skill**:

```bash
   npx skills add <your-fork>/ai-video-agency
   ```

…or copy the `ai-video-agency/` folder into your agent's skills directory
(e.g. `\\\~/.claude/skills/`).

4. **Install ffmpeg** (frame extraction + final assembly):
`winget install ffmpeg` (Windows) / `brew install ffmpeg` (macOS) /
`apt install ffmpeg` (Linux).

## 🎯 Usage

Just tell your agent something like:

> "I want to turn myself into a viking and tell an epic 30-second story. Here's my photo."

or simply:

> "Make an AI video story from this photo."

The skill takes over: analysis → concept pitch → script sign-off → generation →
final video.

## 🧠 The method (why this works)

Most multi-scene AI videos fall apart because the character stops looking like the
same person. This workflow fixes that with three principles:

1. **Character sheet as identity anchor** — every scene references a multi-angle
sheet of your face (plus your in-costume look) instead of chaining video frames.
Frame chaining compounds drift; after 6 chained scenes the character is a
different person. Independent cuts + one shared reference stay stable.
2. **Pose symmetry** — the story's final scene deliberately ends in the same pose
as your start photo, so the reverse morph lands perfectly.
3. **First/last-frame morphs** — transformations use Seedance 2.0's
`startFrame`/`endFrame` (image2video mode), with a motivated trigger
(finger snap) and explicitly enumerated changes.

## 📁 Structure

```
ai-video-agency/
├── SKILL.md                      # the agent workflow
├── README.md
└── references/
    ├── prompting-guide.md        # proven prompt templates per phase
    └── troubleshooting.md        # identity drift, API errors, morph fixes
```

## 💳 Costs

Generation runs on your OpenArt account credits. A full production
(character sheet + target image + 2 morphs + 3×10s scenes) is a handful of
image jobs and \~40s of Seedance 2.0 video. Ask your agent to quote a job with
`openart\\\_model\\\_cost` before generating if you want exact numbers.

## 📜 License

MIT — use it, fork it, ship videos with it.

\---

*This is an OpenArt adaptation of*
[*Arnie936/ai-video-agency*](https://github.com/Arnie936/ai-video-agency)*,
which was originally built on the Higgsfield CLI.*

