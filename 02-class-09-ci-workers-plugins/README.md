# 02 — Class 09: CI Workers, Plugin Authoring & Project 2

**Course:** Panaversity FDE-102, class 09 (recorded 2026-10-25, 1h52m) — [video](https://www.youtube.com/watch?v=SoJ_yL7sFt4)
**Source for these notes:** my own Urdu Whisper transcript (`transcript/`, local only). Auto-transcription has errors, so names and small details may be slightly off.

## Goal

Understand how Claude Code runs **outside my machine** as a CI worker (GitHub Actions), what **plugins** are and how to package and ship one, and start **Project 2: Email Marketing Studio**.

## Class map

| Time | Topic |
|---|---|
| 0:00 | Recap: Claude.ai web → Claude Code (20% of the exam) → skills, MCP connectors, rules, CLAUDE.md → running Claude off your machine |
| 0:01 | What CI is: run a script automatically when an event happens |
| 0:05 | Soup analogy: a house rule "taste every new ingredient"; GitHub Actions is the helper robot that tastes |
| 0:07 | The robot's special power is a **virtual machine**, and Claude Code can run inside it |
| 0:13 | Every workflow has 4 parts: **name, trigger, machine (`runs-on`), steps** |
| 0:15 | The 3 robots in the demo repo: Markdown check, `@claude` (issue → PR), reviewer (on PR) |
| 0:23 | Every robot gets **its own fresh VM**, which is destroyed after the run |
| 0:24 | **The 5 kinds of trigger** (see below) |
| 0:27 | Demo: a **scheduled "daily lesson" robot** (9 PM Pakistan time) |
| 0:37 | Inside it: `--max-turns 40`, allowed tools, and the **prompt** (the most important part) |
| 0:43 | Why it matters for an architect: the same harness runs anywhere (GitHub, cloud, containers, other agents) |
| 0:52 | **Plugins**: packaged, reusable domain expertise |
| 0:54 | `/plugins` in Claude Code: installed, discover, marketplaces, install scope |
| 1:01 | Skills vs plugins side by side; the "skill creator" vs the plugin-authoring skill |
| 1:04 | Why the Zia Tutor AI zip (skill + MCP) is **not** a plugin: it isn't packaged |
| 1:10 | Demo: use **plan mode** to stop Claude jumping straight into building |
| 1:13 | Packaging = adding one `plugin.json` (name, version, description) |
| 1:16 | Shipping: push to GitHub with `gh` → anyone can `/plugin install` it |
| 1:20 | **Weekly challenge** |
| 1:21 | **Project 2: Email Marketing Studio**, steps 1–2 shown live |
| 1:38 | Deadline and the project's parts |
| 1:41 | Q&A |

## Key ideas

### CI workers
- **CI** = when an event happens (push, PR…), automatically run a workflow. The instructor's version: "taste every new ingredient before it goes into the soup."
- A GitHub Actions runner is a **VM**, so it can run *anything*, including Claude Code, OpenCode, Codex, or a call to any LLM API.
- Every workflow has **name → trigger → machine → steps**.
- `/install-github-app` sets up the Claude robots automatically. With OpenCode you build the same thing by hand with GitHub Actions.
- Each run gets a **separate, fresh VM** that is destroyed afterwards. Robots never share a machine.
- In CI, Claude authenticates with your **subscription token** (stored as a secret by the installer) and uses the default model (Sonnet 5.5 in the demo).
- One check can be **required** and another **optional** (the demo: Markdown required, review optional).

### The 5 triggers
1. **GitHub events**: push, PR opened…
2. **Mentioning `@claude`** in an issue or comment
3. **Schedule** (cron): "run every day at 9 PM"
4. **Manual run** (`workflow_dispatch`): for testing
5. **External system**: another app or agent triggers it. This is covered later, when building MCP servers.

### The scheduled "daily lesson" robot (demo)
- Prompt idea: *"Make a workflow that adds one concept lesson Markdown file to the repo daily at 9 PM."* Claude first explained the plan, then asked 4 questions:
  1. PR or straight to `main`? → **PR + auto-merge**, because the ruleset blocks direct pushes
  2. Topic list or Claude decides? → Claude decides
  3. Timezone? → Pakistan, 9 PM (cron runs in UTC, so watch the timezone)
  4. What if yesterday's lesson didn't merge? → doesn't matter
- Key settings in the workflow file:
  - `--max-turns 40`: the most **agent-loop steps** Claude may take. It works as a spending limit.
  - **allowed tools**
  - **prompt**: the most important part, because it decides what Claude does
- The test run (manual) created a "Tool use and agentic loops" lesson, opened a PR and merged it.

### Plugins
- A **plugin** packages **any or all** of: **skills**, **connectors (MCP)**, **hooks**, plus extras like commands and a status line.
- Plugins work in Claude Code and on the web.
- `/plugins` → see what's installed, **discover** new ones, browse **marketplaces** (e.g. Anthropic's directory) → review → install at **user / project / local** scope. Project scope is written to `.claude/settings.local.json`.
- Analogy: skills have a marketplace (`skills.sh`) and a skill-making skill (skill creator). Plugins have marketplaces and a **plugin-authoring skill**.
- **Packaging is easy**, because it's mostly one `plugin.json`. **The real work is the skills and the MCP connector** underneath: do they work, and how reliable are they? Evals come later.
- Order of work: **build and test skills/MCP → package as a plugin → ship.**
- Tip from the demo: if Claude jumps into building when you only wanted to discuss, use **plan mode** (Shift+Tab) or say *"this is a discussion-only session; ask me questions with the AskUserQuestion tool."*

