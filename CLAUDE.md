# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

Personal GitHub profile and CV repo for Cosmin Poieana (cmin764). No build system, no tests, no dependencies. The repo is pure Markdown content with one CI workflow.

## Content Hierarchy and Workflow

There is a deliberate flow between the four files. Changes generally propagate from left to right:

```
codex.md  →  cv.md  →  README.md
travel.md  (independent)
```

1. **`codex.md`** is the source of truth. Raw career history, engagement details, principles, philosophy, and life stories relevant to professional identity. Everything goes in here first, formatted and synthesized with AI assistance.

2. **`cv.md`** is a curated distillation of `codex.md`. Keep it focused: relevant experience, credentials, stack. When new engagements, skills, or achievements land in `codex.md`, assess what belongs in the CV and update accordingly.

3. **`README.md`** is the public-facing surface. Rendered at `github.com/cmin764`. Extract only the essential from `cv.md` — current role, stack highlights, notable links. It is a teaser, not a biography.

4. **`travel.md`** is an independent ledger of countries and date ranges visited. Update it when travel changes. The country count at the bottom should stay in sync with the list.

## CV PDF Build

Defined in `.github/workflows/cv.yml`. Two jobs: `build` then `deploy`.

**Triggers:** push to `main` touching `cv.md`, or manual `workflow_dispatch`.

**Build job:**
1. Checks out the repo.
2. Injects today's date into `cv.md` via `sed` (replaces the `Updated on DD Mon YYYY` pattern). This happens only inside the CI runner — it does not commit back to the repo. The local file is never modified.
3. Runs `pandoc cv.md -o cv.pdf --pdf-engine=xelatex` via the `pandoc/extra:latest` Docker image.
4. Uploads `cv.pdf` as a build artifact.
5. Uploads the **entire repo directory** (`.`) as the GitHub Pages artifact — so all files (`cv.md`, `cv.pdf`, `travel.md`, etc.) are served from the Pages site.

**Deploy job:** deploys the Pages artifact. The CV PDF is then publicly accessible at:
`https://cmin764.github.io/cmin764/cv.pdf`

To trigger a rebuild without editing CV content, use `workflow_dispatch` from the Actions tab or make a whitespace change to `cv.md`.

## Conventions

- No trailing AI signatures in commit messages.
- When updating `cv.md`, check whether `README.md` needs a corresponding surface-level change (current role, stack, links).
- `codex.md` can hold sensitive or unpolished context — it is not rendered anywhere prominent, but it is public. Keep personal contact details and anything not meant for public indexing out of it.
