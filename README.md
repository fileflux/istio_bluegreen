# FileFlux Istio Blue-Green Deployment using ArgoCD

## Overview

This repository contains code that demonstrates a blue-green deployment strategy using Istio for traffic management. The Istio DestinationRule and VirtualService configurations enable traffic routing between different versions (blue and green) of the FileFlux app within the same service. The deployment uses a weighted routing strategy to direct traffic to the blue and green subsets, allowing gradual or instant cutover between different versions of the service. The deployment is managed by **ArgoCD** for continuous delivery, while the infrastructure for ArgoCD itself is provisioned using **Terraform**, located in the FileFlux infrastructure repository.

### Features
- Istio-based blue-green deployment configuration.
- Manage traffic between two versions (blue and green) of a service using Istio.
- Weighted traffic distribution to control the percentage of traffic sent to each version.
- Customizable traffic routing for safe and gradual version rollouts.
- GitOps-based continuous delivery using ArgoCD.

### Key Components:
1. **Istio**: Used for traffic management and controlling blue-green deployments.
2. **ArgoCD**: Handles GitOps-based continuous delivery.
3. **Terraform**: Manages the infrastructure for ArgoCD (located in the FileFlux infra repository).

## Prerequisites

Before deploying the solution, ensure you have the following in place:
- AWS Infrastructure setup using Terraform located [here](https://github.com/fileflux/infra)
- ArgoCD installed on the Kubernetes cluster (automatically installed and setup as part of the infra setup).
- Istio installed on the Kubernetes cluster (automatically installed and setup as part of the infra setup).
- Services and deployments labeled with the versions blue and green.

## Repository Structure

```plaintext
istio-bluegreen/             
├── destinationrule.yaml              
├── README.md               
├── virtualservice.yaml
```

## Deployment Overview

### Istio Blue-Green Deployment

Istio manages traffic routing between two different versions of your service (blue and green) based on the configurations in the `DestinationRule` and `VirtualService` files.

- **`destinationrule.yaml`**:
  Defines the subsets `blue` and `green`, representing the two versions of the service. Each subset is associated with a different version label.

- **`virtualservice.yaml`**:
  Controls how traffic is routed between the blue and green versions. Initially, 100% of the traffic goes to the blue version. This can be updated during the deployment process to shift traffic to the green version via ArgoCD.

## Running the Deployment
No manual intervention is required to run the blue-green deployment. The configuration files (`destinationrule.yaml` and `virtualservice.yaml`) are applied to the Kubernetes cluster using ArgoCD, which automatically manages the deployment process based on the GitOps workflow.

## GitHub Workflow (including Trivy)

A GitHub Actions workflow is included to run yaml validation on the Istio configuration files. This workflow ensures that the configuration files are correctly formatted and adhere to the Istio specifications.

This workflow:
1. Checks out the repository.
2. Installs yaml-lint
3. Validates the `destinationrule.yaml` and `virtualservice.yaml` files using `yaml-lint`.

## Additional Notes
- Ensure that the infrastructure is set up correctly using Terraform and that the services and deployments are labeled correctly with the versions blue and green.

- [Istio Documentation](https://istio.io/latest/docs/)
- [ArgoCD Documentation](https://argo-cd.readthedocs.io/en/stable/)
- [Terraform Documentation](https://www.terraform.io/docs/)