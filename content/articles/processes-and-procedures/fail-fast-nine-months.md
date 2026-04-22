---
title: "Nine months. Then nothing."
date: 2026-02-18
description: "How a two-day workshop saved a year of wasted effort — and why killing a feature is sometimes the best outcome a QA can deliver."
tags: ["processes and procedures", "fail fast", "workshops", "scrum", "case study"]
draft: true
---

<!-- 
FRAMING DECISION NEEDED:
Version A ends with the "fail fast" principle (see bottom of file).
Version B ends with "make the invisible visible" principle.
Choose one before publishing.
-->

A large customer-facing web application needed a rewrite of its user creation flow. The existing implementation handled account setup, card payments, and basic permissions — but it could not scale into modern payment methods or a proper RBAC model. It needed replacing.

On the surface: not a big ask. Add PayPal. Update the permissions model. Tidy up account creation. Everyone who heard the brief nodded along. Easy enough.

That group hallucination cost several weeks before anyone looked at it properly.

## What "easy" actually meant

The feature touched three development teams and two distinct domains — access control and payment processing. Work had started in the usual fragmented way: Jira tickets created, feature descriptions written to varying degrees of completeness, some tasks pulled into some sprints by some teams. Dependencies were assumed rather than mapped. Progress was difficult to track because it was distributed across three backlogs with no shared view of the whole.

Weeks passed. Not much moved. The teams were not slow or disengaged — they were working on something that had not been properly defined, with dependencies that had not been properly understood, across organisational boundaries that had not been properly bridged. The result was motion without progress.

## The workshop

Two sessions. Six hours each. All three teams in the room, all product owners, the PM, me as Scrum Master and QA lead — and a lot of wall space.

The format was deliberately low-tech. Post-its, whiteboard markers, and sustained conversation. No slides. No templates. The goal was to get everything out of people's heads and onto a surface where it could be seen, moved, grouped, and challenged.

Session one: requirements. Everything anyone believed the feature needed to do went on the wall. Then we looked at what was actually there — the overlap, the contradictions, the assumptions stated as facts, the genuine unknowns. The wall was messier than anyone expected.

Session two: dependencies. Which parts of the requirement set belonged to which team, which needed to be sequenced, which were blocked by something in a different domain. Lines were drawn. The picture that emerged was not simple.

From that wall we defined an MVP — the smallest coherent slice of the feature that would actually deliver value. Just that. Not the full vision, not the nice-to-haves. The minimum.

## The number

We estimated the MVP. Then we looked at each team's capacity and pace — actual velocity from recent sprints, not aspirational numbers. We did the arithmetic.

Nine months. Give or take two sprints.

The room went quiet. Nobody had expected that. Management, when they heard it, were unhappy. But the number was honest, it was visible, and it could not be argued away — it was built from real capacity and real requirements, not from optimism.

The full feature — beyond the MVP — added at least seven more months on top.

## The decision

After a few days of further analysis, the feature was dropped. Not paused, not descoped again — dropped entirely. The value it would have delivered did not justify the cost.

That decision frustrated some people. It is never comfortable to end up with nothing after weeks of effort. But the alternative was twelve to sixteen months of three teams partially focused on something that would ultimately be cancelled anyway — just later, with far more sunk cost, and far less clarity about why.

## Version A ending — Fail Fast

Fail fast is easy to say and hard to practise, because failing fast requires knowing quickly — and knowing quickly requires doing the uncomfortable work of mapping what you actually have before you start building it.

The workshops did not produce a plan. They produced clarity. And clarity, in this case, was the finding that the work was not worth doing. That is a valid outcome. It is arguably the best outcome available, given where the project started.

Nine months discovered in two days. That is what fail fast actually looks like.

<!-- 
## Version B ending — Make It Visible

The most valuable thing the workshops produced was not a plan. It was a wall.

The dependencies were always there. The cost was always there. The problem was that nobody could see any of it — until it was on a wall in a room where decisions could be made.

Make the invisible visible, before the sprint does it for you.
-->
