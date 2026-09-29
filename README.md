# plugin-example-external

The reference **dual-placement** plugin (`verb:externalprobe`) — one provider,
two placements, zero authoring change.

The plugin's importable provider package serves the `externalprobe` check verb
and its self-contained CUE schema. It is compiled into charly when the candy is
listed in `compiled_plugins:` (registered in-process), and the same provider is
served out-of-process over go-plugin gRPC by the `cmd/serve` shim when it is not.
Placement is invisible above the provider registry — the verb dispatches
identically either way.

## What it provides

| Capability | Surface |
|---|---|
| `verb:externalprobe` | the `externalprobe:` check verb — a deterministic pass echoing its `marker` |

It is the companion of the built-in `plugin-example`; the out-of-process
build/deploy legs are demonstrated by the `plugin-example-builder`,
`-deploy`, and `-command` plugins.

## How to use it

Compose the plugin candy in a box or check bed's `candy:` list:

```yaml
- '@github.com/opencharly/plugin-example-external/candy/plugin-example-external:<tag>'
```

Then author the verb in a plan:

```yaml
- check: the externalprobe verb passes
  id: externalprobe-dispatches
  externalprobe: {marker: external-plugin-ok}
  context: [runtime]
```

## Layout

- `candy/plugin-example-external/` — the plugin module: `plugin.go` (the provider
  + `NewProvider()`/`NewMeta()`), `schema/externalprobe.cue` (the self-contained
  `#ExternalprobeInput`), `params/cue_types_gen.go`, `cmd/serve/main.go`.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-check:check` — the check verb catalog. This candy
  carries no `skill:` entity of its own; the gap is tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- `/charly-internals:plugin` — the plugin/provider model.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI.
