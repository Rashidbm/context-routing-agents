# Evaluation plan

## Question

Does a relevant lesson delivered only when its trigger matches improve new task outcomes, with fewer unwanted interventions than placing that lesson in every task prompt?

## Conditions

Use the same agent, model version, tools, task set, budgets, and success checks for each condition:

1. **Baseline:** no lesson.
2. **Always-on:** lesson in every task prompt.
3. **Routed:** approved lesson inserted only after a matching event.

Define lessons using a development set. Keep a separate held-out set of tasks unseen during lesson writing and trigger tuning. Include both matching cases and negative cases where the lesson should not appear. If the model is nondeterministic, repeat runs and report counts and variation rather than a single cherry-picked example.

## Metrics

- Task success rate: successful tasks / evaluated tasks.
- Repeated failure rate: runs that repeat the same failed action after the trigger / applicable runs.
- Router precision and recall on labeled match and non-match steps.
- Number of agent steps, model tokens, wall time, and estimated cost per task.

Publish the task list, success criteria, lesson version, raw aggregate counts, and scripts or commands used to reproduce results. Do not infer a task-success improvement from applying a trigger to historical traces alone: replay can show match coverage and false positives, but the changed agent must be run again to measure its outcome.
