---
title: "assert.that(true)"
date: 2026-03-15
description: "What a repository full of green tests taught me about the difference between measuring quality and performing it."
tags: ["quality assurance", "test coverage", "metrics", "selenium", "case study"]
draft: true
---

This was a long time ago. Gmail was still in beta. Selenium was the tool everyone was learning, and automated test coverage was the thing every management team wanted to show on a dashboard.

I was a junior tester on a complex web application. The project had manual testing as its foundation and was building out Selenium automation alongside it. Scenarios were written, reviewed, and approved. Test cases went through sign-off. Code was reviewed. Everything followed the right process.

The goal, stated clearly, was coverage. High numbers, green reports, dashboards that told a good story. Capacity was never quite enough — it rarely is — and the team was always behind schedule. Managers were not happy about that.

## The other team

At some point we were given access to the repository of another team, based in a different country. Their coverage reports were exceptional. High numbers, near-perfect success rates, everything green. They were held up as an example. We were pointed at their codebase to see how they implemented their tests and whether anything could be reused.

We browsed the repository.

Somewhere between 25 and 30 percent of their tests looked like this:

```java
@Test
public void shouldLoginToRegistryWithCorrectUser() {
    assert.that(true);
}
```

Every test passed. Of course it did.

## What this is about

Nobody was stupid. The people who wrote those tests understood exactly what they were doing. They were responding rationally to what was being measured. The metric required green. They made it green. The dashboard was happy. The managers were happy. The process had been followed.

This is what happens when the metric becomes the goal. Coverage stops being a signal about quality and becomes a performance of quality. The tests exist. They pass. The report looks right. And somewhere underneath all of that, the actual behaviour of the application is nobody's problem.

I was reassigned to a different project not long after — API testing, which is its own education. I never found out what happened to those tests, or whether anyone with authority ever saw what we saw. Maybe they did. Maybe the numbers stayed on the dashboard either way.

## The principle

Coverage metrics are not useless. They tell you what has been executed. They do not tell you whether the execution meant anything.

A test that asserts true is not a test. It is a number. And a team optimising for numbers will always find a way to produce them — especially when capacity is tight, deadlines are real, and nobody is looking closely at what the green actually represents.

Measure coverage. But know what you are measuring.
