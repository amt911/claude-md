# [Project Name] — Claude Guide

## Start here

Run `/graphify` before each session. The persistent graph at `graphify-out/graph.json` summarizes architecture, dependencies, and cross-cutting concepts without re-reading the repo each time.

## ⚡ graphify — use every session

```
/graphify            # first run (builds graph from scratch)
/graphify --update   # incremental update (only re-extracts changed files)
/graphify query "<question>"    # architecture questions instead of opening multiple files
/graphify explain "<name>"      # locate a concept or symbol
/graphify path "A" "B"          # dependency path between two modules
```

Outputs in `graphify-out/`: `graph.json` (source of truth), `GRAPH_REPORT.md` (god nodes, communities, surprising connections), `graph.html` (interactive view).

Run `/graphify --update` at end of session if you touched docs or images (code changes rebuild via hook if installed).

## Stack

- **[Runtime/Framework]** vX.Y — [brief role]
- **[Language]** — [notes]
- **[Key libs]** — [role]

## Commands

```bash
# dev
[dev command]

# test
[test command]       # unit/integration
[test:watch]         # watch mode
[test:cov]           # coverage (gate: 80%)
[test:e2e]           # e2e

# build / lint
[build command]
[lint command]
```

## Tests and quality

- **[Test runner]** + **[assertion lib]** + **[component lib if applicable]** — unit/integration. Config: `[config file]`.
- **[E2E runner if any]** — e2e. Config: `[config file]`, specs in `[dir]/`.
- File convention: `*.test.ts(x)` or `*.spec.ts(x)` colocated with source.
- **Coverage gate: 80%** (statements/branches/functions/lines). Don't lower the gate — exclude with justification instead.

### What to test per folder

| Folder | What | Status |
| --- | --- | --- |
| `lib/` | Pure utilities, hooks — deterministic, minimal mocks | Pending |
| `components/` | Logic-bearing components: forms, dialogs, toggles. Mount + `user-event` interactions. Exclude UI primitives | Pending |
| `hooks/` | Custom hooks via `renderHook`. Mock timers/fetch only when unavoidable | Pending |
| `app/api/` | Route handlers: call exported function directly with constructed `Request`. Check status codes, validation, error paths | Pending |

### TDD — required for new logic

For new code in `lib/`, `hooks/`, `components/` (non-primitive), `api/`:

1. **Red** — write failing test that describes the behavior.
2. **Green** — implement the minimum to pass.
3. **Refactor** — clean up under green tests.

Exceptions (TDD not required):
- Pure visual/style changes (CSS, layout, copy).
- UI primitives (tested indirectly by consumers).
- Spikes/exploration — but add tests before merging.

Rules:
- **Don't lower the 80% gate** to ship a PR. Exclude untestable modules in config with a written reason.
- **Test over mock**: exercise real code with minimal stubs; don't mock entire modules.
- **`renderHook` / `render`** from Testing Library for hooks/components. Events via `user-event`.

### Operative conventions

- **Global setup file** — define `matchMedia`, `ResizeObserver`, `IntersectionObserver`, `localStorage` stubs once. Don't redefine per test.
- **Split by aspect** when a test file exceeds ~300 LoC: `.flow.test.ts`, `.errors.test.ts`, `.branches.test.ts`.
- **Exclude with justification** in config, never silently. Example:
  ```js
  // JSDOM cannot simulate layout/animation timing — cover via E2E
  exclude: ['src/hooks/use-grid-reflow.ts']
  ```

## Working rules

- **Don't install packages without asking** — the stack is intentional.
- **TDD by default** for new logic. Don't merge logic without tests.
- **Don't lower the coverage gate** — exclude with justification instead.
- **[Add project-specific rules here]**

## Git & GitHub

- **Commits and branches OK** — create commits and new branches whenever it makes sense, without asking first.
- **Never push** — no `git push` under any circumstance, and absolutely never `git push --force` / `--force-with-lease`. Leave pushing to the user.
- **GitHub via `gh`** — if the `gh` CLI is available, you may open pull requests, issues, and similar (comments, labels, etc.). These don't require pushing on your part beyond what `gh` itself does for an already-pushed branch.
