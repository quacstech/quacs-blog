---
title: "I asked Codex to build me a desktop app. Then I asked for more."
date: 2026-04-22
description: "An experiment in AI-assisted development — from a simple checklist idea to a categorised daily tracker with a Playwright test suite, entirely through incremental prompting."
tags: ["ai in practice", "codex", "playwright", "electron", "testing", "case study"]
draft: true
---

Two goals, really. A practical one: a lightweight desktop app for daily goal tracking — categories, tasks within them, time budgets, persistent state. Something local, something simple, something that would not require opening a browser or logging into anything.

And an experimental one: find out whether Codex could build it.

## The PoC

The first version was a checklist. Five pieces of behaviour: add an item, mark it complete, edit it, delete it, display everything clearly. No categories, no time, no persistence. Just proof that Codex could produce a working Electron application from a description.

It could. The first working version appeared quickly. A desktop window, a text field, a list, items that responded correctly to user actions. Unglamorous, functional, and built entirely through prompting — including the fixes. When something broke during manual testing, the debug request went back to Codex rather than being patched by hand. The experiment needed to stay clean.

## The MVP

With the PoC working, the scope grew into something actually useful.

Categories — predefined buckets for organising the day. Each category carried a time estimate: a budget for how long that area of work should take. Items lived inside categories. The state of every item — edited, completed, deleted — persisted across sessions in a JSON file, so closing and reopening the app returned exactly where things were left off.

The interface was adjusted as the functionality grew. Edit mode was restructured. The window was widened. Buttons were reordered into a deliberate sequence. Each change was a prompt. None of the code was written by hand.

The result is an app that does exactly what it was designed to do: set up the day by category, work through it, come back tomorrow and do it again.

## The tests

Tests were added at the MVP stage, not as an afterthought — because the app now had real state, real persistence, and real behaviour worth protecting.

The test suite was built in Playwright. The key decision was isolation: a separate fixture spins up a dedicated app instance with its own test data JSON, completely independent of the real application's storage. Tests run clean, leave nothing behind, and never touch live data.

The suite grew from 5 tests at the start to 12, extending CRUD coverage as functionality expanded. They are well structured, they run reliably, and they cover the behaviour that matters.

## The catch

Here is the honest part. Those 12 tests are not a regression suite — not yet.

A regression suite works by staying still while the code moves. These tests grew alongside the code. When the UI changed, the tests changed with it. At no point did a test fail because something broke unexpectedly — because at no point were the tests independent of the development that was happening.

That is not a criticism of the tests. They accurately describe how the app works at every stage. But accurate description and independent verification are different things. The suite has not yet had the chance to prove its value as a safety net — because nothing has changed without the tests knowing about it in advance.

Whether that matters depends on where the app goes next. If the functionality stabilises and future changes happen without Codex rewriting the tests alongside them, the suite will start doing genuine regression work. Until then, it is a well-written validation layer.

## The verdict

Codex built a useful application. Not a demo, not a toy — something with categories, time budgets, persistent state, and a test suite. The code is clean. The architecture is sensible. The tests are properly isolated.

For a tool built entirely through incremental prompting, with no manual code changes, that is a strong result.

It will be used again. The question it left behind — whether AI-assisted testing produces real regression coverage or just well-written snapshots of the current moment — gets its own article.
