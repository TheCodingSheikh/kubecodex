# Kubecodex Documentation

A GitOps repository structure standard using ArgoCD.

## Overview

Kubecodex provides a standardized structure for managing GitOps repositories with ArgoCD. This documentation covers the directory structure, configuration options, and usage patterns.

## Directory Structure

- [apps/](apps.md) — Cluster and project-specific applications
- [essentials/](essentials.md) — Essential applications deployed to all clusters
- [projects/](projects.md) — Argo CD AppProject definitions (CLI or Helm chart approach)
- [bootstrap/](bootstrap.md) — Bootstrap manifests for Argo CD and ApplicationSet resources
- [cluster-resources/](cluster-resources.md) — Cluster-wide resources

## Project Management Approaches

Kubecodex supports two approaches for project management:
- **CLI Approach**: Direct YAML management with `./kubecodex project <name>`
- **Helm Chart Approach**: Centralized management through Helm values

See [Projects Directory](projects.md) for detailed information about both approaches.

## Quick Start

1. [Set up projects](projects.md) using either CLI or Helm chart approach
2. [Configure applications](apps.md) in the `apps/` directory
3. [Add essential applications](essentials.md) that should be deployed to all clusters
4. [Manage cluster resources](cluster-resources.md) for cluster-wide configurations

## Configuration

Learn about [config.yaml usage](config-yaml.md) and how to customize application deployments.

## Table of Contents

- [Structure Overview](structure-overview.md)
- [Bootstrap Directory](bootstrap.md)
- [Projects Directory](projects.md)
- [Applications Directory](apps.md)
- [Essentials Directory](essentials.md)
- [Cluster Resources Directory](cluster-resources.md)
- [Config.yaml Usage](config-yaml.md)
