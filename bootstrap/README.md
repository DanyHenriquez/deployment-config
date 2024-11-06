# Cluster bootstrap

This repository contains the necessary configuration to deploy ArgoCD along with a Custom Configuration Management Plugin (CMP) for Helmfile integration, and a basic Argo application structure.

## Overview

The setup consists of three main components:

1. **ArgoCD Installation**: Deploys the core ArgoCD components
2. **Helmfile CMP Plugin**: Adds Helmfile support to ArgoCD
3. **Basic Argo Application**: Configures a base infrastructure application

## Repository Structure

```
.
├── helmfile.yaml              # Main Helmfile configuration
├── environments/             
│   ├── values.dev.yaml       # Environment-specific values
│   ├── values.stg.yaml
│   └── values.prd.yaml
└── values/
    ├── argo-cd/              # ArgoCD specific values
    │   └── values.yaml       # Core ArgoCD configuration
    └── argo-apps/            # ArgoCD Applications configuration
        └── values.yaml       # Applications definitions
```

## Components

### ArgoCD Configuration

The setup uses Helmfile to deploy ArgoCD with specific customizations:
- Disabled Dex authentication
- Disabled notifications
- Configured with a Helmfile CMP plugin

### Helmfile CMP Plugin

The Custom Configuration Management Plugin (CMP) is configured to:
- Use the official Helmfile image (`ghcr.io/helmfile/helmfile:v0.157.0`)
- Run with specific resource limits:
  - Memory: 256Mi (request) / 512Mi (limit)
  - CPU: 200m (request) / 500m (limit)
- Handle Helmfile template generation and discovery

### Argo Application

Argo applications (app of apps) are defined in values/argocd-apps/values.yaml.

More can be added as needed. One for each repository.

## Environment Support

The configuration supports three environments:
- Development (dev)
- Staging (stg)
- Production (prd)

Each environment can have its specific values defined in the `environments/` directory.

## Usage

To deploy this setup:

1. Clone this repository
2. Ensure you have kubectl access to your target cluster
3. Apply the configuration:
   ```bash
   helmfile sync
   ```

This will deploy ArgoCD and configure it to manage your infrastructure through GitOps practices.

## Prerequisites

- Helm 3.x
- Helmfile
- kubectl

## Notes

- Make sure to update the `repoURL` in the applications configuration to point to your actual Git repository
