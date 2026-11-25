---
title: "One rulebook for every AI agent: setting up a Playwright repo for Claude Code and Codex"
date: 2026-11-25
description: "A setup prompt, a manual step-by-step guide and a daily usage guide for making every AI agent and team member write Playwright tests the same way."
tags: ["ai in practice", "playwright", "claude code", "codex", "test automation", "agents"]
draft: false
---

## Part 1: Setup prompt

A prompt for Claude Code. Paste it into a fresh session at the repo root.

```text
I want you to restructure this Playwright UI test repository so that any AI agent (Claude Code or Codex) and any team member produces tests that follow our architecture in the same way. Today the repo has CLAUDE.md with architecture, design patterns and practices, and AGENTS.md whose first line says to read CLAUDE.md. Agents have ignored those rules. Guiding principle: instructions say it, examples show it, checks enforce it, roles and skills make everyone do it the same way. Anything a machine can check must become a check, not a sentence.
How to work with me: build in the stages listed at the end, one stage at a time. After each stage show me the files you created or changed as a diff, and wait for my OK before the next one. When a decision is mine, ask; do not guess. Do not delete or rewrite an existing rule without showing me where it went. Do not change application code or existing tests unless a stage says so.
Target layout:
AGENTS.md: the single source of truth, under ~150 lines, read natively by Codex and imported by Claude Code.
CLAUDE.md: only "@AGENTS.md" plus Claude-specific notes.
CLAUDE.local.md: personal notes, listed in .gitignore.
docs/testing/: architecture.md, patterns.md, examples.md and scenario-template.md. Long material, linked by path, never always-loaded.
.agents/skills/<skill>/SKILL.md: the procedures, shared by both tools. .claude/skills/ holds symlinks to them.
.claude/agents/: the four Claude Code subagents. .claude/commands/: thin entry commands.
.claude/settings.json: hooks and permissions. .claude/hooks/: hook scripts.
ESLint config, strict tsconfig for test code, dependency-cruiser config.
evals/<agent>/, reports/scenarios/, reports/flaky/ (ask me whether reports are committed or gitignored).
AGENTS.md content: one paragraph on what the repo is; then numbered rules R-01, R-02 and so on, each short, concrete and testable (for example "R-03 Specs never call page.locator(); selectors live in page objects under src/pages/"), each tagged [lint], [arch], [types] or [review] to say what enforces it; then the commands (install, run one test, lint, typecheck, verify); the golden example files by path; the four roles and when to use each skill; and the line "For new or changed tests, always use the author-test skill." Rules that cannot be checked by a machine are tagged [review] and the reviewer checks them.
Roles (one job each, minimal tools, fixed output format, a version comment at the top of each file):
analyzer: reads the requirement or ticket text I give, the relevant app area, existing specs and page objects. Proposes scenarios in the template format, numbered S-01, S-02..., each with priority P1-P4, layer (UI, API, or not worth automating), the closest existing spec, and COVERED, PARTIAL or NONE with file and line evidence. Read-only; writes only reports/scenarios/<date>-<subject>.md. Never writes tests.
test-author: a senior automation specialist. Works only on scenario ids I approved and never invents new ones. Procedure: explore the closest spec, page objects and fixtures; plan which page object methods are reused or added; write following the golden examples; run "npm run verify" and the new tests until green; self-review against every R-xx rule. May edit tests, page objects and fixtures only, never AGENTS.md, configs, hooks, skills or agent files. Never adds waitForTimeout, retries or skips to make a test pass.
test-reviewer: fresh context, read-only plus running checks. Reviews the diff against every R-xx rule and the approved scenarios. Output: verdict APPROVE or CHANGES_REQUIRED, then numbered findings with file:line, rule id, why it matters and a suggested fix. Never edits code.
flaky-test-investigator: given a test name or CI run, reproduces it with repeated runs and traces, classifies the cause (timing or race, test data, environment, selector, real app bug, unknown) with evidence from traces, logs and code, and proposes a fix. Writes only reports/flaky/<date>-<test>.md and its evidence folder. Never applies a fix and never adds retries, waits or skips. Marks the result UNVERIFIED when it could not reproduce the failure.
Skills and agents: each procedure lives once, in .agents/skills (analyze-scenarios, author-test, review-test, investigate-flaky), with a description precise enough to trigger the right skill, so Codex uses the same procedures. Each Claude Code subagent file sets name, description, tools and model, and tells the agent to follow its skill file. Subagents cannot ask me questions or call each other, so the main session drives every hand-off and approval gate.
Commands: "/new-test <ticket or scenario text>" runs analyzer, shows the scenarios, waits for me to approve ids, runs test-author, then test-reviewer, and loops author to reviewer at most twice before handing back to me. "/review" runs test-reviewer on the current branch diff. "/flaky <test name or CI link>" runs flaky-test-investigator.
Hooks (in .claude/settings.json, scripts in .claude/hooks/, fast, failures explained in plain words). Hooks only run checks and give feedback; they never start agents.
PostToolUse on Edit and Write for .ts files: run ESLint on that file and a typecheck; on failure exit 2 with the errors so the agent fixes them.
PreToolUse on Edit and Write: block changes to AGENTS.md, CLAUDE.md, .claude/, .agents/ and the ESLint, tsconfig and dependency-cruiser configs, with a message telling the agent to propose the change to me instead.
Stop: if spec, page object or fixture files changed and "npm run verify" fails, block with its output; if it passes and no review ran in this session, remind the agent to run test-reviewer. Respect stop_hook_active so the hook never loops.
Checks: ESLint with eslint-plugin-playwright plus custom rules derived from the R-xx list (no-restricted-syntax where that is enough); TypeScript strict on test code; dependency-cruiser rules for folder boundaries; one "npm run verify" that runs lint, typecheck and architecture checks. Before turning a rule on, run it and show me how many existing violations it finds; I decide whether to fix now, baseline, or warn only.
CI: a job that runs "npm run verify" and the Playwright suite on every pull request, with Playwright retries enabled and tests that passed only on retry listed separately as flaky in the job summary. Ask me before adding any headless AI step to CI, and use only tools my company has approved.
Evals: evals/<agent>/cases.md with 3-5 cases per agent (input, expected key outcome, how to judge it), a short README on rerunning them after any change to AGENTS.md, a skill or an agent, and results appended to evals/<agent>/results/<date>.md.
Build order, one stage at a time with my approval:
Audit, read-only: read CLAUDE.md, AGENTS.md, configs, specs, page objects and fixtures. List every rule you found, contradictions between files, rules the current code already breaks, a proposed R-xx list and proposed golden examples.
AGENTS.md, the CLAUDE.md import, CLAUDE.local.md and docs/testing/.
Checks and "npm run verify", with violation counts per rule.
The scenario template and the four skills.
The four subagents and the three commands.
Hooks, then prove them: show me one blocked edit and one blocked Stop.
The CI job.
Evals, plus one dry run of /new-test on a small real scenario I give you.
A README section "Working with AI agents in this repo" for my colleague, covering both Claude Code and Codex.
Before you start: confirm you understood, list the decisions you need from me for stages 0 and 1 (model per agent, whether reports are committed, which files are golden examples), then do stage 0 only.
```

