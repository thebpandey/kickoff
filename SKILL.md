---
name: kickoff
description: "Turn an app idea that was already discussed and decided into a ready-to-build project: IDEA.md, SPEC.md, PLAN.md, a starter app, a project CLAUDE.md, a first git commit, and tasks loaded into Beads for agent-team. Use right after grilling (and domain-modeling) have settled what to build. Triggers: /kickoff, \"kickoff\", \"kick off the project\", \"turn this into a project\", \"set up the project\", \"we're ready to build\", \"write the spec and plan\"."
metadata:
  version: "0.1.2"
---

# Kickoff

You turn decisions the user already made into a project that a team of agents can start building. You do not run an interview of your own and you do not build product features. Follow the steps in order.

Words used here:
- **Repository (repo)**: a folder whose history git tracks.
- **Beads**: a task tracker for agents. Its command is `bd`.
- **Scaffold**: the empty starter app plus the project files around it.
- **Acceptance check**: a test a reviewer can run or a result they can see that proves a task is done.

## Rules for the whole run

- Never read, print, or commit `.env` files, keys, or passwords. If you need one, tell the user where to put it.
- Never install global tools, and never change global settings (`~/.gitconfig`, `~/.claude`, system packages).
- Never overwrite a file or a repo that already exists. Never re-run `git init` in a repo.
- Never start building features. That is the next stage.
- Before each write, get the approval that belongs to it (steps 4, 6, 7). Only a clear yes counts. Silence, "maybe", or a new topic is not a yes: stop and wait.
- Ask one question at a time. Give 2 to 4 options, recommended option first, in plain words. Use tappable options when the host has them.
- Keep it small: every screen, table, and task must come from a decision the user made. Do not add "nice to have" items. Smaller is better when two plans both work.

## Step 1: Collect what was decided

1. Read this conversation for decisions from `is-this-a-good-idea`, `grilling`, and `domain-modeling`.
2. In the project folder (the current folder), read `IDEA.md` and `GLOSSARY.md` if they exist. Use the glossary's words everywhere you write.
3. If there is no earlier discussion and no `IDEA.md`, say: "I have nothing decided to work from. Run `grilling` on your idea first, then run `kickoff` again." Then stop. Do not interview from zero.

## Step 2: Write IDEA.md

Running `kickoff` is the approval for this one file.

- If `IDEA.md` does not exist, write it: a title, then a short bullet list of settled decisions, one line each, plus a "Still open" list. Use the user's words. No new ideas.
- If it exists, show the lines you would add or change and ask before editing it.
- Whenever a question in step 3 is answered, add that decision to `IDEA.md`.

## Step 3: Fill the gaps

Check that `IDEA.md` answers each item below. For each item that is missing, vague, or contradicts another, ask one question, then the next.

1. **User**: who uses the app, and who logs in.
2. **Core job**: the one thing it must do well.
3. **Screens**: the pages people see.
4. **Data**: what it stores, and which of it is private to whom.
5. **Stack**: the tools it is built with.
6. **Out of scope**: what this version will not do.

For **stack**, do not ask the user to design one. Ask:

> I recommend **Next.js + Supabase, hosted on Vercel**. It is the most widely used and documented setup for this kind of app, it is free to start, it deploys in one click, and Supabase gives you login and per-user data rules out of the box. Use it?
> 1. Yes, use it (recommended)
> 2. I need a specific tool (tell me which)

If the user names another stack, record it and use that stack's official starter command in step 6. If you do not know one for certain, say so and skip the starter app.

## Step 4: Summary and first approval

