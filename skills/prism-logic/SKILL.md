# Prism Logic

A universal reasoning-and-taste layer for coding agents. Two halves, one mode: **verified code** and
**deliberate craft**.

Prism Logic doesn't replace your agent. It upgrades how your agent works when the task is worth the
effort — plan before building, audit its own output as a hostile reviewer, run a per-language
checklist, and pass judgment on anything visual instead of calling it "fine."

---

## What it does

**Section 1 — Smarts / Coding**
- Plan before building; verify against edge cases before delivering
- Hostile second-pass self-critique ("this is wrong / this is fragile") before you ever see the code
- Per-language audit checklist: idioms, memory/ownership model, toolchain, error handling,
  version gotchas, classic footguns
- Deep notes for C++, Rust, Lua, Python; unknown languages researched on the web rather than bluffed
- Catches contradictions between what you asked for and what's achievable
- States assumptions and flags uncertainty instead of asserting confidently

**Section 2 — Creativity**
- Taste checklist for visual, motion, 3D, animation, and game work: spacing, color & contrast,
  hierarchy, easing & pacing, density, consistency, intentionality
- Never says "looks good" without naming what it judged
- Learns your taste within a conversation and applies it going forward
- Cost-aware visual inspection: **fewer screenshots** by default, video capture when motion is the
  thing being judged, blind only with a mandatory workaround

**Guardrails:** no unnecessary code, no gold-plating, no scope creep, no second-guessing you.
If something genuinely won't work, it warns once, asks once, and then builds it your way.

---

## Install

Prism Logic is a standard agent skill. Use the [`skills`](https://github.com/vercel-labs/skills) CLI —
it detects your agents and installs to all of them, including Claude Code, Codex, Cursor, Gemini CLI,
Copilot, Windsurf, Zed, and OpenCode.

```bash
# preview what's in the repo first
npx skills add <you>/prism-logic -l

# install globally to every agent it detects
npx skills add <you>/prism-logic -g

# or target one agent, non-interactively
npx skills add <you>/prism-logic -g -a claude-code -s prism-logic -y

# try it without installing (generates a one-shot prompt)
npx skills use <you>/prism-logic@prism-logic --agent cursor
```

Useful flags: `-g` global instead of project-level · `-a <agent>` target · `-s <skill>` pick one ·
`-l` list before installing · `-y` skip prompts.

> Prefer project-level installs (drop `-g`) for team repos. Only use `-y` on sources you've read —
> non-interactive flags install instructions an agent will later obey.

### ChatGPT / other chat UIs

No file-based skill system — paste the contents of `skills/prism-logic/SKILL.md` into a
**Project's instructions** (not Custom Instructions; the limit is too small).

### Raw API

Use `skills/prism-logic/SKILL.md` as the system prompt.

---

## Usage

Engage it by name or with a trigger phrase:

> "Prism Logic, build me a rate limiter in Rust"
> "deepthink mode: refactor this C++ allocator"
> "use your brain on this UI"

On coding tasks it asks once whether to run in deepthink mode, then remembers your answer for the
rest of the conversation. Say **"no, don't ask again"** to stop the prompt, or **"yes, always"** to
auto-engage.

Creative work engages the same mode — there's no second toggle.

---

## Configuration

The top of `SKILL.md` has a hand-editable `PREFERENCES` block. It's read as ground truth at the
start of every conversation; anything you say in chat overrides it for that conversation only.

```yaml
deepthink_on_coding: ask          # ask | always | never
visibility: short_summary         # silent | short_summary | max_context
languages: [C++, Rust, Lua, Python]
unknown_language_policy: research_web
visual_inspection_default: fewer_screenshots
guardrails: [no_unnecessary_code, no_gold_plating, no_scope_creep, warn_once_then_comply]
taste_profile: |
  (your taste notes here)
```

**Skills are stateless across chats.** Your in-chat preferences and taste log do not survive into a
new conversation. To keep them, either edit this block yourself or ask Prism Logic to emit an
updated `PREFERENCES` block and paste it in.

---

## Repo layout

```
prism-logic/
├── README.md
├── LICENSE
└── skills/
    └── prism-logic/
        └── SKILL.md
```

The `skills/<name>/SKILL.md` shape is what lets `npx skills` discover this repo as a skill
collection. Don't move `SKILL.md` to the repo root — that changes how the CLI parses it.

`SKILL.md` must keep its YAML frontmatter. The `description` field is what the loader reads and what
the model uses to decide whether to invoke the skill — narrow it and the skill never fires; widen it
and it fires on every message.

---

## Contributing

Keep `SKILL.md` under ~300 lines. Heavy reference material belongs in a sibling `references/`
directory, loaded on demand. Bump `version` in the frontmatter and add a CHANGELOG entry for
behavioral changes — people need to know when the taste checklist or guardrails shift.

---

## License

MIT — see [LICENSE](LICENSE).

Built by [<your-name>](https://github.com/<you>).
