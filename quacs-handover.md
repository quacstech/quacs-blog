# QuACS — Claude Code Handover Document

## First step — always

Before starting any article or task, fetch and read https://quacs.tech and all article URLs listed under "Articles already live" below. Absorb the tone, structure, length, and how articles end. Every article you write must feel like it belongs on that blog.

---

## What this is

QuACS (quacs.tech) is Mat's personal blog and professional digital identity. It documents real work experience across QA, process improvement, and AI-assisted development. Content is always grounded in lived experience — no filler, no artificial content.

Mat provides bullet points and outlines. Claude drafts from those. Mat owns the content and the voice.

---

## The blog

- **URL:** https://quacs.tech
- **Tech:** Hugo (v0.160.1 extended, Homebrew, Apple Silicon/macOS)
- **Hosting:** Cloudflare Pages
- **Repo:** github.com/quacstech/quacs-blog
- **Editor:** Sublime Text
- **Hugo config:** `buildFuture = true` in hugo.toml; `draft: true` for all new articles

**Categories:**
- quality-assurance
- processes-and-procedures
- ai-in-practice
- thoughts

---

## Writing style — strict

- Confident, dry, economical. No fluff, no hedging.
- Short punchy sentences mixed with longer ones.
- Sections with headers.
- Real case studies with specific details (team sizes, timescales, outcomes).
- First person but not naval-gazing.
- Industry observations delivered with quiet authority.
- Plain English, no jargon inflation.
- **Articles always end on a principle or uncomfortable truth — never a call to action.**
- Content reflects Mat's experience and successes — written in first person where appropriate.

---

## Hugo file format

Use this exact frontmatter structure (based on existing articles):

```markdown
---
title: "Article title here"
date: 2026-04-22
description: "One sentence description."
tags: ["tag one", "tag two", "case study"]
draft: true
---

Article body starts here...
```

---

## TL;DR shortcode

Case study articles get a TL;DR box at the top, after the frontmatter, before the first paragraph. Format:

```
{{< tldr context="One or two lines setting the scene." issue="What was broken or missing." approach="What was done and by whom." outcome="What actually changed." >}}
```

- Keep each field to one or two lines maximum.
- Applied to case study articles only — not "thoughts" category.
- Labels: Context / Issue / Approach / Outcome.

---

## Articles already live (7)

1. Shift-left-left
2. Testing a data migration nobody should notice: part 1
3. Working MVP in 90 minutes — Lovable, Anthropic API
4. We do Scrum. We have dailies.
5. The Scrum adoption that actually worked
6. Postman, Newman, and a little bash
7. The course industrial complex

---

## Articles drafted — need Hugo files (3)

### Fail fast — Nine months. Then nothing.
- Full draft exists in this handover session
- TWO version endings written — Mat to decide:
  - **Version A:** ends on "fail fast" principle
  - **Version B:** ends on "make the invisible visible"
- Once decided, remove the unused ending and create Hugo file

**TL;DR:**
```
{{< tldr context="Large enterprise web application, customer-facing, three development teams, two domains." issue="A feature assumed to be simple spanned three teams, two domains, and nine months of MVP work nobody had budgeted for." approach="Organised and facilitated two full-day workshops with all teams, all product owners, post-its and a wall." outcome="Feature dropped entirely. Nine months of wasted effort avoided in two days of structured conversation." >}}
```

### Codex desktop app — I asked Codex to build me a desktop app. Then I asked for more.
- Full draft exists
- Hugo file created: codex-desktop-app-playwright.md

**TL;DR:**
```
{{< tldr context="Solo experiment, personal productivity problem, no production pressure." issue="Needed a simple local app for daily goal tracking — and wanted to know if Codex could build it." approach="Defined the functionality, built incrementally through Codex prompts. All code written by Codex, including the tests." outcome="Working desktop app with categories, time budgets, persistent state, and 12 automated GUI tests." >}}
```

