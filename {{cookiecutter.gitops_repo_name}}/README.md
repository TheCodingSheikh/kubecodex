# Documentation

Welcome to the Kubecodex documentation! This directory contains comprehensive guides for understanding and using the Kubecodex GitOps repository structure.

## Quick Navigation

- **[Start Here](index.md)** - Complete documentation overview and quick start guide
- **[Structure Overview](structure-overview.md)** - High-level directory structure explanation
- **[Configuration Guide](config-yaml.md)** - Detailed config.yaml usage and customization

## Key Concepts

Kubecodex is a standardized GitOps repository structure that works with ArgoCD to manage Kubernetes applications across multiple clusters. It provides:

- **Organized Structure**: Clear separation between cluster-specific and essential applications
- **Automated Discovery**: ApplicationSet controllers automatically discover and manage applications
- **Flexible Configuration**: Override defaults through simple config.yaml files
- **Multi-Project Support**: Logical grouping and access control through ArgoCD projects

## Getting Started

1. Read the [Structure Overview](structure-overview.md) to understand the directory layout
2. Follow the [Quick Start Guide](index.md#quick-start) to set up your first project
3. Explore the [Configuration Guide](config-yaml.md) to customize your deployments

For detailed information about each component, see the [main documentation index](index.md).
