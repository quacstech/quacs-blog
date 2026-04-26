---
title: "Working MVP in 90 minutes — Lovable, Anthropic API, and knowing what you want to build"
date: 2026-04-10
description: "How I went from idea to working MVP in a single evening — and why the 90 minutes was the easy part."
tags: ["ai in practice", "lovable", "claude", "mvp", "case study"]
draft: false
---

{{< tldr context="Solo evening build, Lovable and the Anthropic API, one clear idea." issue="Most 'built with AI' stories skip the part that actually made it work." approach="Defined the full sequence, inputs, outputs, and architecture before writing a single prompt." outcome="Working MVP in 90 minutes — CV analysis, scoring, tailored output, downloadable PDFs." >}}

There is a version of this story that is about speed. Ninety minutes, working app, Claude API, impressive. That version is true but it is not the interesting part.

The interesting part is what happened before I opened Lovable.

## What the app does

The app analyses CVs against job listings. You paste in a PDF of your CV, paste a link to a job listing, and it does four things: extracts the requirements, company name, salary range and URL from the listing; analyses how well your CV matches; scores the match from 0 to 100; and asks you on a scale of 1 to 10 how much you want to tailor your CV for this role.

Based on that input it produces two PDFs — a tailored CV and a job listing summary to store alongside it for when the interview call comes.

One thing worth saying clearly: this tool does not invent experience. It reads what is already in your CV and brings the relevant parts forward. The goal is honest self-presentation, not fabrication. If the experience is not there, the score reflects that.

## The planning

Before writing a single line — before opening Lovable, before typing a prompt — I defined what the app needed to do and how it needed to be structured.

Not in a document. Not in a diagram. Just clearly, in order:

Read a PDF. Extract text from a job listing URL. Make sure both inputs are actually parseable before doing anything with them. Run the analysis. Produce a score. Take the tailoring preference as an input. Rewrite the CV based on real content, not invented content. Make the outputs downloadable.

That is it. Simple sequence, clear inputs and outputs at each step, nothing clever. But the clarity of that sequence is what made the 90 minutes possible. I was not figuring out what to build while building it. I was building something I had already figured out.

The second decision was scalability. The PoC needed to be structured in a way that the MVP could follow without rewriting everything, and the MVP needed to be structured so the final version could add features without breaking what was already working. Each stage had a defined scope. The PoC proved the core — can I read a CV, read a listing, produce a comparison. The MVP proved the product — can a real user do this end to end and get something useful out. The final version is about polish, edge cases, and the things you only discover when something actually works.

## The 90 minutes

With that clarity going in, the build was fast because every decision had already been made. When Lovable asked what I wanted, I could describe it precisely. When Claude needed prompting, I knew what I was asking for. When something did not work, I knew which part of the sequence had failed and why.

The MVP at the end of those 90 minutes had everything working: PDF ingestion, listing extraction, matching analysis, scoring, tailoring preference, and downloadable output. The tailored CV was not styled correctly yet — that came later — but it was there, it was accurate, and it was built from the actual content of the CV, not from thin air.

That last part matters. The Claude integration is doing real analytical work — reading a document, understanding a job description, identifying what is relevant and what is not, rewriting with that in mind. The prompt engineering behind that is not trivial. Getting it right required knowing what good output looked like before asking for it, which is another form of planning.

## What this is actually about

The speed was a consequence of preparation. The tool worked because the requirements were clear before the first prompt was written. The architecture held because scalability was a constraint from the start, not an afterthought.

This is the part that gets lost in most "I built X with AI in Y minutes" stories. The implication is that the AI did the work and the time is the measure of how powerful the tool is. That is not what happened here. The AI executed. The thinking — what to build, in what order, with what constraints, toward what outcome — was done beforehand, by a person, without any tool at all.

Lovable and Claude are genuinely impressive. They collapsed what would have been days of boilerplate into an evening. But they collapsed it for someone who knew what they were building. Pointed at ambiguity, they would have produced something faster — and much less useful.

The app is still a work in progress. The final version will be linked here when it is ready. The point of writing about it now is not the finished product — it is the process that got it this far this quickly, and what that process actually required.
