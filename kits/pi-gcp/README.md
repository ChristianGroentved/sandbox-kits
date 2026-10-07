# Pi GCP Mixin

Adds custom GCP Vertex AI models to Pi.

## Requirements

- Pi workload
- Host authenticated against GCP
- `gcp-vertex` sandbox secret configured

## Arguments

| Argument | Default  | Description   |
| -------- | -------- | ------------- |
| project  | required | GCP project   |
| location | global   | Vertex region |