## Part 2: Manual setup guide

The same setup as human-readable steps, for doing or checking it by hand.

### Setting up AI agents in the Playwright repo: manual step-by-step guide

This guide describes the same setup as the prompt above, for doing or checking it by hand. Work on a branch, commit after each step, and open one pull request per step or two so your colleague can review.

### Step 0. Audit what you have (about 1 hour)

- Read `CLAUDE.md` and `AGENTS.md` side by side and list every rule in a scratch file.
- Mark duplicates, contradictions and vague rules ("write clean tests").
- Search the code for rules that are already broken, for example raw locators in specs or `waitForTimeout`.
- Pick 1-2 golden examples: one spec and one page object you want every new test to copy.

Result: a list of rules, problems and examples for the next steps.

### Step 1. Make AGENTS.md the single source of truth

Create a new `AGENTS.md` with:

- one paragraph on what the repo is;
- numbered rules R-01, R-02..., one line each, concrete and testable;
- a tag on each rule saying what enforces it: `[lint]`, `[arch]`, `[types]` or `[review]`;
- the commands: install, run one test, lint, typecheck, verify;
- the golden example paths;
- the four roles and when to use each one;
- the line "For new or changed tests, always use the author-test skill."

Then:

- Keep it under ~150 lines.
- Move long architecture and pattern text to `docs/testing/architecture.md` and `docs/testing/patterns.md`, and link them from `AGENTS.md` by path.
- Replace `CLAUDE.md` with a single line: `@AGENTS.md` (add Claude-only notes below it if you have any).
- Add `CLAUDE.local.md` to `.gitignore` for personal notes.

