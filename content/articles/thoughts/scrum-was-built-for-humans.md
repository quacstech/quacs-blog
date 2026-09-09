---
title: "Scrum was built for humans. AI agents aren't."
date: 2026-09-09
description: "Scrum solves problems that come from being human — context switching, batching, the cost of a meeting. AI agents don't have those problems. An observation on what that means for QA, not a verdict on Scrum."
tags: ["ai", "scrum", "process", "qa", "thoughts"]
draft: true
---

This is not an argument that Scrum is broken, or that agile is dead, or any of the other headlines that get written every time a new tool shows up. It is an observation about what a framework is actually for, and what happens when the thing it was built around stops being true.

## What Scrum was actually solving

Scrum did not appear out of nowhere. It was a response to real constraints — human ones.

People context switch badly. So work gets batched into sprints, so nobody is asked to reprioritise every hour. Communication between people is expensive and lossy. So there are ceremonies — standups, plannings, retros — because getting a room of humans aligned takes deliberate, repeated effort. Estimation is hard because humans are inconsistent day to day, so points and velocity exist to average that inconsistency out over time. Feedback loops are slow because writing code, testing it, and understanding the result takes a person hours or days. So the loop gets fixed at two weeks, because that is a reasonable cadence for a human team to plan around.

None of this was arbitrary. Every ceremony in Scrum is a patch for a specific human limitation. That is exactly why it worked as well as it did — it was designed for the constraint that was actually there.

## The constraint that is no longer there

An AI agent does not context switch the way a person does. It does not need a two-week batch to stay focused, because it does not lose focus. It does not need a standup to know what the rest of the team did yesterday — it can read every commit. It does not get tired, does not have a bad estimation day, does not need a retro to remember what went wrong last sprint, because "last sprint" is not how it experiences time at all.

Feed it a well-defined task and it can produce working code in minutes, not days. That collapses the very thing sprint cadence was built to manage — the gap between deciding what to build and having something to look at.

Point at the framework and ask which ceremony solves a problem the agent has, and the honest answer is: none of them. They solve problems a human has. The agent doesn't have those problems. It has different ones.

## What actually needs to change

This is not a case for abandoning process. It is a case for noticing that the process needs to move to where the real bottleneck now sits.

The bottleneck used to be generating the work. Now, increasingly, it is verifying it. An agent can produce a plausible pull request faster than a human can read it properly. So the skill that matters is shifting — from writing code to reviewing what was written, at a pace and a rigour that most review processes were never built to sustain.

That points toward continuous verification rather than sprint-boundary verification. Not "we'll test it before the sprint review," but a constant, tight loop of generate, check, correct, running far faster than two weeks and far faster than a human alone can sustain unassisted.

It also points toward QA moving from a stage in the pipeline to a policy layer sitting over the whole thing — the standards an agent's output has to meet before it is trusted, checked continuously rather than at a gate. Not "did QA sign off this sprint," but "does everything produced, by anyone or anything, meet the bar, all the time."

And it points toward a new core skill: reading and judging agent output quickly and correctly. Not writing the test. Reviewing whether the agent's test is actually testing the right thing, whether its confidence is earned, whether what looks plausible is actually correct. That is a different discipline from either traditional manual testing or traditional code review, and most teams do not have anyone who has deliberately practised it yet.

## The uncomfortable part

None of this is controversial once you say it out loud. Most people building with AI agents daily already feel the mismatch — the standup that reports on work an agent finished before the meeting started, the sprint boundary that exists only because the calendar says it should.

So why does the ceremony survive unchanged on so many teams? Not because it still fits. Because changing a working process is organisationally expensive, and "it's fine, we're used to it" is a comfortable place to stay. Scrum persists on a lot of teams now not because it solves the problem in front of them, but because replacing it means admitting the old answer no longer applies, and building a new one from scratch.

That is a harder thing to say in a retro than "we're keeping the ceremonies because they work." But it is the truer one.
