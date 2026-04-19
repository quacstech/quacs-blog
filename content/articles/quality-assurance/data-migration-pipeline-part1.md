---
title: "Testing a data migration nobody should notice: part 1 — the problem"
date: 2026-04-18
description: "When an API project needs to change its data source without the customer noticing, unit tests are not enough. This is the story of how I built a custom comparison pipeline to close that gap — alone, incrementally, and with a little help from ChatGPT."
tags: ["python", "api", "data migration", "testing", "qa", "ai in practice", "case study"]
draft: false
---

The goal of a successful data migration is invisibility. The customer sends a request, gets a response, and has no idea that the system serving that response changed entirely underneath. No missing fields, no value drift, no subtle differences in how arrays are ordered or how dates are formatted. Identical, for all practical purposes, to what came before.

That is a harder problem than it sounds.

## What the tests already covered

This was an API project with a proper test suite. Unit tests covered the logic layer. Integration tests covered the connections between components. Functional tests covered the behaviour of the endpoints — correct status codes, correct error handling, correct JSON structure. The new system passed all of them.

And yet none of those tests answered the question that actually mattered: does the data coming back from the new source match the data that was coming back from the old one, record by record, field by field, across real production data?

That question requires a different kind of test entirely. Not a test of how the system behaves, but a test of what it returns — at scale, over time, across the full range of real requests that customers actually make.

## Why this gap exists

Functional tests work with controlled inputs and expected outputs. They are excellent at verifying that the system does what it is supposed to do. They are not designed to verify that the data the system retrieves is equivalent across two completely different sources — sources that may have different schemas, different sorting behaviours, different null handling, different array structures.

The gap between "the API works correctly" and "the API returns the same data as before" is exactly the gap that causes migrations to fail silently. Everything passes in test. Production looks fine at first glance. Then a customer reports something wrong, or worse, does not report it and just leaves.

Closing that gap was the task.

## The approach: incremental and file-based

The solution I designed was a Python pipeline — a set of scripts that would call both APIs with the same request, capture both responses, and compare them systematically. Not a one-shot comparison, but an evolving pipeline that grew alongside the development of the new system.

The decision to build custom rather than reach for an off-the-shelf diffing tool was deliberate. The responses were large, nested JSON objects containing multiple sections, some of them arrays. Those arrays were sorted differently on each side. Some fields were intentionally different between the two systems — legacy artefacts in the old source that would not and should not exist in the new one. A generic diff tool would have produced noise. What was needed was a comparison engine with domain-specific rules.

The pipeline was built around a simple but effective architecture: every script produced a result file. Those result files were then processed by aggregation scripts that produced cleaner summary files. Each layer of output was more readable than the last — necessary because the JSON responses were growing with each development milestone, and raw diffs of large JSON objects are not something a human can usefully review at scale.

## The incremental design

The new data source was not built all at once. Development happened in stages — some sections of the response were ready and correct, others were in progress, others had not been started. Testing the whole response from the beginning would have produced results that were mostly noise: failures in parts of the system that were not yet supposed to work.

Instead, the pipeline was designed to grow with the project. Early versions compared only the sections that were ready. As development progressed, new comparison scripts were added to the pipeline. Each version of the pipeline reflected the current state of the migration, testing what could be tested and ignoring what could not yet be evaluated.

This meant the results were always meaningful. Every run of the pipeline produced actionable information — not a list of expected failures mixed in with real ones, but a clean signal about the parts of the system that were supposed to be working.

## The scale

The pipeline ran against real data — approximately 5,000 unique records per day, every day, for several months. Around 150,000 unique records tested over the course of the project. Not enormous by data engineering standards, but well beyond what any manual review process could have handled, and large enough that patterns in the discrepancies — systematic differences rather than one-off anomalies — became visible over time.

Those patterns were the most valuable output of the whole exercise. Individual differences are noise. Systematic differences point at something real: a field being transformed differently, an array being populated from a different source, a date format that changed between systems. The pipeline found several of these. None of them were caught by the existing test suite.

## What comes next

Part 2 covers the technical implementation in detail — the exclusion array pattern, the per-array comparison rules, the result file chain, and how the whole thing was built iteratively with ChatGPT as a coding assistant. The pipeline is also being open-sourced on GitHub, linked from that article, so you can use or adapt it for your own migration projects.

Part 3 is about the ChatGPT angle specifically — what it looks like to build a non-trivial testing tool with an AI assistant, what worked, what needed fixing, and what that workflow actually feels like in practice.
