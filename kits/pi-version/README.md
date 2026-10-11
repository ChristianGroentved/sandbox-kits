# Pi Version Mixin

Installs a chosen Pi release on top of the Pi workload.

The published `docker.io/docker/sbx-kit-pi` bakes its Pi version in at
publish time and exposes no `version` argument. This mixin reinstalls Pi
from npm when the sandbox is created.

## Requirements

- Pi workload

## Arguments

| Argument | Default  | Description                              |
| -------- | -------- | ---------------------------------------- |
| version  | required | Pi release, e.g. `1.0.4`, or `latest`    |

## Trade-offs

- Adds an npm download (~10s) to every new sandbox. Skipped when the
  baked Pi already matches the requested release.
- Sandbox creation fails if the npm install fails.

## Usage

```bash
sbx run \
  docker.io/docker/sbx-kit-pi:latest \
  --kit ./kits/pi-version \
  --kit-arg pi-version.version=1.0.4 \
  .
```
