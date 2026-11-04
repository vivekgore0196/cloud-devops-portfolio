# Azure DevOps CI/CD to AKS

## Goal
Demonstrate a CI/CD workflow that builds a container image, pushes it to Azure Container Registry (ACR), and deploys it to Azure Kubernetes Service (AKS).

## Flow
`Azure Repos → Azure Pipelines → Docker build → ACR → AKS`

## Repository contents
- `app/` — minimal Python Flask web application
- `Dockerfile` — container image definition
- `k8s/deployment.yaml` — Kubernetes Deployment and Service
- `azure-pipelines.yml` — example Azure Pipelines workflow

## Prerequisites
- Azure DevOps project and repository
- Azure subscription
- ACR and AKS clusters
- Azure Resource Manager service connection with appropriately scoped permissions
- Kubernetes access configured for the pipeline agent
- Pipeline variables: `ACR_LOGIN_SERVER`, `ACR_REPOSITORY`, `AKS_NAMESPACE`
- Configure the service connection names in the YAML to match your Azure DevOps project.

## Important setup notes
1. Create an ACR and AKS cluster in a sandbox subscription.
2. Grant AKS permission to pull from ACR (for example, attach ACR to AKS or configure least-privilege pull access).
3. Create and authorize the Azure Resource Manager service connection.
4. Configure the Kubernetes deployment authentication according to your organization's approved method. The sample uses an Azure Resource Manager service connection with the `KubernetesManifest@1` task; verify task inputs against your agent and cluster configuration.
5. Review and adjust resource names, namespace, image tags and permissions before running.

## Run and validate
- Run the pipeline on a feature branch first.
- Confirm the image tag exists in ACR.
- Confirm the Deployment becomes Available: `kubectl rollout status deployment/demo-web -n portfolio`
- Confirm the Service and application response.
- Capture screenshots of a successful pipeline, ACR image and AKS rollout for your portfolio.

## Security
Use service connections and secret variables. Do not store passwords, tokens, kubeconfig content or service-principal secrets in source control.
