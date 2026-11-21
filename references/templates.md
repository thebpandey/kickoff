# Templates

Fill each template from IDEA.md and GLOSSARY.md. Use the glossary's words. Replace every `<...>`. Delete any line that does not apply; never leave a placeholder.

## SPEC.md

```md
# <App name>: Spec

<One sentence: what the app is and who it is for.>

## User
<Each kind of person who uses the app, one line each. Say who can log in.>

## Core job
<The one thing the app must do well, in one or two sentences.>

## Screens
| Screen | Who sees it | What they do there |
|---|---|---|
| <name> | <user kind, or "anyone"> | <one line> |

## Data
| Thing stored | Fields (plain words) | Private? Who can see and change it |
|---|---|---|
| <name> | <fields> | <for example: "only the owner"> |

## Must never happen
- One user sees or changes another user's private data.
- A secret (password, API key) appears in the app's code, the browser, or git.
- <app-specific rule, for example: two clients book the same time slot>

## Out of scope (not in this version)
- <item>
```

## PLAN.md

```md
# <App name>: Plan

## Stack
<Stack in one line> (<one-line reason>). App code lives in `web/`.

## Tasks
Run tasks in order unless marked parallel. A task starts only when everything in "Needs" is done.

### T1: <short title>
- Owns: <files or folders this task may change>
- Needs: none
- Parallel: no
- Done when: <a check a reviewer can run or see, for example: "`npm run build` in web/ exits with no errors" or "signed in as the owner, /bookings lists the 3 test bookings">

### T2: <short title>
- Owns: <...>
- Needs: T1
- Parallel: yes, with T3
- Done when: <...>
```

## CLAUDE.md (project)

```md
# <App name>

<One sentence from SPEC.md.>

- Spec: SPEC.md. Plan: PLAN.md. Words we use: GLOSSARY.md. Original decisions: IDEA.md.
- Task tracker: Beads (`bd ready` shows what can start). If Beads is not set up, PLAN.md is the task list.
- Stack: <stack>. App code is in `web/`.
- Run: `cd web && npm run dev`, then open http://localhost:3000.
- Check: `cd web && npm run lint && npm run build`.
- Secrets go in `web/.env.local`, which the owner fills in. Never read, print, or commit any `.env` file.

## Must never happen
<copy the list from SPEC.md>

## How to build
- Do the smallest thing that meets the task's "Done when" check. No extra features, screens, or libraries.
- Only change the files the task owns. Anything not in SPEC.md is out of scope: ask first.
- Every table that holds private data has row level security on (a database rule that limits each row to the people allowed to see it).
```
