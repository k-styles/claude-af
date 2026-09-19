# claude-af

Claude Code plugin for [`af`](https://github.com/tobiasosborne/vibefeld) (Vibefeld), Tobias J. Osborne's adversarial proof framework for mathematical research.

Provers construct. Verifiers attack. No agent does both.

This is packaging only. The tool, and the thinking behind it, belong to other people — see [Credit](#credit).

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

## Credit

**None of the ideas here are mine. I packaged someone else's tool as a plugin, that is the whole contribution.**

`af` / Vibefeld is the work of **Tobias J. Osborne** (Leibniz Universität Hannover): <https://github.com/tobiasosborne/vibefeld>. The adversarial architecture, the event-sourced ledger, the taint model, the 60+ command CLI, all of it is his. Its predecessor, [alethfeld](https://github.com/tobiasosborne/alethfeld), is also his.

The discipline this skill encodes, and the reason role isolation is non-negotiable, comes from **Danielle Loader, Jonathan Oppenheim (University College London) and Tobias J. Osborne**, *How to train your slop cannon*, which is the guide to using LLMs on research mathematics and physics without fooling yourself. Its section on adversarial formalisation points readers straight at `af`. Read the guide before you use either.

This repository contributes a `plugin.json`, a `marketplace.json` and one `SKILL.md`. Bugs in the tool belong upstream. Bugs in the packaging belong here.

## License

MIT
