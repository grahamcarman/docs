---
title: Astro API overview
sidebar_label: Overview
id: overview
---

The Astro API is a standard REST API that you can use to develop applications for interacting with Astro components.

The API contains multiple schemas, with each schema representing a different part of Astro management:

- **Platform API**: This schema includes endpoints for updating your Astro infrastructure and resources, such as clusters, Deployments, and Workspaces.
- **Identity and access management (IAM) API**: This schema includes endpoints for managing role-based access control (RBAC) components, such as users and Teams.

To make your first request using the Astro API, see [Get started with the Astro API](api/get-started.md).

:::info

The Astro API is currently in beta. See [Support](versioning-and-support.md) for all policies related to using beta releases.

:::

## Authentication

All requests to the API must be authenticated. You can use [bearer authentication](https://swagger.io/docs/specification/authentication/bearer-authentication/) to authenticate with a [Workspace API token](workspace-api-tokens.md) or [Organization API token](organization-api-tokens.md). The following example shows how you can add a token to a curl request:

```curl
GET https://api.astronomer.io/v1beta1/organizations/{organizationId}/clusters \
--header 'Authorization: Bearer <your-api-token>'
```

Endpoints can return subsets of specific attributes based on the permissions of your API token. Generally speaking, if your API token's role would allow you to access something in the Cloud UI or Astro CLI, it will also allow you to access the same thing or action using the API. See [User permissions](user-permissions.md) for a list of all possible permissions.

## Rate limiting

You can make a maximum of 10 requests per second with the same API token.

## Idempotent requests

Astro supports different levels of [idempotency](https://en.wikipedia.org/wiki/Idempotence) for different request types.

- **`CREATE` requests**: Identical `CREATE` requests will result in the creation of multiple objects. For example, if you post identical requests to create an Organization, Astro creates multiple Organizations with identical settings and unique IDs.
- **`UPDATE` requests**: Idempotency is guaranteed for all `UPDATE` requests.
- **`DELETE` requests**: Idempotency is guaranteed for all `DELETE` requests. Any successive identical `DELETE` requests return a 404 error.



