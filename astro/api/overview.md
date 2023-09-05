---
title: Astro API overview
sidebar_label: Overview
id: overview
---

The Astro API is a standard REST API which you can use to develop applications for interacting with Astro components.

The API contains multiple schemas, with each schema representing a different group of Astro components:

- Platform API: This schema includes endpoints for updating your Astro infrastructure and resources, such as clusters, Deployments, and Workspaces.
- Identity and access management (IAM) API: This schema includes endpoints for managing role-based access control (RBAC) components, such as users and Teams.

To make your first request using the Astro API, see [Get started with the Astro API]

## Authentication

All requests to the API must be authenticated. You can use bearear authentication to authenticate with a Workspace API token or Organization API token. The following example  shows how you can add a token to a curl request:

```curl
GET https://api.astronomer-dev.io/v1alpha1/organizations/{organizationId}/clusters \
--header 'Authorization: Bearer <your-api-token>'
```

## Rate limiting

You can make a maximum of 10 requests per second with the same API token.

## Idempotency

