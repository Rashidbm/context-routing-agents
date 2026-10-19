# Architecture and shared contract

This document is the starting interface between parallel workstreams. Change it through a pull request before changing a field used by another component.

## Components

1. **Trace adapter** records an ordered stream of events from one agent run.
2. **Failure and lesson pipeline** groups repeated failures, proposes a draft lesson with supporting run IDs, and stores reviewer approval, edits, or rejection.
3. **Router and preview** evaluate the same trigger against stored traces for a pre-activation preview and against current observable state before the next agent step. Runtime routing matches only approved lessons and logs its decision.
4. **Local dashboard** displays traces, evidence, lessons, preview matches and misses, and evaluation outcomes. It calls the same underlying APIs as a CLI; it does not own business logic.
5. **Evaluation harness** runs the same held-out tasks under three conditions and reports outcomes.

The agent integration is an adapter; the event, lesson, and routing interfaces should not depend on one agent framework. Version 0 only needs one adapter.

## Trace event, version 1

Each event is JSON-serializable. Required fields:

```json
{
  "schema_version": 1,
  "run_id": "run-001",
  "step_id": 3,
  "timestamp": "2026-10-19T10:00:00Z",
  "event_type": "tool_result",
  "data": {"tool_name": "search", "is_empty": true}
}
```

`event_type` is one of `task_start`, `tool_call`, `tool_result`, `agent_step`, or `run_end`. `data` varies by event type. A `run_end` event includes `success` (`true`, `false`, or `null`) and a reason. Null means the outcome has not been judged. Store `model`, `agent_version`, and task ID as run metadata so comparisons remain interpretable.

Record observable actions and outputs. Hidden model reasoning is not required. The adapter must redact secrets and personal data before persisting `data`.

## Lesson, version 1

```json
{
  "schema_version": 1,
  "lesson_id": "empty-search-fallback",
  "version": 1,
  "status": "draft",
  "trigger": {"event_type": "tool_result", "tool_name": "search", "is_empty": true},
  "text": "If search returns no results, change the query or use the fallback source before trying the same call again.",
  "evidence_run_ids": ["run-001"]
}
```

`status` is `draft`, `approved`, or `rejected`. Only `approved` lessons may be routed. The trigger uses observable event fields. Every change to a lesson creates a new version so results can be tied to the exact text and trigger used.

## Router input and output

Input: the latest trace event plus the current run metadata and approved lessons. Output: zero or more matches with `lesson_id`, `version`, `text`, and a reason. A no-match decision is recorded too. Version 0 should use deterministic matching; the agent host decides where to insert returned text before its next model call.

The preview runs the identical matcher over stored trace events, returning matching run/step IDs and non-matches. It estimates reach and possible false matches; it does not predict how agent behavior would change.

## Integration sequence

First agree on a synthetic example run and one held-out task format. Then merge the event schema and a trace-producing agent example. The review pipeline and router can work against the same fixture while the adapter is built. Evaluation consumes a stable end-to-end run after those pieces connect.
