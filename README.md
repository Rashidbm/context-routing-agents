# Context Routing for AI Agents

An open-source student project exploring how to give an agent a relevant lesson **only when a matching situation occurs**. The initial goal is a small, reproducible end-to-end system and an honest evaluation, not a production platform.

## The first demonstration

A tool-using agent can repeat the same call after an empty tool result. We will collect execution events, identify this recurring failure, review a proposed lesson, and route that lesson when a similar situation occurs in a new task.

```text
agent run -> trace events -> failure review -> approved lesson
                                            |
new agent step -> condition match ----------+-> inject lesson -> evaluate
```

This is inspired by products such as [Mirrors](https://mirrors.dev/). The goal is a transparent, reusable implementation that students and other developers can inspect, extend, and benchmark.

## Version 0 scope

- One Python agent integration, with a small core interface that can support more adapters later.
- Trace capture for task, tool call, tool result, and run outcome events.
- Human review before a lesson becomes active.
- A rule-based router with a clear `match` / `no match` decision and recorded reason.
- A simple local view or CLI for inspecting traces and lessons.
- A reproducible comparison of the baseline agent, an always-on lesson, and a routed lesson on held-out tasks.

Automatic pattern discovery, multiple frameworks, dynamic tool-schema editing, and a hosted dashboard are later possibilities. They are outside version 0.

## Start here

1. Read the [architecture and shared data contract](docs/architecture.md).
2. Read the [evaluation plan](docs/evaluation.md).
3. Pick an open [issue](../../issues), comment that you are taking it, and open a draft pull request early.
4. Follow [CONTRIBUTING.md](CONTRIBUTING.md) for branches and reviews.

The runnable example will be added as the first workstream lands. Until then, the repository is a coordination and design starting point.

## Team workstreams

| Workstream | First deliverable |
| --- | --- |
| Coordination and integration | Agree on the example agent, data contract, and end-to-end demo flow |
| Trace capture and SDK | Emit versioned trace records from one agent integration |
| Failure review and lessons | Group the repeated failure and support lesson draft, approval, and rejection |
| Runtime router | Match an approved lesson against current step state and record the decision |
| Evaluation | Build held-out tasks and compare baseline, always-on, and routed conditions |

## Responsible trace collection

Tool inputs and outputs may contain secrets or personal data. The version 0 example should use synthetic data, redact sensitive fields before storage, and never commit raw private traces or API keys.

## License

MIT. See [LICENSE](LICENSE).
