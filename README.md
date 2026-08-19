# @skyf0xx/hedgehog-core-authored

Hedgehog's authored core: designs a layer sequence and generates a
verified workspace for a project that fits neither shipped core, and
carries the same design for adopting Hedgehog's discipline into an
existing repo without bootstrapping a workspace at all.

Unlike other cores, this package ships
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

## Working on this core

This is a versioned npm package that the Hedgehog engine's `init` fetches
by name, carrying `authored`'s own agent, skills, and the
`hedgehog-core.yaml` manifest that names them to the engine. Unlike
`full-stack-app` and `landing-page`, this package ships no pre-built
`workspace/` — see the engine repo
([`skyf0xx/hedgehog`](https://github.com/skyf0xx/hedgehog)) and its
[`ARCHITECTURE.md`](https://github.com/skyf0xx/hedgehog/blob/master/ARCHITECTURE.md)
for how `init` resolves and fetches a core package, and for why `authored`
carries no install flag — that mechanism lives there, not here.

No root `CLAUDE.md` lives in this repo. `CLAUDE.core.md` and
`CLAUDE.core.adopted.md` are payload files: their content is installed
into a *consuming project's* generated `CLAUDE.md`, filling that
project's `{{CORE_SECTION}}` placeholder (the freshly-authored and
adopted cases respectively). A plain root `CLAUDE.md` here would
auto-load into any coding agent working on this package itself, bleeding
project-build context into a repo where no Hedgehog build ever runs —
build guidance for a project using this core lives in that project's own
generated `CLAUDE.md`, never here.

Changing this core means editing `agents/layer-eng.md` or one of the
five skills under `skills/` (`hedgehog-core-design`,
`hedgehog-bootstrap-authored-core`, `hedgehog-authored-loop`,
`hedgehog-adopt`, `hedgehog-adopt-elicit`). There is no `workspace/`
template and no regeneration script here — this core's stack and layer
sequence are designed live, per project, by `hedgehog-core-design`
rather than pre-built. A change here is a release of this package, not
of the engine: bump `package.json`'s version, commit, and merge to
`main` — this repo's own `publish.yml` tags and publishes from there.

This core has no generator tooling of its own — no `tools/`,
`generators/`, or `scripts/` directory, since it has no `workspace/`
template to scaffold boilerplate for. The scaffolding-first policy still
applies at the point it becomes relevant: if `hedgehog-bootstrap-authored-core`
or a future authored-core workspace ever needs a piece of repeatable
generated boilerplate, prefer building or extending a generator over
hand-authoring the output once, modeled on
[`hedgehog-core-full-stack-app`](https://github.com/skyf0xx/hedgehog-core-full-stack-app)'s
`workspace/tools/generators/`, the concrete working example.
