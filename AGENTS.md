# AGENTS.md — plugin-example-external

Standalone plugin repo for the `externalprobe` capability
(`verb:externalprobe`) — the reference dual-placement plugin. The plugin is a Go
module at `candy/plugin-example-external/` (module path
`github.com/opencharly/plugin-example-external/candy/plugin-example-external`);
the root `charly.yml` only declares `discover: candy` so the repo is a project
and its candy is scanned.

Canonical files:

- `candy/plugin-example-external/charly.yml` — the `plugin-example-external:`
  candy entity (`plugin:` block, `plan:` check).
- `candy/plugin-example-external/plugin.go` — the provider (`NewProvider()` +
  `NewMeta()`).
- `candy/plugin-example-external/schema/externalprobe.cue` — the self-contained
  `#ExternalprobeInput`.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-internals:plugin` — the plugin authoring reference: the `plugin:`
  block, the unified Provider model, the per-plugin CUE-schema contract,
  placement. Load before touching the provider or schema.
- `/charly-check:check` — the declarative check-verb surface the `externalprobe:`
  verb is authored through.
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `go build ./...` in `candy/plugin-example-external/` — compile the plugin
  module.
- `go test ./...` in `candy/plugin-example-external/` — the plugin's Go tests.
- `charly box validate` at the repo root — the structural check (the candy +
  `plugin:` block, CUE schema).
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate.

## Modify this repo

- Edit the `plugin-example-external:` candy entity, the Go source, and
  `schema/externalprobe.cue` **together** — the schema is the single source for
  the `params/` struct.
- The plugin is **dual-placement** (compiled-in or served over go-plugin gRPC);
  keep both paths working and do not describe it as one placement only.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
