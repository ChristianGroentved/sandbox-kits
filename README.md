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

## Testing before release

Pull requests are automatically validated to ensure the Sandbox Kit descriptors can be built and composed correctly.

This catches structural issues such as:
- invalid Kit descriptors,
- invalid capabilities,
- dependency conflicts,
- composition failures,
- packaging errors.

Build validation does **not** prove that Pi can successfully use the configured models.

Changes that affect runtime behavior — especially `models.json`, provider configuration, authentication, network policy, or model identifiers — should therefore be tested manually before the pull request is merged.

### Runtime verification

Test the local `pi-gcp` mixin together with the published Pi workload:

```bash
sbx run \
  docker.io/docker/sbx-kit-pi:latest \
  --kit ./kits/pi-gcp \
  --kit-arg pi-gcp.project=<your-gcp-project> \
  .
```

Inside Pi, select or invoke the models affected by the change and verify the relevant behavior.

For changes to `models.json`, this normally means verifying that:

- Pi discovers the expected provider and models.
- The intended model can be selected.
- A real prompt successfully reaches Vertex AI.
- Authentication through the sandbox credential proxy works.
- The model returns a valid response.

Test the parts affected by the change rather than relying on a fixed smoke-test script. For example, adding a new model should involve testing that model, while changing authentication or endpoint configuration should involve exercising the corresponding provider flow.

### Release flow

The expected development flow is:

`change → manual runtime verification when relevant → pull request → automated validation → merge → Release Please PR → release`

The Release Please pull request is the final release gate. Merging it creates the version tag and publishes the Sandbox Kit artifacts.

A successful CI build means the Kit is structurally valid. Runtime-sensitive changes should also be verified in a real sandbox before release.