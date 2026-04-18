---
title: "Why test coverage metrics lie to you"
date: 2025-03-12
description: "A case study from a legacy migration project where 94% coverage masked a critical blind spot in business logic."
pinned: false
draft: true
tags: ["test coverage", "qa", "case study", "metrics"]
---

We had 94% test coverage. The migration still broke production.

This is the story of how we learned that coverage metrics measure what you tested, not what matters.

## The project

A legacy billing system migration. Six months of work, a full test suite rewritten from scratch, and a coverage report that made everyone feel confident. The go-live was scheduled, the stakeholders were happy.

## What went wrong

The tests covered every function. What they did not cover was the interaction between a rounding rule introduced in 2019 and a currency conversion edge case that only appeared for a specific subset of enterprise accounts.

94% coverage. 0% coverage of the thing that actually broke.

## What we changed

After the incident we stopped reporting coverage as a health metric. Instead we started mapping tests to business rules explicitly — every critical business rule had a named test, and that test was reviewed by someone who understood the rule, not just the code.

Coverage still runs. We just stopped trusting it as a proxy for quality.