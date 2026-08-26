# Work Tracking for Human–LLM Collaboration

## A comparative field study of Beads, Backlog.md, and an event-log control

Work trackers usually answer questions such as “What is open?” and “What is
blocked?” Human–LLM collaboration adds a different question: **even if a task
is ready, who has authorized it to start?**

This article reports a small engineering field study of three approaches:

- [Beads](https://github.com/gastownhall/beads), evaluated at
  [version 1.2.2](https://github.com/gastownhall/beads/releases/tag/v1.2.2);
- [Backlog.md](https://github.com/MrLesk/Backlog.md), evaluated initially at
  version 1.48.0 and rechecked at
  [version 1.50.1](https://github.com/MrLesk/Backlog.md/releases/tag/v1.50.1);
- a minimal Markdown/JSONL control designed to express the study requirements
  without a product interface.

The study informed [Work Ledger](https://github.com/Gronshin/work-ledger), a
separate implementation rather than a fork of either evaluated project.

## Research question

Can a local, Git-friendly tracker preserve dependencies, resumable history,
and human authorization boundaries while remaining economical in the context
returned to an LLM coordinator?

The study treated the tracker as a hidden operational layer. A person continued
to discuss ordinary work; the coordinator translated that work into tracker
state. Direct end-user tracker usability was outside the study.

## Method

The comparison had two stages:

1. A deterministic replay applied the same generic hierarchy, dependency,
   readiness, lifecycle, and history operations to all three approaches.
2. Nine subsequent cycles followed real documentation, governance, review, and
   release work. The primary operational representation rotated while the
   other representations were reconciled for comparison.

The cases were real but sanitized for publication. Internal task names,
repositories, paths, identifiers, authorization records, account details, and
raw histories are not published. The retained public claims are either
reproducible with generic fixtures or explicitly labelled as field
observations. The private source material is not available for independent
audit, which limits the strength of the conclusions.

The study did not use a composite score. It examined:

- hierarchy and blocking dependencies;
- dependency-ready queries;
- separation of readiness from human authorization;
- append-only history and resume quality;
- readability in Git;
- returned output volume and storage growth;
- recovery and operational maintenance;
- telemetry and supply-chain boundaries;
- additional attention caused by the hidden tracker layer.

## Initial replay measurements

The following values are single-machine observations, not performance
benchmarks. The tools have different capabilities and storage models.

| Measurement | Beads 1.2.2 | Backlog.md 1.48.0 | Markdown/JSONL control |
| --- | ---: | ---: | ---: |
| Captured tracker-state bytes | 1,954,081 | 6,546 | 18,595 |
| Returned bytes for list + one item + readiness | 12,833 | 1,691 | 1,455 |
| Returned whitespace words for the same reads | 1,041 | 178 | 63 |

No model call was made to estimate tokens. Setup and authoring effort were not
equivalent, so these measurements must not be reduced to a ranking.

## Findings

### Beads: strong native mechanics, larger operational surface

Beads exposed the richest native lifecycle, dependency, readiness, and history
surface in the tested workflow. That reduced the need to invent lifecycle
primitives.

Its initial reads returned the most context, and its native state grew the most
during the longer field cycles. Version 1.2.2 also enabled third-party command
metrics by default in the evaluated path. This triggered a safety stop; the
study resumed only through a local containment wrapper with metrics and event
flushing disabled. This is a version- and configuration-specific observation,
not a claim about every Beads release.

### Backlog.md: readable files and improving readiness support

Backlog.md kept tasks in human-readable Markdown and represented hierarchy and
blocking dependencies without requiring a database-oriented inspection path.
It was the easiest representation to inspect directly in Git.

The initially tested version required a small read-only adapter for a
dependency-ready query. By version 1.50.1, native readiness support had made
that proposal obsolete. This was an important reminder to re-evaluate a living
upstream before designing extensions or forks.

One confirmed limitation remained in the tested release: board export included
a parent and direct child but omitted grandchildren and deeper descendants.
The generic reproduction was reported as
[MrLesk/Backlog.md#944](https://github.com/MrLesk/Backlog.md/issues/944).

Native readiness still answered only a dependency question. The study's
separate human-authorization requirement remained an external concern.

### Markdown/JSONL control: exact semantics, manual consistency cost

The control made the required distinction explicit: dependencies could be
satisfied while authorization remained absent. It returned the smallest
initial query context and represented structured lifecycle events directly.

That precision came with a maintenance cost. JSONL history, current Markdown
state, and comparison projections had to remain consistent through manual
procedures and validators. Without a single writer and deterministic rebuild,
the control could become its own infrastructure project.

## From control to Work Ledger

The experiment did not establish one universally superior tracker. It instead
identified a requirement combination that none of the evaluated product
workflows represented directly:

1. one append-only event history as the only authority;
2. deterministic, disposable Markdown projections;
3. blocking dependencies independent from hierarchy;
4. dependency readiness independent from human authorization;
5. explicit assessment and authorization evidence bound to the current task
   specification;
6. no scheduler, daemon, telemetry, network client, or automatic next-task
   loop.

[Work Ledger](https://github.com/Gronshin/work-ledger) implements that
combination as a small Node.js and TypeScript CLI. It was created as a separate
project so its event model could follow these boundaries without changing the
philosophy or compatibility surface of an existing tracker.

The conclusion is therefore not “build instead of reuse.” The sequence was the
opposite: evaluate existing tools, update one of them before judging its gaps,
report a generic upstream defect, retain the useful ideas, and build only after
the remaining requirement was specific and testable.

## Limitations

- This was one longitudinal field study conducted through one coordinator and
  one working environment.
- The nine cycles were real but not statistically independent or equivalent.
- Direct usability for people operating each tracker was not tested.
- Raw field evidence is private, so external readers cannot audit the complete
  event history.
- Versions and project behavior may change after the stated checkpoints.
- Storage and output measurements describe the tested workflows, not general
  throughput or product quality.
- Work Ledger was derived from the study requirements; evaluating it on future
  work is a product validation step, not independent confirmation of the study.

## Future work

The next useful evaluation is prospective: operate Work Ledger on new tasks,
measure recovery after meaningful pauses, and test whether its authorization
model remains understandable without maintaining parallel trackers. A later
study could add multiple coordinators and direct human use, publish a fully
synthetic reproducibility fixture, and pre-register comparison criteria before
collecting observations.

---

Author: Gleb <gronshin@pm.me>.

This article was developed with AI-assisted analysis and drafting. Gleb
reviewed the material and is responsible for publication.

Licensed under
[Creative Commons Attribution 4.0 International](../LICENSE.md).