## What the instructor asks me to do

| # | Task | Due |
|---|---|---|
| 1 | **Practise the CI workers**: the demo repo and prompts are shared. Exam topic. | This week |
| 2 | **Explore plugins**: run Claude Code → `/plugins` → Marketplaces → Anthropic directory; review and install one | This week |
| 3 | **Challenge: ship Zia Tutor AI as a plugin yourself**, packaging its skill + MCP connector with the plugin-authoring skill | This week |
| 4 | **Project 2: Email Marketing Studio** (below) | **~8 Nov 2026** (class was 25 Oct; "two weeks", the instructor said the 8th) |

### Project 2: Email Marketing Studio
- Build a small email-marketing studio **run by Claude Code** (or OpenCode). It plans a campaign, writes the emails, personalises them per target customer, and can optionally send them through an email service.
- Example idea from class: you know how to manage an AI workforce (learned at the start of the course) and want to reach people by email to offer it.
- The **step-by-step guide** (with a prompt plus a **check prompt** for every step) is in the project document on the **dashboard**.
- Per-step loop: understand the step → give Claude the prompt → run the check prompt → ask questions → next step.
- Prerequisites: Node, git, **gh CLI**, Claude Code.
- Project layout: `CLAUDE.md`, `brand.md` (or a rule), `leads/`, `campaigns/`, `notes/`, `.claude/` (hooks, skills, rules), `.mcp.json`, `scripts/`
- Shown in class:
  - **Step 0:** make the `email-marketing-studio` folder, start Claude, first prompt: *create a GitHub repo and push this project* (public).
  - **Step 1:** research the suggested skills on `skills.sh`. Write a notes file saying which to use and why, and which not to. **Plan only, no installing.** Then run the check prompt.
  - **Step 2:** install Anthropic's **marketing plugin**, create the folder structure (brand, leads, campaigns, notes), write `CLAUDE.md` and `.gitignore`, push. Then check.
- Parts:
  - **Part 1:** basic setup
  - **Part 2:** a live service, plus safety and security
  - **Part 3:** automation, better workflows, resuming and forking sessions, automated reviews, packaging the studio as a plugin
  - **Part C:** extra challenges
  - Optional extras
- The guide maps every step to **exam objectives**.
- Tip: in Claude Code, `!` + a command runs it in the shell, e.g. `!code .`

## How this maps to what I already did (task 01)

| Class demo | Me |
|---|---|
| Markdown robot (required check) | ✅ `markdown-only.yml` + "Protect main" ruleset |
| `@claude` robot (issue → branch → PR) | ✅ Issue #6 → PR #7 |
| Reviewer robot (optional check) | ✅ `claude-review` on every PR |
| Scheduled robot + manual run | ⬜ Not yet. Next practice. |
| Plugins / marketplace | ⬜ |
| Project 2 | ⬜ |

## Q&A highlights
- **Desktop app vs CLI:** both work on your local files; use whichever you like.
- **Vibe coding vs production:** fine for demos and MVPs, but shipping to production needs an architect's decisions. That skill is the point of the course.
- **Is my Zia Tutor setup (connector + skill) a plugin?** No. It's components added to Claude, not packaged. Adding the skill improves teaching quality.
- **A project works in one folder but not another:** put both on GitHub first, then ask Claude to compare them, including what's in each project's memory.

## Files in this folder
- `transcript/class09_ur.ts.txt`: Urdu transcript with `[h:mm:ss]` timestamps (local only)
- `scripts/run_transcription.sh`, `scripts/merge_chunks.py`: re-runnable, resumable Whisper pipeline (local only)
