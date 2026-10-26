# Governed autonomy

> One person, one Mac, and an AI operation with better change control than most funded startups.

![License: MIT](https://img.shields.io/badge/license-MIT-blue) ![Built with Claude Code](https://img.shields.io/badge/built%20with-Claude%20Code-d97757)

A friend asked "how did you set everything up?" and the honest answer was too long for a text message, so this is the answer in repo form: the method I run my AI operation on, and the build order that got me here. It is the system behind [moonops.org](https://moonops.org) and my portfolio, built in the margins between June and August 2026. Every rule in it was paid for by a real mistake first, and the mistakes are included.

```text
                     you (the operator)
                          │  verdicts, approvals, taste
        ┌─────────────────┴──────────────────┐
        │  operator manual                   │  read at every session start
        ├────────────────────────────────────┤
        │  memory · skills · agent charters  │  what the AI knows and may do
        ├────────────────────────────────────┤
        │  hooks · scanners · publish gates  │  rules enforced as code
        ├────────────────────────────────────┤
        │  ledger · task board · daily sync  │  what keeps it honest
        ├────────────────────────────────────┤
        │  learning loop                     │  corrections fold back in
        ├────────────────────────────────────┤
        │  backup · quarterly restore drill  │  proven recoverable
        └─────────────────┬──────────────────┘
                          │  stage, then fire
                  public surfaces
```

**In this repo:**

| File | What it is |
| --- | --- |
| README.md | The method (six rules) and the build path (ten stages) |
| [templates/operator-manual.md](templates/operator-manual.md) | A fill-in skeleton for your own operator manual, the first thing to build |
| [examples/first-hook.md](examples/first-hook.md) | A complete working first hook you can install in ten minutes |
| [CHECKLIST.md](CHECKLIST.md) | The starting path as checkboxes |

**Contents:** [What you need](#what-you-need) · [What I built](#what-i-built) · [The method in six rules](#the-method-in-six-rules) · [The build path](#the-build-path) · [If you are starting this week](#if-you-are-starting-this-week) · [The one-liners](#the-one-liners)

## What you need

- **Claude Code**, Anthropic's terminal agent: `npm install -g @anthropic-ai/claude-code`, then run `claude` in any folder. It requires a paid Claude plan; the middle tier is plenty to start.
- **git and a GitHub account**, needed from stage 4 onward for secret scanning and backups.
- **A scheduler**, eventually. Claude Code has scheduled tasks built in, and macOS launchd or plain cron both work. Nothing before stage 5 needs one.
- **Time.** This took me about three months of steady side-time. The first useful pieces, the manual and the first skill, pay for themselves inside a week.

## What I built

The short inventory, so you know where this path ends up:

- **An operator manual** the AI reads at the start of every session: who I am, what it may never do, how I like to work.
- **A persistent memory**: one fact per file, with the why attached, so lessons survive the conversation that taught them.
- **A library of skills**, about 35 as of August 2026: repeatable workflows I proved by hand first, then wrote down.
- **A team of AI agents with charters**: a researcher, a reviewer, a verifier, a builder, each with a defined lane and things it is forbidden to do.
- **Guardrails as code**: hooks that block a bad write at the moment it happens, secret scanners, style linters.
- **An operations layer**: an append-only ledger, a task board, a daily sync, and a fleet of scheduled jobs with health rules.
- **A learning loop**: my corrections get captured and folded back into the skills, so I never make the same correction twice.
- **A tested backup**: private mirror, tripwires, quarterly restore drills.
- **Public surfaces with a ship pipeline**: the website, the portfolio, and a toolkit that publishes on a written grant, each behind pre-publish gates.

The theme across all of it: I run AI as a governed workforce rather than a chat tool. Trust in AI is a property you engineer, with permissions, gates, verification, and audit trails, and the engineering turns out to be the fun part.

## The method in six rules

This is the method, with the failures that taught it attached.

### 1. Decide what to believe

AI is confidently wrong in ways that sound right, and systems fail silently. Never trust a claim you have not checked on the live surface. An empty search result means "unknown", never "it does not exist". A success message is an event, not a state: my publish pipeline once reported "published" twice while the server's own count read zero. The toast described the click; the count described the world.

Instruments lie by omission too. A one-character typo once silenced an analytics beacon for a week, and the dashboard read as a clean, plausible zero the entire time. A monitor that reports nothing might be broken rather than quiet, so prove your instruments are alive before you trust their silence.

### 2. Decide what AI may do

Classify actions by blast radius rather than difficulty. Moving money, touching credentials, and deleting permanently are never delegated. Sending messages to real people and publishing publicly sit one notch down: human by default, delegable only through a written grant with a named scope, an expiry date, and a one-word kill switch. For everything else the working mode is stage-then-fire: the AI does all the work, and I perform the one irreversible click.

### 3. Enforce rules with code

A rule that lives in a document decays; a rule that lives in a hook fires every time. My style rules, secret protections, and publish gates are scripts that block the action and print the offending line. Then test the guards themselves. I once had a test stay green after the protection it guarded was deleted, because the keyword it matched survived in a nearby comment. Now every guard gets its failure put back on purpose, and I watch the alarm fire before I trust it. When a filter holds something back, that is the guard working: read the match, and never reword past it.

### 4. Teach the system instead of repeating yourself

A workflow becomes a skill only after it has worked twice by hand, and the skill records the judgment (what good looks like, when to reject a draft) rather than just the steps. Corrections get captured and folded back into the owning skill with the evidence attached, so no correction is made twice. And keep one canonical copy of every rule, with everything else pointing at it. I learned that one after a single voice rule quietly forked into three copies that disagreed with each other.

### 5. Keep the operation honest

One append-only ledger records everything that happens. Append-only means lines are added and never edited, so it cannot lie about order. A daily sync reconciles it against reality and closes items only on live evidence. Decisions become standing rulings with dates so they stay decided: an automated audit once reversed a decision I had made 75 minutes earlier, because a deliberate removal looks exactly like breakage until you stamp it. Scheduled jobs are judged by their latest run in both directions: never declared dead off one red, and never declared healed until they succeed on their own schedule.

### 6. Protect your attention

Every AI output leads with the verdict in the first line. One question per ask, with a recommended default. The system carries its own state so I never have to remember where we were. Your attention is the scarcest resource in the whole loop; spend it on judgment.

## The build path

The order below is the order that worked, and the order matters more than the pace. Each stage assumes the previous ones exist.

### Stage 0: pick one real project

You cannot govern a hypothetical. Mine was a media company: a YouTube channel and its website, moonops.org, which later grew a portfolio wing. I set its direction and hold the quality gate, and everything it publishes runs under the written-grant system from rule 2. Pick something with real outputs and real consequences for shipping something wrong, even if the stakes are small.

### Stage 1: write the operator manual

Claude Code automatically reads a file named `CLAUDE.md` at the start of every session: project-level from the folder you launch in, personal-level from `~/.claude/CLAUDE.md`. That one mechanism is what turns a text file into standing policy, and it is why this stage comes first. Mine is organized in tiers, in priority order:

1. **Safety and irreversibility**: what may never happen, stated as hard lines with no exception path.
2. **Hard constraints**: the rules that override convenience.
3. **Workflow defaults**: where deliverables go, how dates are written, what a finished handoff looks like.
4. **Style and voice**: how you write, so drafts come back sounding like you.
5. **Who I am and what is running**: enough context that no session starts from zero.

There is a fill-in skeleton at [templates/operator-manual.md](templates/operator-manual.md). Write it before you automate anything; every later stage inherits it.

### Stage 2: build the memory

A memory is a small markdown file, one fact each, in a directory with a one-line index; point to that index from your `CLAUDE.md` and every session knows to read it. The discipline that makes it work: record the why and the how-to-apply, because a rule without its reason gets misapplied the first time context shifts. A real one of mine, lightly trimmed:

```markdown
# api-timestamps-are-utc
The mail API returns timestamps in UTC and I am in Eastern time.
Why: a 13:30Z reply logged as "1:30 PM ET" turned a 111-minute
reply into a fake 3-day silence in my records.
How to apply: convert to local time before writing any timestamp
into a durable file.
```

When something goes wrong, the lesson gets a file. When a preference gets stated twice, it gets a file. Absolute dates only, because "yesterday" is wrong by tomorrow.

### Stage 3: codify your first skills

A skill is a markdown file at `~/.claude/skills/<name>/SKILL.md`: frontmatter that says when it triggers, then the procedure and the quality bar. The whole shape:

```markdown
---
name: weekly-review
description: 'Gather the week, fill my review template, surface the
  top three. Use when I say "weekly review" or "wrap up the week".'
---

The procedure, the quality bar, and a link to one good past output.
```

Mine include a daily sync, a weekly review, and a publish pipeline. The bar I enforce: a loop becomes a skill only after it has run successfully twice by hand with documented results, because a skill written first is a guess with a slash command in front of it. Encode the judgment, not the click-path: acceptance criteria and a link to one example of good output survive every tool change, and button sequences do not.

### Stage 4: install guardrails as code

Hooks are shell commands wired into `~/.claude/settings.json` that run automatically before or after the AI acts; a hook that exits with code 2 blocks the action and shows the reason. This is the stage most people skip, and it is the one that makes delegation safe:

- **Hooks** on file writes and commands, blocking violations with the offending lines printed. Your first one takes ten minutes: [examples/first-hook.md](examples/first-hook.md).
- **Secret scanning** at multiple depths (before staging, in the pipeline, on the finished artifact), each layer assuming the one before it failed.
- **Style linting** on public-facing files only, so internal work stays fast.
- **A declared failure posture per guard**: publish gates fail closed (a hit means nothing ships), internal tooling fails open (a broken check never blocks normal work), records refuse malformed writes loudly.

Then test the guards: put a fake secret in a throwaway repo and watch the scanner catch it. Never trust a control you have not watched fail.

### Stage 5: stand up the operations layer

- **An append-only ledger**, one timestamped line per event, written through one script, that outranks every dashboard built from it.
- **A task board** holding only work that is actionable now, with dated work in a separate file under absolute dates.
- **A daily sync** that reconciles the ledger against live reality (mail, calendar, repos, schedulers) and produces a short brief with one recommended action.
- **Scheduled jobs** for the recurring work, each with a bounded window and a frequency cap.

### Stage 6: hire your AI team

An agent is a markdown file in `~/.claude/agents/` whose frontmatter lists the tools it may use, which is what makes separation structural: a reviewer defined with read-only tools cannot edit anything, however it is prompted. Give each agent a written charter (its lane, the sources it must load before speaking, what it is forbidden to do) and split maker from checker: the agent that drafts never validates its own work, and a standing verifier exists whose entire job is trying to refute claims before you act on them.

Mine grew into a full executive council of named personas, one vote each, a design veto a majority cannot override, ties escalating to me. You can skip the personas. Keep the separation of duties.

### Stage 7: close the learning loop

A hook captures each session's corrections and decisions at session end. A scheduled daily pass folds them back into the owning skills, inside clearly marked blocks, each change carrying a dated quote as its evidence. Anything consequential becomes a proposal for my review instead of an automatic edit: a self-improving system that can edit its own guardrails has no guardrails.

### Stage 8: back it up, then prove the backup

Everything durable mirrors to a private repo on a schedule, behind a tripwire that aborts the run if anything credential-shaped tries to cross. Then, quarterly, an actual restore drill: clone the mirror fresh and diff it against the live sources. An untested backup is a hope, not a backup.

### Stage 9: ship the public surfaces

The website and portfolio get their own pipeline: audit, fix, commit, push, then verify the live deploy byte-for-byte and re-run every check against the real URL, because a pass on localhost proves nothing about the live site. Public-facing content passes pre-publish gates (link integrity, content checks, style lint) before anything goes live. Publishing stays a human action unless a written grant from rule 2 covers it; everything up to the click is staged by the machine.

## If you are starting this week

The compressed version of the path, at a humane pace. The same list lives as checkboxes in [CHECKLIST.md](CHECKLIST.md).

1. **Day one**: install Claude Code, copy [templates/operator-manual.md](templates/operator-manual.md) into `~/.claude/CLAUDE.md`, fill in the safety tier honestly, then start a session and ask what rules it sees. Thirty minutes of honest writing beats any tool you could add today.
2. **Week one**: start the memory directory, and run your most annoying recurring workflow with the AI twice by hand. Write down what worked.
3. **Week two**: turn that workflow into your first skill, and install your first hook from [examples/first-hook.md](examples/first-hook.md).
4. **Month one**: stand up the ledger, the task board, and a daily sync. Add secret scanning before your first push to any repo.
5. **After that**: split roles into agent charters, add the learning loop, then the backup with a drill, then the public pipeline.
6. **Last, on purpose**: grant standing autonomy, in writing, with an expiry and a kill switch, only after the gates exist.

## The one-liners

The portable rules, in the form I say them to myself:

- Absence of evidence is a reason to go look.
- A silent instrument reads as zero.
- A success toast is an event, not a state.
- Verify on the real surface, the one your user will touch.
- A blocked file is the guard working.
- An untested backup is a hope.
- Codify only proven loops.
- Encode the judgment, not the click-path.
- Stage, then fire.
- Verdict in the first line.

---

Written by Elliott Storms, 2026-08-28, and maintained as I go. The long-form version lives in my private notes; this is the map. If you build your own version of this, open an issue and tell me what your first hook was. More at [moonops.org](https://moonops.org).
