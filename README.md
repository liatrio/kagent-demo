# kagent-demo

This repository demonstrates [Kagent](https://kagent.dev/) running on a local Kubernetes cluster provisioned with k3d.

## Getting Started

### Dependencies

- [Task](https://taskfile.dev/) - Task runner
- [k3d](https://k3d.io/) - Tool to run k3s in Docker
- [helmfile](https://helmfile.readthedocs.io/en/latest/) - Declarative management of Helm charts

### Setup

1. Generate local `.env` files and populate them with the required secrets (e.g., your OpenAI API key):

   ```bash
   task generate-envs
   # Edit the generated .env files below with your actual secrets
   ```

   The following `.env.example` files are available to guide you on required environment variables:

   - `manifests/kagent/.env.example`: Contains `OPENAI_API_KEY` for the main kagent configuration.
   - `manifests/kagent/mcp/.env.example`: Contains `GH_PAT` for the GitHub MCP server and `CONTEXT7_API_KEY` for the Context7 MCP server.

2. Stand up the demo:

    ```bash
    task up
    ```

> **Note**: By default this demo uses OpenAI via the configuration in `manifests/kagent/kagent-values.yaml`. You can adjust the provider or other settings in that file; refer to the [upstream chart values](https://github.com/kagent-dev/kagent/blob/main/helm/kagent/values.yaml) for acceptable options.

## Clean Up

To clean up the cluster, run:

```bash
task down
```

## Adding Applications

- Add each application under `manifests/<app-name>` so the folder matches the application identifier (for example, `manifests/kagent`).
- Include a `kustomization.yaml` in that folder to orchestrate the application's manifests and resources.
- When installing via Helm, supply a `helmfile.yaml` for the application. If the chart ships CRDs separately (recommended), also include a `crd-helmfile.yaml` alongside it; the automation renders CRDs first, waits for them to establish, and then applies the remaining manifests.
