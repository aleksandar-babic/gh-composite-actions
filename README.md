# Common GitHub Composite Actions

## GCP authentication
> Authenticates to GCP using Workload Identity
- Accepts the following arguments
    - **token_format**: Indicates whether to generate `GCP access_token`
    - **access_token_lifetime**: `access_token lifetime` in seconds
    - **create_credentials_file**: Indicates whether to create `credentials_file`
    - **GCP_PROJECT_NUMBER**: GCP `project number`
    - **GCP_PROJECT_ID**: GCP `project id`
    - **GCP_SERVICE_ACCOUNT**: GCP `service account`
    - **GCP_IDENTITY_PROVIDER_POOL**: GCP `Identity provider Pool` - From Workload Identity
    - **GCP_IDENTITY_PROVIDER**: GCP `Identity provider` - From Workload Identity

> Calling gcp-auth composite action
```yaml
name: gcp auth
on:
  pull_request:
    branches:
      - develop
jobs:
  gcp-auth:
    name: 'gcp-auth'
    runs-on: ubuntu-latest
    defaults:
      run:
        shell: bash
    steps:
       - name: "gcp-auth"
        id: auth
        uses: 'aleksandar-babic/gh-composite-actions/gcp-auth@v0.1.0'
        with:
          token_format: 'access_token'
          access_token_lifetime: 18000s
          create_credentials_file: true
          GCP_PROJECT_NUMBER: ${{ secrets.GCP_PROJECT_NUMBER }}
          GCP_PROJECT_ID: ${{ secrets.GCP_PROJECT_ID }}
          GCP_SERVICE_ACCOUNT: ${{ secrets.GCP_SERVICE_ACCOUNT }}
          GCP_IDENTITY_PROVIDER_POOL: ${{ secrets.GCP_IDENTITY_PROVIDER_POOL }}
          GCP_IDENTITY_PROVIDER: ${{ secrets.GCP_IDENTITY_PROVIDER }}

```

## pre-commit hooks
> Install all pre-commit dependencies as follows
-  pre-commit
- install checkov
- terraform-docs
- tfsec
- helm-docs

At the end runs pre-commit hooks
```shell
pre-commit run -a
```


> Calling pre-commit composite action
```yaml
name: pre-commit
on:
  pull_request:
    branches:
      - develop
jobs:
  pre-commit:
    name: 'pre-commit'
    runs-on: ubuntu-latest
    defaults:
      run:
        shell: bash
    steps:
      - name: pre-commit
        uses:  'aleksandar-babic/gh-composite-actions/pre-commit@v0.1.0'
```

## Argo Workflows
> To submit argo workflows
- Connects to GKE cluster
- Install` argo cli`
- submit argo workflow

> Accepts the following arguments
 - **argoCliVersion**: `Argo CLI version`
    -  default: `v3.3.8`
- **argoNamespace**: `Argo Workflow Namespace`
- **gkeClusterName**: `GKE Cluster Name`
- **gkeZone**: `GKE Cluster location`
- **argoWorkflowFilePath**: `Argo Workflow File path`
- **argoParameterFilePath**: `Argo Workflow Parameter File path`

> Calling argo workflows composite action
```yaml
name: submit argo workflow
on:
  pull_request:
    branches:
      - develop
jobs:
  submit-argo-workflow:
    name: 'submit-argo-workflow'
    runs-on: ubuntu-latest
    defaults:
      run:
        shell: bash
    steps:
       - name: "gcp-auth"
        id: auth
        uses: 'aleksandar-babic/gh-composite-actions/argo-workflows@v0.1.0'
        with:
          argoCliVersion: 'v3.3.8'
          argoNamespace: 'workflows'
          gkeClusterName: 'my-gke-cluster'
          gkeZone: europe-west4-a
          argoWorkflowFilePath: ./my-workflows/workflow.yaml
          argoParameterFilePath: ./my-workflows/parameters/dev.yaml
```
