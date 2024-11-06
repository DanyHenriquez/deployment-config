# Helmfile App of Apps

This repository contains a Helmfile configuration for deploying an "app of apps" using ArgoCD. The Helmfile manages multiple ArgoCD applications, each representing a different component of your infrastructure.
The main application that manages the app of apps is in the argo-cd directory with the release name `argo-apps` in the `Helmfile.yaml`.

## Structure

The Helmfile is structured into four main sections:

1. Environments
2. Templates
3. Releases
4. charts

### Environments

The configuration supports multiple environments (dev, stg, prd) with environment-specific values. These are defined in separate YAML files in the `environments/` directory.
If an app is enabled depends on if it is in the environment specific `installed` array.

### Templates

A template named `argocd` is defined, which sets up common configurations for ArgoCD applications. This template is used as a base for all releases.

Key features of the template:
- Uses a custom ArgoCD application chart (charts/argocd-application)
- Supports environment-specific values
- Allows for release-specific overrides

### Releases

Each release in the Helmfile represents an ArgoCD application. These applications are typically other Helm charts that ArgoCD will manage.

Releases inherit from the `argocd` template, ensuring consistent configuration across all applications.

## How It Works

1. When you run Helmfile, it processes the configuration for the specified environment.
2. For each release, Helmfile applies the `argocd` template and any release-specific configurations.
3. This results in the creation of ArgoCD application resources in your Kubernetes cluster.
4. ArgoCD then manages the actual deployment and lifecycle of the applications defined by these resources.

## Usage

To use this Helmfile:

1. Ensure you have Helmfile and Helm installed.
2. Run `helmfile template -e <environment> --output-dir-template "tmp/{{ .Release.Name}}"` to inspect the configuration for a specific environment.

For example:
```
helmfile template -e dev --output-dir-template "tmp/{{ .Release.Name}}"
```

This will allow you to inspect the ArgoCD applications for the dev environment.

Remember to review and adjust the values files and release configurations as needed for your specific use case.
