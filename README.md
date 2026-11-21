# kickoff

A Claude Code skill that turns an app idea you have already talked through into a project a team of AI agents can start building.

It is for people who build apps by directing AI agents, not by writing code themselves.

## What it does

`kickoff` does not interview you from scratch. It reads what you already decided in the conversation, then:

1. Writes `IDEA.md`: your settled decisions, so nothing is lost if the session closes.
2. Checks six things: who uses the app, its core job, its screens, the data it stores (and what is private), the stack (the tools it is built with), and what is out of scope. It asks one simple question for each gap.
3. Recommends a default stack (Next.js + Supabase, hosted on Vercel) and asks for a yes.
4. Shows a one-screen summary and waits for your approval.
5. Writes `SPEC.md` (what the app is, plus things that must never happen) and `PLAN.md` (small tasks, each with the files it owns, what it needs first, and a check that proves it is done).
6. After a second approval: sets up git, a `.gitignore`, a project `CLAUDE.md`, a starter app from the stack's official generator, and a first commit.
7. After a third approval: loads the plan's tasks into Beads, the task tracker that `agent-team` reads.
8. Tells you exactly what to run next.

It never reads or commits secrets, never installs global tools, never overwrites existing files, and never builds features. It only acts on a clear yes.

## Where it fits

```
is-this-a-good-idea  ->  grilling + domain-modeling  ->  kickoff  ->  agent-team
 should I build it?       what exactly do we build?      set it up     build it
```

## Install

Create the skills folder if it does not exist, then clone the repository:

```sh
mkdir -p ~/.claude/skills
git clone https://github.com/thebpandey/kickoff ~/.claude/skills/kickoff
```

On Windows (PowerShell):

```powershell
git clone https://github.com/thebpandey/kickoff "$env:USERPROFILE\.claude\skills\kickoff"
```

No git? Click "Download ZIP" on the repository page, unzip it, and rename the folder to `kickoff` inside your skills folder.

Then restart Claude Code or open a new session. After grilling your idea, type `/kickoff`.

To update later, run `git pull` inside that folder.

## Requirements

- Claude Code
- git
- Node.js, for the default starter app (without it, `kickoff` skips the starter app and says so)
- Optional: [Beads](https://github.com/gastownhall/beads) (`bd`), tested with 1.2.2. Without it, `PLAN.md` is your task list.

## Files

- `SKILL.md`: the instructions Claude follows.
- `references/templates.md`: the shapes of `SPEC.md`, `PLAN.md`, and the project `CLAUDE.md`.

## License

MIT. See `LICENSE`.
