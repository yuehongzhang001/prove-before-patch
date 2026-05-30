---
name: prove-before-patch
description: Use this skill when a problem feels complex, slippery, or repeatedly patched without sticking. It provides a principles-first debugging method: establish facts before fixes, segment the system into stages, verify one hypothesis at a time, separate symptoms from causes, trace state across boundaries, and stop patch stacking when the problem definition is likely wrong.
---

# Prove Before Patch

Use this skill to debug by building an evidence chain before changing the system. Favor observation over speculation, narrow the problem one boundary at a time, and treat repeated patching as a signal that the current problem definition may be wrong.

## Core Principles

- Treat the reported failure as a symptom, not the root problem.
- Break the flow into stages and identify the earliest stage that is confirmed to fail.
- Define one concrete hypothesis per step and gather evidence that could prove it true or false.
- Prefer direct observations such as logs, requests, state snapshots, outputs, and reproducible probes over explanation by intuition.
- When one stage succeeds but the overall outcome fails, inspect the boundary between stages first.
- If fixes are accumulating without a stable explanation, stop patching and restate the problem from the last confirmed facts.

## Working Loop

1. Restate the observable failure in neutral terms.
2. Segment the relevant flow into a small number of stages.
3. Mark each stage as confirmed, disproved, or unknown based on current evidence.
4. Choose the earliest unknown or contradictory stage.
5. Form one hypothesis about that stage.
6. Add the smallest probe needed to test it.
7. Update the stage map from the result before making further changes.

Do not run multiple speculative fixes in parallel when one targeted observation would eliminate them.

## Stage Mapping

Use simple stage language. Examples:

- input received
- handler executed
- state updated
- boundary crossed
- downstream system accepted the state
- final output rendered

The exact stages depend on the task. Keep them short and causal.

## Symptom Versus Cause

Symptoms often appear late and move around:

- error messages
- loading states
- missing UI changes
- retries
- timeouts

Causes are usually earlier and more stable:

- wrong assumption about where the flow breaks
- state not persisting across a boundary
- mismatched identifiers, timing, formats, or ownership
- success in one layer not being recognized by the next

Do not optimize around the loudest symptom before locating the causal break.

## Boundary Checks

When a local action appears successful but the end-to-end result still fails, inspect the handoff between layers. Compare both sides of the boundary for:

- the same state name or key
- the same identifier or namespace
- the same timing assumptions
- the same transport or serialization format
- the same source of truth

Boundary mismatches are common because each side may be internally correct while still disagreeing with the other.

## Stop Conditions

Pause and reframe when any of these appear:

- multiple patches were added without improving certainty
- the working theory explains some symptoms but not the full flow
- evidence contradicts the current diagnosis
- the same failure reappears in a slightly different form after each fix

When this happens, roll back to the last confirmed fact pattern and redefine the problem from there.

## Output Expectations

When reporting progress or a fix:

- state the last confirmed failing stage
- name the hypothesis that was tested
- summarize the evidence that confirmed or disproved it
- explain the root cause as a break in the causal chain
- describe the fix only after the causal explanation is clear
