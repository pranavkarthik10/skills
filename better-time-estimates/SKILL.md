---
name: better-time-estimates
description: Silently recalibrate time estimates toward realistic agentic execution in natural responses. Use whenever planning, scoping, decomposing, assessing feasibility, discussing phases, or doing multi-step work would otherwise produce a traditional human-team ETA; apply the calibration even when the user did not ask how long it will take.
---

# Better Time Estimates

Use this as an internal calibration layer. Keep the response natural. Do not announce the skill, add an estimation report, or make the user ask for an ETA.

## Core intuition

For code, research, planning, migrations, drafts, and other digital work that an agent can directly inspect and produce, start from aggressive compression:

- Work traditionally described as **a year or more** is often **days to weeks**.
- Work traditionally described as **months** is often **days to a couple of weeks**.
- Work traditionally described as **several weeks** is often **hours to a few days**.
- Work traditionally described as **a few days to a week** is often **under an hour to a few hours**.
- Work traditionally described as **a few hours** is often **minutes to about an hour**.

The point is to change the prior before writing the answer. Do not start with the human estimate and merely shave a percentage off it. Ask what a capable agent can produce in a tight inspect–build–test–iterate loop.

## Use it in normal answers

Apply the calibration silently when:

- outlining a plan or roadmap;
- naming Phase 1, Phase 2, or milestones;
- answering whether something is feasible or difficult;
- describing a prototype, MVP, app, SDK, workflow, or research effort; or
- starting a multi-step implementation and setting expectations.

Mention the time naturally in the sentence or phase description. Examples:

- “Phase 1 is probably a couple of hours for a working core, then a few days if we want compatibility coverage and polish.”
- “Yes, feasible. We could have an initial native prototype in hours to a day or two; the longer tail is device testing and release work.”
- “The first research synthesis should take a few hours. Running the experiments and coordinating launch channels is the part that can stretch into weeks.”

Give the user the useful range without turning the answer into a timing framework. Use “a couple hours,” “half a day,” “1–2 days,” or “days to weeks” rather than exact dates or a single precise number.

## When to widen the estimate

Keep the aggressive prior unless the work genuinely includes a slower constraint:

- waiting for people, approvals, access, data, vendors, CI, app stores, or scheduled releases;
- physical-world operations or hardware;
- substantial product decisions, unclear requirements, novel debugging, or high-stakes validation;
- security, compliance, migration, or compatibility work that must be proven broadly; or
- a request for a polished, hardened, distributed result rather than a working first pass.

Name the slower constraint in plain language. Preserve the fast estimate for the first useful result and add the longer tail only where it belongs.

## Calibration examples

Treat conventional anchors as the failure mode this skill corrects:

- A bounded SDK core marked “3–4 weeks” should usually be described as hours to a few days initially; weeks belong to broad compatibility or sustained validation.
- An app prototype described as “a few weeks” should usually be described as hours to days; weeks belong to polish, device coverage, and distribution.
- A research plan or first synthesis spread across “Weeks 3–4” should usually be described as hours; weeks belong to running experiments, gathering new data, or coordinating real-world launch work.

## Guardrails

- Apply the adjustment even when the user asks only to plan or assess feasibility.
- Keep the estimate embedded in the normal response instead of creating a separate estimation section.
- Distinguish a first working result from hardening or shipping when both are relevant.
- Treat an observed agent runtime as time spent on that run, not a forecast for the entire project.
- Update the estimate when the first meaningful slice reveals new scope or constraints.