### assert.that(true)
- Full draft exists
- Hugo file created: assert-that-true.md

**TL;DR:**
```
{{< tldr context="Large complex web application, early Selenium automation era, multiple teams across countries." issue="Test coverage metrics were the goal — not quality, not confidence. The metric." approach="No approach. Junior tester, browsing a repository held up as an example of good practice." outcome="25–30% of tests asserted true and nothing else. Every run green. Nobody asked questions." >}}
```

---

## Articles pipeline — needs full information and writing (10)

When Mat provides bullet points or context, draft the article following the writing style above, then create a Hugo file with `draft: true`.

**1. Context coding and changing tests**
If AI rewrites tests with each iteration, do they function as regression tests at all?

**2. Should tests live outside the app?**
Separate CI/CD project as good practice. Mat's leaning: yes. Needs research before writing.

**3. Data migration part 1**
Content exists on the blog already. Needs Hugo file — Mat to provide a Hugo template example first.

**4. Fail fast**
See drafted articles above.

**5. QA recruitment evolution**
Three eras: 10 years ago (test cases for a chair / biggest possible number of scenarios for a button), today, and the AI era (pick the best agent output). Observational, not a single case study.

**6. Full circle — POM/Selenium**
Mat was using Page Object Model with Selenium WebDriver in 2016. Now it's LinkedIn news. Playwright brought a new wave of testers for whom POM is a discovery. Course creators repackage old patterns. The wheel turns. Tone: quiet amusement, not bitterness.

**7. AI and QA process change**
Not praise or blame. Core argument: Scrum was built for human constraints (context switching, batching, ceremonies) that AI agents remove or reduce. Adaptation path: continuous verification, QA as policy layer, reviewing agent output as the new core skill. Uncomfortable truth: teams keep Scrum because change is hard, not because it still fits.

**8. Test organisation in the AI era**
Unpopular take. No need for traditional test pyramid or regression suite. Instead: generate several test PoCs simultaneously, run in parallel, use an AI agent to analyse results, human reviews and decides. Build light, discard fast. Reframes Scrum's inspect-and-adapt — same principle, compressed from months to days. Human as guardrail, not executor.

**9. Cost of automation**
Real case. Selenium/Java suite run across environments, closer to production = smaller suite. 2-3 hours daily for one person to analyse results, rerun failed tests, triage failures (flaky UI, wrong user permissions, changed UI behaviour, complex business scenarios). "Automate all" industry paradigm. The cost was accepted as normal and never questioned. That is the uncomfortable truth at the end.

**10. AI test automation playbook**
Case study plus guide. How to work with AI to design test automation: define goal, build scalable PoC, grow to MVP, full product across environments, maintenance. Should a separate AI agent review test code on pull requests, guardrailed by a human? Based on the Codex experiment. Proposed PR review agent is untried — present it honestly as a logical next step, not a proven approach. Audience: everyone.

**11. Test architecture**
Granular, specific, separated suites. Each test is an information bearer of its own. Suites separated by usability: can the user do X (simple), moving toward more complex scenarios. Real case observed — no specifics available, describe as a general pattern. Problem: test suites over-engineered into monoliths — hard to manage, create false confidence of coverage and progress. Both a planning failure and a cultural one.

**12. What test coverage actually is**
Not how many tests, but what kind of tests and what value they bring to the product. Confidence of delivering a good app for customers — not a number on a dashboard. Connects to the assert.that(true) article already written.

---

## Key principles to never break

- No filler content, no artificial examples
- Articles end on a principle or uncomfortable truth, never a call to action
- TL;DR boxes on case study articles only
- The data comparison pipeline (github.com/quacstech/data-comparison-pipeline) is zero external dependencies by design
- "Shift-left-left" is Mat's coined concept — quality starting at the cultural/mindset level before SDLC entry
- Test coverage is about customer confidence and product value, not dashboard numbers
- Cost of automation was historically accepted without being questioned — central framing for article 9
