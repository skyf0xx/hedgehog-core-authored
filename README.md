# @skyf0xx/hedgehog-core-authored

Hedgehog's authored core: designs a layer sequence and generates a
verified workspace for a project that fits neither shipped Golden Core,
and carries the same design for adopting Hedgehog's discipline into an
existing repo without bootstrapping a workspace at all.

Unlike the `full-stack-app` and `landing-page` cores, this package ships
no pre-built workspace — the stack is chosen and the layer sequence
derived per project by `hedgehog-core-design`, then generated live and
verified before it's committed.

## Contents

- `agents/layer-eng.md` — builds each layer of an authored or adopted
  core's sequence, one `hedgehog next` packet at a time, gated by
  `hedgehog verify`.
- `skills/hedgehog-core-design` — elicits the drivers, names the system
  shape, picks the stack, derives the layers, and writes
  `.hedgehog/core.yaml` for a project that needs a designed core.
- `skills/hedgehog-bootstrap-authored-core` — generates the workspace for
  the stack `hedgehog-core-design` chose, closing Bootstrap on an
  authored core.
- `skills/hedgehog-authored-loop` — the operating loop for every unit of
  work on an authored core: one layer per `hedgehog next` packet via
  `layer-eng`, the module-axis reading, the Correction Protocol, and the
  Stop Condition, all driven from `.hedgehog/core.yaml`.
- `skills/hedgehog-adopt` — brings Hedgehog's discipline to an existing
  repo without bootstrapping a workspace: reads the repo read-only,
  proposes a linear-chain `.hedgehog/core.yaml` whose `verify` commands
  are the repo's own, confirmed with the user, and writes only
  `.hedgehog/`.
- `skills/hedgehog-adopt-elicit` — a short clarifying pass for a large or
  under-specified change request on an already-adopted repo, run by
  `hedgehog-adopt` before adding that intent.
- `CLAUDE.core.md` — fills a Hedgehog project's root `CLAUDE.md`
  `{{CORE_SECTION}}` placeholder for a freshly authored core.
- `CLAUDE.core.adopted.md` — fills the same placeholder for a repo
  Hedgehog adopted rather than bootstrapped.
- `hedgehog-core.yaml` — this package's manifest: name, the selection
  prose the Hedgehog planner matches a project description against, and
  which agents/skills/templates it carries.

## Using this package

A Hedgehog installation depends on this package for the `authored` core
rather than carrying its content directly. See the Hedgehog engine
(`@skyf0xx/hedgehog`) for the installer and build-graph tooling that
consumes it.
