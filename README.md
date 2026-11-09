# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) presets for chiva's repos.

| Preset | For | What it adds |
|---|---|---|
| `local>chiva/renovate-config` | every repo | Weekly batch on Monday morning (Europe/Madrid), 3-day release age, one non-major group, automerge of green non-major/digest/pin/lock-file PRs; majors stay manual, and so do digest-only updates of tag-pinned actions (a moved tag), grouped in their own PR so they never hold back the non-major batch; branch-tracked actions (`# main`/`# master`) from `home-assistant`, `hacs` and `frenck` automerge in a separate group; actions pinned to full semver tags |
| `local>chiva/renovate-config:python-app` | Python services shipped as images | Exact pins in `pyproject.toml`, `requires-python` untouched, python + uv images moved together and held below 3.14 |
| `local>chiva/renovate-config:ha-integration` | Home Assistant integrations | `pytest-homeassistant-custom-component` in its own manually merged PR; `hassil` and `home-assistant-intents` follow it by hand |
| `local>chiva/renovate-config:ha-addon` | Home Assistant add-ons wrapping a chiva image | Lockstep updates of the wrapped image across Dockerfile, `build.yaml` and `config.yaml`, proposed as soon as it is released (no release age, no weekly window) |

A repo's `renovate.json` extends `local>chiva/renovate-config` plus its type preset, and keeps only its own
`enabledManagers`, custom managers and package rules. Repo rules run after preset rules, so they win.

Automerge is safe only because [github-infra](https://github.com/chiva/github-infra) enforces required status
checks on every default branch: never extend this preset from a repo it does not manage.

Known gap: `minimumReleaseAge` does not reach lock-file maintenance, because the package manager resolves it
and Renovate integrates the cooldown only with npm and Poetry. uv (`exclude-newer`) and pnpm
(`minimumReleaseAge`) must enforce it themselves; until each repo configures that, its lock refreshes can
include releases younger than three days.

`ha-addon` expects config.yaml's version to carry the image it wraps:

```yaml
# renovate: datasource=docker depName=ghcr.io/chiva/<image>
version: "1.2.3"
```