Check: in Claude Code, run `/context` or `/memory` and confirm `AGENTS.md` is loaded through `CLAUDE.md`.

### Step 2. Turn rules into checks

Install the tools:

```bash
npm i -D eslint typescript-eslint eslint-plugin-playwright dependency-cruiser
```

- In the ESLint config, enable the Playwright recommended rules and add rules for your `[lint]` items, for example `no-restricted-syntax` to ban `page.locator()` in `.spec.ts` and to ban `waitForTimeout`.
- Turn on `"strict": true` for the test code in `tsconfig`.
- Add dependency-cruiser rules for your `[arch]` items, for example "specs import only from `pages/` and `fixtures/`".
- Add npm scripts: `lint`, `typecheck` (`tsc --noEmit`), `check:arch`, and `verify` (runs all three).
- Run `npm run verify` and count violations per rule. For each rule decide: fix now, baseline existing code, or warn only for now.

Result: `npm run verify` passes on main and fails on rule-breaking code.

### Step 3. Scenario template and shared skills

- Create `docs/testing/scenario-template.md` with fields: id (S-01), title, priority P1-P4, layer, preconditions and test data, steps, expected results, closest existing spec, coverage status.
- Create four skill folders, each with a `SKILL.md` (front matter: name and a precise description of when to use it, then the procedure):
  - `.agents/skills/analyze-scenarios/`
  - `.agents/skills/author-test/`
  - `.agents/skills/review-test/`
  - `.agents/skills/investigate-flaky/`

Make them visible to Claude Code with symlinks:

```bash
mkdir -p .claude/skills
ln -s ../../.agents/skills/author-test .claude/skills/author-test
```

Repeat for the other three.

Check: Codex lists them under `/skills`; Claude Code lists them when you type `/`.

### Step 4. The four Claude Code subagents

- Create one file per agent in `.claude/agents/`: `analyzer.md`, `test-author.md`, `test-reviewer.md`, `flaky-test-investigator.md`.
- Each file has front matter (name, description, tools, model) and a short body: its one job, its output format, and "Follow `.agents/skills/<skill>/SKILL.md`".
- Give each agent only the tools it needs:
  - analyzer: read and search only;
  - test-author: read, edit, write and Bash;
  - test-reviewer: read, search and Bash for running checks, no edit;
  - flaky-test-investigator: read and Bash, writes only its report.
- Add thin commands in `.claude/commands/`: `new-test.md` (analyzer, wait for approved ids, author, reviewer, at most two loops), `review.md` and `flaky.md`.

Check: `/agents` lists all four, and `/new-test` appears in the command list.

### Step 5. Hooks (Claude Code)

Create three scripts in `.claude/hooks/` and make them executable (`chmod +x`):

- `check-file.sh`: reads the edited file path from the JSON on stdin; for a `.ts` file runs ESLint on it and a typecheck; on failure prints the errors to stderr and exits 2.
- `protect-rules.sh`: if the path is `AGENTS.md`, `CLAUDE.md`, `.claude/`, `.agents/` or a check config, prints "Propose this change to the user instead" to stderr and exits 2.
- `on-stop.sh`: exits 0 at once if `stop_hook_active` is true; otherwise, if specs, page objects or fixtures changed (`git diff`), runs `npm run verify` and exits 2 with the output on failure; if it passes and no review ran, exits 2 with "Run the test-reviewer agent before finishing".

