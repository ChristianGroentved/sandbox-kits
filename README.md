# Sandbox Kits

## Pi

Temporary Pi v3 workload.

Remove this once Docker publishes an official v3 Pi workload.

## Pi GCP

Adds custom Vertex-hosted models to Pi.

Provides:

- Qwen
- GLM
- Vertex AI authentication
- Vertex AI network access

### Authentication

Authenticate on the host:

    gcloud auth application-default login

Register the dynamic sandbox secret:

    sbx secret set gcp-vertex \
      --command 'gcloud auth application-default print-access-token'

The access token is resolved on the host and is never exposed
inside the sandbox.

### Run

    sbx run ./kits/pi \
      --kit ./kits/pi-gcp \
      --kit-arg pi-gcp.project=<project> \
      .