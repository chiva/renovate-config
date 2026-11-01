# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) presets for chiva's repos.

| Preset | For | What it adds |
|---|---|---|
| `local>chiva/renovate-config` | every repo | Weekly batch on Monday morning (Europe/Madrid), 3-day release age, one non-major group, automerge of green non-major/digest/pin/lock-file PRs; majors stay manual |
| `local>chiva/renovate-config:python-app` | Python services shipped as images | Exact pins in `pyproject.toml`, `requires-python` untouched, python + uv images moved together and held below 3.14 |
| `local>chiva/renovate-config:ha-integration` | Home Assistant integrations | `pytest-homeassistant-custom-component` in its own PR |
| `local>chiva/renovate-config:ha-addon` | Home Assistant add-ons wrapping a chiva image | Lockstep updates of the wrapped image across Dockerfile, `build.yaml` and `config.yaml` |

A repo's `renovate.json` extends `local>chiva/renovate-config` plus its type preset, and keeps only its own
`enabledManagers`, custom managers and package rules. Repo rules run after preset rules, so they win.

Automerge is safe only because [github-infra](https://github.com/chiva/github-infra) enforces required status
checks on every default branch: never extend this preset from a repo it does not manage.

`ha-addon` expects config.yaml's version to carry the image it wraps:

```yaml
# renovate: datasource=docker depName=ghcr.io/chiva/<image>
version: "1.2.3"
```
