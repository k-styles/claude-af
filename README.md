# claude-af

Claude Code plugin for [`af`](https://github.com/tobiasosborne/vibefeld) (Vibefeld), the adversarial proof framework for mathematical research.

Provers construct. Verifiers attack. No agent does both.

## Install

```bash
claude plugin marketplace add k-styles/claude-af
claude plugin install af@vibefeld
```

The plugin ships one skill. It triggers on its own when you ask to prove, verify or audit something in a directory holding `.af/`, or you invoke it directly with `/af`.

## What it does

`af` is a CLI, and a capable one: 60+ commands, self-documenting, built so an agent can ask it what to do next. What a CLI cannot enforce is the discipline around it. An agent will happily prove a step, admire it, and accept its own work.

This plugin encodes the part that has to sit outside the tool:

- **Role isolation.** Never prove and verify in one session. Dispatch the opposing role as a subagent that sees only the numbered tree, or use a separate session. An agent acting as both must declare it in the proof, because self-acceptance is provenance, not proof.
- **Severity honesty.** `critical` and `major` block. `minor` and `note` do not. Neither inflate nor deflate.
- **What taint costs.** `admit` accepts a step without proving it and propagates uncertainty to everything downstream. It is the honest record of what the proof rests on, never a way to turn a tree green.
- **The four fakes.** Archiving the step you could not prove, accepting your own work, quietly weakening a challenged statement, burying an assumption in prose.

Everything else it defers to the CLI: `af role-help <role>`, `af <command> --help`, `af status`, `af jobs`. That is deliberate, so the skill does not drift as `af` grows commands.

## Requires

`af` on PATH. Build it from a clone of [vibefeld](https://github.com/tobiasosborne/vibefeld):

```bash
./scripts/build.sh install    # stamped build -> $GOBIN or ~/go/bin/af
```

Go 1.25.5 or newer.

## Not affiliated

`af` and Vibefeld are Tobias Osborne's work. This repo is a third-party packaging of a Claude Code skill for it, nothing more. Bugs in the tool belong upstream; bugs in the skill belong here.

## License

MIT
