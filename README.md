<div align="center">

# Gleb · Gronshin

Building practical methods and local-first tools for human–AI collaboration.

</div>

## Focus

- explicit human decision and authorization boundaries;
- reproducible, Git-native workflows for people and agents;
- evidence-aware use of AI services and their changing terms;
- small open-source tools with quiet operational and supply-chain surfaces.

## Projects

- [U/S/G](https://github.com/Gronshin/usg) — a method for assessing uncertainty,
  change surface, and governance boundaries before execution.
- [Work Ledger](https://github.com/Gronshin/work-ledger) — a local-first,
  event-sourced tracker that keeps dependency readiness separate from human
  authorization.
- [Model Terms Observatory](https://github.com/Gronshin/model-terms-evidence) —
  verification-only metadata and hashes for tracking changes in AI vendor
  terms without redistributing their documents.

Together, these projects cover assessment, execution evidence, and changing
external constraints without turning access into authority or readiness into
permission.

## On caution

These projects are about boundaries, so it is worth saying what they are not.

They are not built to prove innocence. Earlier revisions of the method scored
how an execution path would *look* to someone arriving to audit it — proxy
access, disguised modes, account workarounds — and the record shows exactly
what that bought. Across the full ledger the governance gate reached
`explicit-confirmation` thirty-one times and refused nothing: thirty-eight
authorizations granted, none revoked, no task stopped. It had never changed an
outcome, only added a step before a yes.

So the components now measure whether work can be seen, attributed and stopped,
and whether it spends or affects what its initiator meant it to. They do not
measure appearance. Scoring appearance teaches you to lawyer rather than to
think, and it fills the record with defences against accusations instead of
facts about the work.

The method now engages where being wrong is expensive and recovery is not
available — publication, money, other people's systems, delegation past what
one person can review — and stays out of the way everywhere else. That is a
deliberate trade: unassessed work in exchange for cycle time, stated in the
method where it cannot be missed, and it expires with the observation that the
cost of error is still small.

The stance underneath is fatalist and optimist at once. Fatalist, because you
cannot insure against everything, and a system built to survive every
imaginable accusation mostly survives instead of working. Optimist, because
the safety that holds comes from measuring your own work and publishing what
the measurement found. On this record the gates caught nothing; attacking my
own method caught three real defects, and all three are written up rather than
quietly fixed.

## Writing

- [Work Tracking for Human–LLM Collaboration: a comparative field
  study](articles/work-tracking-for-human-llm-collaboration.md) — sanitized
  observations from evaluating Beads, Backlog.md, and an event-log control,
  followed by the design rationale for Work Ledger.

## Technology and working style

TypeScript, Node.js, Git, deterministic validation, local-first data, explicit
security boundaries, and documentation that states what the evidence does not
prove.

## License

Original authored profile and article content is available under
[CC BY 4.0](LICENSE.md).
