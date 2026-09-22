## This project's core: authored

This project's core was designed for it by `hedgehog-core-design` at
planning intake, rather than taken from a shipped core. The layer
sequence, the stack, and each layer's file scope and verification live in
two files, and both are locked:

- **`.hedgehog/core.yaml`** — the design authority: `id`, the layer
  order, and per layer its `scope` globs, `verify` command, and commit
  message. `hedgehog plan` compiled the build graph from this.
- **`.hedgehog/core-design.md`** — the rationale: the system shape (what
  this project fundamentally is), the stack and why it was chosen, a line
  per layer on what it owns and why it sits where it does, and the
  module-axis decision.

Read `core-design.md` to know what this project is and what each layer
owns.

**The packet is what actually runs**, not `core.yaml` directly, once
`hedgehog plan` has compiled a task — see `hedgehog-authored-loop`'s
"core.yaml vs. the packet" for the reconciliation mechanic (DRIFT,
`--recompile`) when the two disagree; that skill is the source, not
restated here. Changing either locked file re-shapes every task the
graph compiles *from then on* — a `planner` decision through the
Correction Protocol, never a quiet edit.

### The skills — invoke these, don't improvise

- **`hedgehog-authored-loop`** — every unit of work once bootstrapped:
  `hedgehog claim` reserves the packet for one ready layer (`hedgehog
  next` previews it read-only, without reserving), `layer-eng` builds
  it, `hedgehog verify` gates and commits it. Also holds the Correction
  Protocol and this core's Stop Condition. Invoke it at the start of any
  build session and for "what's next".
<!-- hedgehog:bootstrap-only start -->
- **`hedgehog-bootstrap-authored-core`** — run **once**, at project
  start, to generate and verify this core's workspace from the stack in
  `core-design.md`. Skip once its `feat(<id>): workspace` commit exists.
<!-- hedgehog:bootstrap-only end -->
- **`conventional-commits`** — when a change spans several layers in one
  working-tree pass and needs splitting back into per-layer commits
  (mainly Correction Protocol cleanups).

### The agents — delegate the judgment calls

`layer-eng` builds one layer per `hedgehog claim`ed packet, working from
the packet's ALLOWED SCOPE and `core-design.md`'s description of what
that layer owns. Reports the work done; never commits it. See
`hedgehog-authored-loop` for exactly which agent runs which part of the
loop, including the Layer Transition Checks' use of `reviewer` and the
Stop Condition's handoff to `tweaker` — that skill is the source, not
restated here.

## The constants (do not deviate)

### Stack (locked)

Named in `.hedgehog/core-design.md`'s stack record — language, package
manager, framework(s), test runner — and realized in the workspace
`hedgehog-bootstrap-authored-core` generated. The stack was chosen
deliberately for this project's system shape; a felt need for a new
library is worth surfacing before adding it, since it usually belongs to
the layer's design rather than to a build step.

### Layout

The layer `scope` globs in `.hedgehog/core.yaml` define where each
layer's code lives — that file is the layout, and it's enforced:
`hedgehog verify` rejects a task that writes outside its own scope.

```text
.hedgehog/
  hedgehog.db         the build graph — intents, compiled tasks, verifications, committed to git
  core.yaml           this core's layer sequence, scope, verification, commit messages — locked
  core-design.md      the design rationale behind core.yaml — write-once, from planner
  BMAD/               vendored BMAD-METHOD shelf's raw output (brief, PR-FAQ, PRD, UX spec, research) —
                       write-once, from planner
```

### Core rules

- **One layer, one commit**, in the exact message `.hedgehog/core.yaml`
  names for that layer.
- **Sequential through the chain.** A layer starts once the one before it
  passes its own verification — `hedgehog next` enforces this.
- **Scope is the boundary.** A layer writes inside its ALLOWED SCOPE and
  nowhere else; a change that needs to land elsewhere is a correction,
  not a wider write.
- **A layer owns one artifact**, reached through the interface
  `core-design.md` named — that boundary is what makes the layer
  independently verifiable.
- **The layer's own `verify` command gates every commit.** Never weaken
  it to clear a gate; a layer whose command passes with no tests
  certifies nothing.
- **Fix wrong layers at the source** via the Correction Protocol — never
  a downstream workaround.
- **The layer sequence itself is locked.** Changing `.hedgehog/core.yaml`
  or `.hedgehog/core-design.md` is a `planner` decision, not a quiet
  edit.