Send the summary first, as its own assistant message of normal reply text, before you ask anything. One screen, no more than about 25 lines: user, core job, screens, data and privacy, stack, out of scope, and the "must never happen" rules you will write (always include: one user never sees or changes another user's private data; secrets never appear in code, browser, or git; plus any rule this app needs).

Never put the summary inside a tool call, a question dialog, an option preview, a status line, or a thinking block. Never replace it with a line such as "here is the summary".

Ask only after that message, as a separate message (or after the summary text in the same reply if the host has no tappable options):

> Write SPEC.md and PLAN.md from this?
> 1. Yes, write them (recommended)
> 2. Change something first

On "change", fix the item, update `IDEA.md`, and show the summary again in full. If the user says they cannot see the summary, print it again in full as plain text.

## Step 5: Write SPEC.md and PLAN.md

Use the templates in [references/templates.md](references/templates.md). If either file exists, show the differences and ask before replacing it.

**SPEC.md** says what the app is: user, core job, screens, data and privacy rules, must-never-happen rules, out of scope.

**PLAN.md** says how to build it: the stack, then ordered tasks. Each task has:
- an id (`T1`, `T2`, ...) and a short title;
- **Owns**: the files or folders it may change (two tasks that run at the same time must not own the same files);
- **Needs**: the task ids that must finish first (only earlier ids);
- **Parallel**: which tasks it can run beside;
- **Done when**: an acceptance check a reviewer can run or see. "Works well" is not a check.

Plan rules:
- A typical first version is 8 to 15 tasks. Each task is small enough for one agent to finish and check in one sitting.
- The starter app already exists after step 6, so no task sets it up again.
- Put the data tables and their privacy rules early. Include one task whose check proves the "one user never sees another user's data" rule.
- Mark at least two tasks that can run in parallel when the work allows it.
- End with a task that checks the whole core job end to end.

## Step 6: Scaffold (second approval)

1. Run `git rev-parse --is-inside-work-tree`. If it prints `true`, a repo already exists: use it and skip `git init`.
2. List the folder. If it holds anything besides `IDEA.md`, `GLOSSARY.md`, `SPEC.md`, `PLAN.md`, `docs/`, or `.git`, show the list and ask whether to continue here or stop. Never delete or move the user's files.
3. Ask:

> Set up the project now? I will: run `git init` (only if this is not a repo yet), add `.gitignore` and `CLAUDE.md`, create the starter app in `web/` with `npx create-next-app@latest web --example with-supabase --use-npm --disable-git --yes`, and make the first commit. No features get built.
> 1. Yes, set it up (recommended)
> 2. Not yet

4. On yes, and only for files that do not exist yet:
   - `git init` if needed.
   - `.gitignore` with at least: `.env*`, `!.env.example`, `node_modules/`, `.next/`, `.vercel/`. If one exists, add only the missing lines and say which.
   - `CLAUDE.md` from the template. If one exists, show what you would add and ask.
   - The starter app in `web/`. If Node.js is missing (`node --version` fails), skip this, say so, and keep going.
   - If the starter command downloads the app but its install step fails, run `npm install` once inside `web/`. If that also fails, say so in plain words, keep the downloaded files, and keep going. Do not change npm settings, and do not run lint, build, or tests here: checking the starter is the first plan task's job.
5. Run `git status --short` and check that no `.env` file (other than `.env.example`) is listed. If one is, stop and tell the user.
6. Commit: `git add -A && git commit -m "chore: project kickoff (spec, plan, starter app)"`. If git reports no user name or email, tell the user the exact `git config user.name` and `git config user.email` commands to run in this repo, and wait.
7. Tell the user: the database keys go in `web/.env.local`, copied from `web/.env.example`, and they fill it in themselves from their Supabase project.

## Step 7: Load tasks into Beads (third approval)

1. Run `command -v bd && bd --version`. If `bd` is missing, say: "Beads is not installed, so PLAN.md is your task list. Nothing else was changed." Skip to step 8. If the version is not 1.2.2, say it is untested and continue.
2. Ask:

> Load the N tasks from PLAN.md into Beads? This sets up Beads in this repo if needed (Beads makes its own small git commit when it does), then creates one Beads task per plan task, with its dependencies and its "Done when" check.
> 1. Yes, load them (recommended)
> 2. No, keep PLAN.md as the task list

3. On yes, run from the project folder:
   - `bd init --skip-hooks --skip-agents --non-interactive --init-if-missing`
   - `bd --readonly list --all --has-metadata-key plan_id --json` to find tasks loaded on an earlier run. Skip any plan id already there and reuse its Beads id.
4. Create tasks in PLAN order, so every task it needs already has a Beads id. For each task:

```sh
bd create "T3: <title>" --type task \
  --description "Owns: <files>. Parallel: <yes, with T4 | no>. Source: PLAN.md" \
  --acceptance "<Done when text>" \
  --external-ref "PLAN.md#T3" --metadata '{"plan_id":"T3"}' \
  --deps <beads-id-of-T1>,<beads-id-of-T2> --silent
```

   Run one plain `bd create` command per task. Do not wrap them in a script, a loop, or a heredoc: each command should be readable in the permission prompt.
   `--silent` prints only the new Beads id. Write down each `T` id and its Beads id. Leave out `--deps` when "Needs" is none.
5. Check, without changing anything:
   - `bd --readonly list --all --flat --has-metadata-key plan_id` shows every plan task once.
   - `bd --readonly show <id> --json` for one task with needs shows its acceptance text and dependencies.
   - `bd --readonly ready --limit 20` lists only tasks whose needs are done (at the start, the tasks with "Needs: none").
   If a count or dependency is wrong, report it. Do not delete tasks to fix it without asking.
6. Always run `git status --short` now. If it shows changed files under `.beads/`, commit them: `git add .beads && git commit -m "chore: load plan into Beads"`.

## Step 8: Report

Keep it short:
- Files written or changed, and the commits made.
- Task count, and how many are ready to start now.
- Anything skipped and why (for example: no Node.js, no Beads, a file left as it was).
- The user's to-do before building: fill in `web/.env.local`.
- Next step: "Run `agent-team` and say `start`. It reads the ready tasks from Beads." Without Beads: "Build from PLAN.md, one task at a time, in order."