Register them in `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [{ "matcher": "Edit|Write", "hooks": [{ "type": "command", "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/protect-rules.sh" }] }],
    "PostToolUse": [{ "matcher": "Edit|Write", "hooks": [{ "type": "command", "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/check-file.sh" }] }],
    "Stop": [{ "hooks": [{ "type": "command", "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/on-stop.sh" }] }]
  }
}
```

Check: ask Claude to edit `AGENTS.md` (it should be blocked), and to add a `waitForTimeout` to a spec (the lint hook should make it fix the code).

Note: Codex does not use these hooks; for Codex users, `npm run verify` and CI are the gate.

### Step 6. CI

- Add a pull request job that runs `npm ci`, `npm run verify` and the Playwright suite.
- In `playwright.config.ts` set `retries: process.env.CI ? 2 : 0`.
- Add the JSON reporter and a small script that lists tests with status "flaky" in the job summary.
- Make the job a required check on main.
- Add a headless AI step (for example running the flaky investigator automatically) only after confirming it is an approved tool.

### Step 7. Evals

- For each agent create `evals/<agent>/cases.md` with 3-5 real cases: input, expected key outcome, how to judge it. Include at least one case where test-reviewer must catch a rule violation.
- Write a short `evals/README.md` on how to rerun them.
- Rerun the evals after every change to `AGENTS.md`, a skill or an agent, and append results to `evals/<agent>/results/<date>.md`.
- Do one end-to-end dry run of `/new-test` on a small, real scenario.

### Step 8. Hand-over to your colleague

- Add a "Working with AI agents in this repo" section to the README (the usage guide below fits).
- Walk through one `/new-test` together, in both Claude Code and Codex if you use both.
- Agree that rules, skills, agents and hooks change only through pull requests, with the evals rerun.

### Done when

- `npm run verify` fails on rule-breaking code, locally and in CI.
- Both tools load the same `AGENTS.md` and the same four skills.
- In Claude Code, an agent cannot edit the rules and cannot finish with failing checks.
- You and your colleague get similar results on the eval cases.

## Part 3: Daily usage guide

How you and your colleague use the agents once they are set up.

### Working with AI agents in this repo

#### Before you start

- Open Claude Code or Codex at the repo root.
- Claude Code: `/memory` should show `AGENTS.md` loaded through `CLAUDE.md`, and `/agents` should list analyzer, test-author, test-reviewer and flaky-test-investigator.
- Pull main first, so you have the current rules, skills and agents.

#### New test (Claude Code)

1. Paste the ticket text or fill `docs/testing/scenario-template.md`.
2. Run: `/new-test <ticket or scenario text>`
3. Read the analyzer output in `reports/scenarios/` and reply with the ids you approve, for example "S-01, S-03".
4. test-author writes and verifies. Hooks block it until lint, typecheck and `npm run verify` pass.
5. test-reviewer returns APPROVE or CHANGES_REQUIRED. After at most two author-reviewer loops it hands back to you.
6. Read the diff and the findings yourself before you commit. The agents assist; you own the merge.

#### New test (Codex)

1. `$analyze-scenarios <ticket text>`
2. Approve ids, then: `$author-test S-01 S-03`
3. `$review-test`

Codex has no hooks here, so run `npm run verify` yourself; CI is the final gate.

#### Review only

- Claude Code: `/review`
- Codex: `$review-test`

#### Flaky test

- Claude Code: `/flaky <test name or CI link>`
- Codex: `$investigate-flaky <test name or CI link>`

Read `reports/flaky/<date>-<test>.md` and decide the fix yourself. Never accept a fix that only adds waits, retries or skips.

#### Calling one agent directly (Claude Code)

Ask by name, for example "use the test-reviewer agent on my last commit".

#### Changing the rules

- Agents cannot edit `AGENTS.md`, skills, agents, hooks or check configs; the hook blocks it.
- Change them yourself in a pull request, rerun the evals in `evals/`, and tell your colleague what changed.
- Personal preferences go in `CLAUDE.local.md`, never in shared files.

#### When an agent breaks a rule that nothing caught

1. Add or tighten a lint or architecture rule first.
2. Only if no check is possible, add or sharpen an R-xx line tagged `[review]`.
3. Add the case to `evals/` so it stays fixed.
