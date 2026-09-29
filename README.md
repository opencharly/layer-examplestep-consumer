# examplestep-consumer

Reference consumer candy for the build-time plugin-execution example.

`examplestep-consumer`'s `plan:` carries a **build-context** `run:` step with
`plugin: examplestep`. At image build it drives charly's build-path plugin
connect + `invokeVerbBuildEmit` seam: the out-of-tree `candy/plugin-example-step`
is host-built and connected out-of-process, its `OpEmit` invoked, and the
returned Containerfile `RUN` spliced into the generated Containerfile — baking
`/opt/examplestep-baked` into the image. Compose it **with**
`candy/plugin-example-step` (which provides the `examplestep` verb); the runtime
check proves the bake ran.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `examplestep-consumer` |
| Plugin verb | `examplestep` (provided by `candy/plugin-example-step`) |
| Artifact | `/opt/examplestep-baked` |
| Service / port | none |

This is a **reference/fixture** candy: it exists to exercise and demonstrate the
build-context external-plugin step seam, not to be composed into production
boxes. Its deploy-context counterpart is `layer-examplestep-deploy-consumer`.

## How to use it

Compose it together with the plugin candy that provides the verb:

```yaml
my-step-demo:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-examplestep-consumer:v2026.241.1216'
      - '@github.com/opencharly/plugin-example-step/candy/plugin-example-step:v2026.239.1643'
```

After the image is built, the runtime check proves the bake ran:

```bash
test -f /opt/examplestep-baked
```

## Layout

- `charly.yml` — the `examplestep-consumer:` candy entity: the plugin-candy
  `candy:` reference, the build-context `run:` step, and the runtime `check:`
  step.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Plugin provider: `candy/plugin-example-step`
- Deploy-context counterpart: `layer-examplestep-deploy-consumer`
- Plugin authoring: `/charly-internals:plugin`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
