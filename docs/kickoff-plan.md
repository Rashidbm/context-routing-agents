# Project kickoff: what we will build and how we will work

This is the asynchronous working plan for a five-person university team. The idea was shared with the team on **11 September 2026**. The [Sentry paper](https://arxiv.org/abs/2610.02994) was published on **2 October 2026**, after that message; we use it now to inform implementation and evaluation, not as the source of the idea. [Mirrors](https://mirrors.dev/) is the product reference for the open-source workflow we want to build. See the companion [literature review](literature-review.md) for details.

## One-sentence outcome

Ship an open-source, self-hostable alternative inspired by Mirrors: a Python SDK, trace and lesson review dashboard, runtime context router, and reproducible evaluation. A developer should be able to connect an agent, inspect a recurring failure, approve a targeted lesson, preview its matches, and observe the effect on later runs.

Success means someone outside the team can clone the repository, run the example, inspect the routing decision, reproduce the comparison, and adapt the package to a second agent without rewriting its core.

## Version 0: exact demonstration

Use a small research agent with two tools: a local document search and a fallback search source. Some tasks produce an empty local-search result. The unassisted agent may repeat the same call instead of changing strategy. Use synthetic documents and tasks so the demo is safe to publish.

The first vertical slice is:

1. Run the agent on a development task and store observable trace events as JSONL.
2. Detect a repeated empty-result retry from those events and create a **draft** lesson with links to supporting run IDs.
3. Show the linked traces and the candidate lesson in a small local dashboard. A human reviewer approves, edits, or rejects it.
4. Preview which stored traces the trigger would match, including unintended matches, before activation.
5. On a new run, match an approved lesson after the relevant tool result and before the next model call. Log the match or no-match reason.
6. Run held-out tasks under three conditions: baseline, lesson always in the prompt, and lesson routed at the matching step. Show task outcomes and overhead in the dashboard.

The initial detector may group traces by a simple failure signature and propose a candidate lesson; the router may use explicit rules. Do not spend the first week building an LLM-based clustering engine or polished UI. More adapters, dynamic tool description/schema/result edits, advanced matching, and hosted deployment are later extensions after the end-to-end path works. This is a staged build toward the broader Mirrors-like workflow, not a claim that the first release covers every feature.

## Technical choices for the first slice

| Concern | Version 0 choice | Why |
| --- | --- | --- |
| Language | Python | Fits the agent and research tooling; keeps one implementation language. |
| Agent integration | One small adapter for the example agent | Proves the API without claiming universal framework support on day one. |
| Trace storage | Local JSONL, schema versioned | Easy to inspect, diff, and reuse in tests. |
| Lesson storage | Local JSON with explicit draft/approved/rejected status and version | Makes review and rollback visible. |
| Routing | Deterministic predicate over observable step state | Match and no-match decisions are testable and explainable. |
| Review UI | Minimal local Streamlit dashboard; CLI for automation | Keeps the first build in Python while showing evidence, approvals, previews, and outcomes. |
| Evaluation | Scripted tasks with development and held-out splits | Lets others reproduce both successes and failures. |

The shared event and lesson shapes live in [architecture.md](architecture.md). A proposed change to these shapes needs a PR that updates the examples before dependent code changes. Hidden model reasoning is not required or collected.

### Developer journey and first dashboard views

The SDK records a run through a small, framework-neutral event API. The example agent calls a `before_model_call` hook after each tool result; that hook asks the router for applicable approved guidance and logs the decision. The first adapter is for our example agent. A future adapter can translate another framework's callbacks into the same event records.

The local dashboard needs four plain views, each backed by the same stored data and APIs used by the CLI:

| View | Minimum content |
| --- | --- |
| Runs | Run list with outcome and event timeline; inspect tool inputs/outputs after redaction. |
| Patterns & lessons | Failure group, count, linked evidence runs, draft text and trigger, approve/edit/reject actions. |
| Preview | For one draft lesson, show matching run/step IDs plus examples that should not match. |
| Results | Side-by-side baseline, always-on, and routed aggregate counts and cost/latency. |

The package should remain usable without the dashboard. Its public core should expose event recording, lesson storage/review, preview, and runtime routing through Python functions or methods. Keep UI code out of those core modules.

## Work ownership

One person owns each row. Ownership means making the first PR, keeping the issue current, coordinating interface changes, and writing the corresponding part of the final report. Everyone still reviews other parts.

| Owner | GitHub issue | First reviewable deliverable | Later deliverable | Report section |
| --- | --- | --- | --- | --- |
| Team lead | [#1](../../issues/1) | Confirm example agent, success check, and two synthetic traces (one match, one no-match) | Integrate components, build the minimal dashboard views, maintain README/demo, edit final report | Problem, scope, architecture, prior work synthesis |
| Trace/SDK owner | [#2](../../issues/2) | Event types and a JSONL trace from the example | Thin instrumentation API, redaction, adapter tests | Instrumentation and data model |
| Failure/lesson owner | [#3](../../issues/3) | Detector for repeated empty-result retry, draft lesson fixture | Lesson storage and approve/edit/reject API, version history, evidence supplied to the dashboard | Failure pattern and lesson lifecycle |
| Router owner | [#4](../../issues/4) | Match/no-match function against fixtures | Historical match preview, runtime insertion, decision logs, routing tests | Routing mechanism and edge cases |
| Evaluation owner | [#5](../../issues/5) | Task list, held-out split, and success checks frozen before tuning | Three-condition runner, metrics, plots/table, reproduction commands | Experimental setup and results |

If an owner is blocked by another component, use the synthetic fixtures in `docs/architecture.md` and comment on the blocking issue. The lead coordinates a small integration PR as soon as the trace and router can connect; do not wait for all four parts to be feature-complete.

## Research-paper reading assignment

Everyone reads the abstract and Sections 2–4 of [Sentry](https://arxiv.org/html/2610.02994). Each owner posts **three short points** in their issue: (1) a design idea we adopt, (2) one thing we simplify or do differently, and (3) what their component will test. The evaluation owner also reads Section 5.3 and the held-out protocol. The lead keeps a summary of prior work in the [literature review](literature-review.md).

Sentry uses failure diagnosis, retrieval, verification, and an evolving playbook. Mirrors is the closest product reference for our trace-to-reviewed-lesson-to-runtime-routing workflow. Our first release focuses on a human-approved lesson, match preview, inspectable routing, and a local dashboard. We may explore automated learning and broader context edits after the base system and evaluation are credible. The paper's reported gains are its results; our report must contain our own measured results.

## Working without a meeting

1. Each teammate sends the lead a GitHub username and first and second workstream preference. The lead assigns exactly one owner per workstream and invites them to the repository.
2. Before coding, the owner comments on the issue with their first small deliverable, expected input/output, and any dependency.
3. Use the issue for decisions and blockers. Post `Done / Next / Blocked` when the state changes; a daily update is enough while work is active.
4. Work on a branch named for the issue, such as `sdk/trace-events`. Open a draft PR with the first reviewable slice, link the issue, and request one teammate's review. `main` requires one approval.
5. A PR is ready to merge when its behavior is demonstrated, its relevant tests pass, its interface/docs are updated, and a reviewer can explain what it changes. The lead checks the full example after each merge.
6. Hold a brief asynchronous checkpoint at the end of each week: a runnable command or screenshot, what failed, the next smallest step, and any scope change. Record it in [issue #1](../../issues/1).

Suggested cadence: first week contracts, fixtures, baseline agent, and first PR from every owner; second week lesson review, dashboard skeleton, and runtime integration; third week preview and held-out comparison; final week documentation, packaging, and demo. Adjust the pace to the course deadline and each member's availability, but preserve the order of dependencies.

### First 48 hours, without a call

- Lead: post the kickoff message and polls, collect handles, assign issues, and add the synthetic matching/non-matching runs to issue #1.
- Every owner: read the README, architecture contract, and assigned Sentry sections; comment on the issue with a first PR-sized deliverable and one question or risk.
- SDK and router owners: agree on the exact event passed to `before_model_call` in issue #1 before parallel implementation.
- Lesson and evaluation owners: draft a failure group and held-out task examples using the same synthetic trace fixture.
- Lead: resolve interface questions in writing within the issue; merge the smallest contract/fixture PR first.

## Evaluation and claims

Follow [evaluation.md](evaluation.md). The development set may be used to write and tune the lesson. The held-out set must remain unseen until the lesson and trigger are frozen. Run the same model, tools, task set, budgets, and success checks for all three conditions. Report raw counts, not just a percentage, and include negative cases where a lesson would be irrelevant. Historical trace preview can measure trigger coverage and false matches; only new runs can measure changed task outcomes.

## What makes the repository useful to others

Aim for a quickstart that works with one command, a small API, a clear example, transparent routing logs, synthetic fixtures, reproducible evaluation, and a contributor-friendly issue list. Those are concrete reasons for developers to try and share an open-source project. GitHub stars are a possible outcome of utility and visibility, not a technical success metric or promise. Publish honest limitations and results, including cases where routing does not help.
