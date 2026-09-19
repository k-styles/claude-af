---
name: af
description: "Use when constructing, attacking, or auditing a rigorous mathematical proof with the af CLI (Vibefeld, the Adversarial Proof Framework) — any request to prove a claim step by step, verify or challenge an existing proof tree, work through af jobs, or inspect proof state in a directory containing .af/. Provers construct, verifiers attack, and no agent may do both."
---

# af — adversarial proof construction

`af` builds natural-language mathematical proofs through adversarial collaboration. Provers convince, verifiers attack, and the proof is its own event history. Upstream: [tobiasosborne/vibefeld](https://github.com/tobiasosborne/vibefeld).

If `af` is not on PATH, build it from a clone of that repo with `./scripts/build.sh install`, which stamps version and commit, or `go build ./cmd/af` for a quick unstamped binary. Go 1.25.5 or newer.

## The one rule that matters

**Never prove and verify in the same session.** A verifier that saw the prover's reasoning is not a verifier, it is a rubber stamp. When work needs both roles, either dispatch the opposing role as a subagent that receives only the numbered tree, or tell the user to run it in a separate session.

If you do end up acting as a single agent on both sides, say so in the proof itself: every acceptance there is a self-acceptance, which is provenance, not proof.

## The CLI is the documentation

`af` is built to tell an agent what to do next. Before inventing an approach, ask it:

```bash
af status                  # proof tree with states and taint
af jobs                    # what is available to work on right now
af role-help prover        # commands for one role (also: verifier, info, operator)
af <command> --help        # every command carries worked examples and common mistakes
af get 1.2                 # a node with its full context
```

Do not reproduce this file's tables from memory when the CLI can answer. Run it.

## Workflow

```
af init            once, by the operator
af jobs            find available work
af claim <id>      take a node (locks it)
  ... prover or verifier work ...
af release <id>    hand it back
```

Claims expire. `af extend-claim` when the work runs long, `af reap` clears stale locks.

## Prover

| Command | Use |
|---|---|
| `refine <id>` | add child nodes that establish the parent |
| `amend <id>` | correct a node's statement |
| `request-def` | demand a definition you need before proceeding |
| `resolve-challenge` | answer an objection raised against your node |

A leaf with no justification is an exposed gap. Label it. Never bury it in prose.

## Verifier

| Command | Use |
|---|---|
| `challenge <id>` | raise an objection, `--severity critical\|major\|minor\|note` |
| `accept <id>` | validate a node |

`critical` and `major` mean the issue must be fixed before the proof proceeds. `minor` and `note` are suggestions and do not block. Do not inflate severity, and do not deflate it either.

Attack every step. Accept nothing you cannot check yourself. Reaching for `accept` because the argument reads fluently is the exact failure this tool exists to prevent.

## Escape hatches, and what they cost

| Command | Cost |
|---|---|
| `admit <id>` | accepts without proof and **introduces taint** that propagates to everything downstream |
| `refute <id>` | marks the node disproven |
| `archive <id>` | abandons the branch |

Taint is the honest record of what a proof rests on. `af recompute-taint` forces recalculation, `af taint-trace` shows where it came from. Never admit a step quietly to turn a tree green.

## Reading state

```bash
af progress        # completion metrics
af health          # stuck states
af challenges      # open objections across the whole proof
af deps <id>       # dependency graph
af log             # the event ledger, the actual source of truth
af export          # LaTeX or Markdown
```

Node states run on three axes: workflow (`available`, `claimed`, `blocked`), epistemic (`pending`, `validated`, `admitted`, `refuted`, `archived`), and taint (`clean`, `self_admitted`, `tainted`, `unresolved`).

## Exit codes

| Code | Meaning | What to do |
|---|---|---|
| 0 | success | continue |
| 1 | retriable, usually a lock conflict | wait and retry, or `af reap` |
| 2 | blocked, usually a pending definition | resolve the definition first |
| 3 | logic error, invalid input | fix the command |
| 4 | corruption, ledger inconsistent | stop, run `af replay`, tell the user |

## Failure modes to refuse

These are the ways an agent fakes a proof. Do none of them:

- archiving the step you could not prove
- accepting your own work
- quietly weakening a statement after it was challenged
- burying an assumption in prose instead of declaring it

## Examples

Worked proofs ship with the upstream repo under `examples/` (sqrt2, Dobinski), and `prompts/` holds the supervisor prompts that drove them. Read one before starting a first proof in a new workspace.
