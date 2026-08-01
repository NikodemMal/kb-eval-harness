# CLAUDE.md — how we work in this repository

Read this first. It describes the project conventions and the rules for working with AI.
Private notes, if any exist, live in `CLAUDE.local.md` — outside git.

@CLAUDE.local.md

## What this project is

An eval harness measuring how the **organisation of knowledge** affects factual fidelity,
hallucination rate and honest refusals in LLM-generated content. It is the practical part of
a master's thesis, carried out under a Design Science Research methodology — the artefact is
the object of study, not merely a tool.

The full reasoning behind the axis, the ablation ladder and the delivery plan live **outside
this repository**, in a private knowledge library.

## ⛔ Overriding rule: I write the code myself

**You do not generate Python code.** This is not a stylistic preference — it is the point of
the project.

This work doubles as Python training, because "Python I can defend in an interview without AI"
is a blocking competence for the whole career path. AI-generated code does not serve that goal,
even when it works.

**What you may do:**
- explain concepts, libraries and error messages
- review code I have written and point out problems
- suggest a direction ("a generator would fit better than a list here") without writing the
  implementation
- write and edit **non-`.py`** files: configuration, documentation, YAML data, CI

**What you may not do:**
- write or edit `.py` files — not "quickly", not "as an example", not "because it is trivial"
- hand over ready-made code blocks to copy

When I am stuck, explain the mechanism and point to an analogous example **from the
documentation**, not a finished solution to my problem.

## Architecture — what is fixed

- **One shared interface for every arm** (`src/kbeval/systems/base.py`): an arm receives a task
  and returns an answer plus metadata (tokens, cost). The fairness of the comparison must follow
  from the structure of the code, not from a claim about it.
- **One arm = one file** in `systems/`. Adding an arm must not require changes to the harness.
- **Evaluation does not know which arm it is scoring.** The `eval/` layer operates on answers,
  not on systems.
- **`data/` is data, not code.** Brands, fact ledger, hallucination traps and tasks live in YAML.
- **`results/` is versioned.** Every run stays in history — it is the record of progress.

## Conventions

- **Language: English, everywhere, no exceptions.** Code, comments, file and directory names,
  documentation, YAML data, commit messages, issues and pull requests. The repository is public
  and read outside Poland. The only things that stay in their original form are proper nouns —
  names of people, places and institutions.
- **Commits:** `type: description in the imperative`, with the body explaining WHY. Details below.
- **Tests:** every module in `eval/` has a test. Scoring logic without a test is not credible,
  and this is a thesis about credible measurement.
- **Secrets:** through `.env` only. Never in code, never in YAML, never in prompts.
- **Personal data:** brands are synthetic. Real client data does not enter this repository in
  any form.

## Commit format

```
type: description in the imperative, up to ~50 characters

Body: why this change, not what it changed (the diff shows that).
Wrap at ~72 characters.
```

Types: `feat` (new capability) · `fix` · `docs` · `test` · `refactor` ·
`chore` (configuration, dependencies) · `ci` · `data` (brands, golden dataset) ·
`exp` (experiment runs and results)

One commit = one logical change. Do not mix a refactor with a new feature.

## Working rhythm

**Close something publicly every two weeks**, rather than once at the end: a commit, an update
to the Results/Roadmap section of the README, and a short note on what now works. This is a
deliberate counter to a known weakness in finishing long tasks — closure should happen eight
times in small pieces, not once at a scale I have historically failed to deliver.

## Scope — what is not here

Deliberately out of scope until the end-to-end evaluation chain is closed: a RAG variant on
Azure, the full combinatorics of layers, more than four arms, more than one brand. Do not
propose them; if you believe something on this list has become necessary, say so explicitly
with your reasoning instead of quietly adding it.
