---
title: Get started with the Astro API
sidebar_label: Get started
id: get-started
---

In this quickstart tutorial, you'll make some simple requests to retrieve and modify values for a Workspace API token.

## Prerequisites

- An [Astro account](log-in-to-astro.md). 
- A Organization API token with the Organization Owner role. Astronomer recommends creating a new API token for this tutorial. 
- A method for making API requests. This tutorial assumes you're using curl, but you can also use tools such as Postman.

## Step 1: Make your first API request

To access most endpoints, you need to provide an Organization ID to the API as a path parameter. One of the few requests that doesn't require an Organization ID is the [List Organizations](https://docs.astronomer.io/astro/api/platform-api-reference#tag/Organization/operation/ListOrganizations) request, which means you can programmatically retrieve an Organization ID for other API request types. 

To retrieve the Organization ID through the API, run the following command:

```bash
curl https://api.astronomer-dev.io/v1alpha1/organizations \
--H 'Authorization: Bearer <your-api-token>' \
```

If the command was successful, then you receive a response that begins similarly to the following:

```json {16}
{
  "limit": 10,
  "offset": 0,
  "organizations": [
    {
      "billingEmail": "billing@company.com",
      "createdAt": "2022-11-22T04:37:12T",
      "createdBySubject": {
        "apiTokenName": "my-token",
        "avatarUrl": "https://avatar.url",
        "fullName": "Jane Doe",
        "id": "clm8qv74h000008mlf08scq7k",
        "subjectType": "USER",
        "username": "user1@company.com"
      },
      "id": "clmaxoarx000008l2c5ayb9pt",
      "isScimEnabled": false,
...
      ],
```

Copy the top-level `id` from this response. This is your Organization ID.

While you could have retrieved this value manually from the Cloud UI, using the API lets you script this workflow and execute it on a regular basis.

## Step 2: Request more information from the API

Using the Organization ID you copied, send the following request.

```bash
curl https://api.astronomer-dev.io/v1beta1/organizations/<your-organization-id>/workspaces \
--H 'Authorization: Bearer <your-api-token>' \
```

If the command succeeds, then you receive a response similar to the following:

```json
{
  "limit": 0,
  "offset": 0,
  "totalCount": 0,
  "workspaces": [
    {
      "apiKeyOnlyDeploymentsDefault": true,
      "createdAt": "2019-08-24T14:15:22Z",
      "createdBy": {
        "apiTokenName": "string",
        "avatarUrl": "string",
        "fullName": "string",
        "id": "string",
        "subjectType": "USER",
        "username": "string"
      },
      "deploymentCount": 0,
      "description": "string",
      "id": "string",
      "name": "string",
      "orgShortName": "string",
      "organizationId": "string",
      "organizationName": "string",
      "organizationShortName": "string",
      "serverlessRuntimeCount": 0,
      "updatedAt": "2019-08-24T14:15:22Z",
      "updatedBy": {
        "apiTokenName": "string",
        "avatarUrl": "string",
        "fullName": "string",
        "id": "string",
        "subjectType": "USER",
        "username": "string"
      },
      "userCount": 0
    }
  ]
}
```

Copy the `workspace.id` for the next step. 

:::tip

If the API returned many Workspaces, try sending the following request instead to limit your search: 

```bash
curl https://api.astronomer-dev.io/v1beta1/organizations/<your-organization-id>/workspaces?search="<your-workspace-name>" \
--H 'Authorization: Bearer <your-api-token>' \
```

Query parameters like `search` are useful for limiting the results that the API returns for `GET` requests.

:::

## Step 3: Update your token description using the API

Now that you have both an Organization ID and a Workspace ID, you can update your API token description and check your changes in the Cloud UI. 

1. Run the following command to retrieve metadata for your Workspace API token:

    ```bash
    curl https://api.astronomer-dev.io/v1beta1/organizations/<your-organization-id>/workspaces/<your-workspace-id>/api-tokens \
    --H 'Authorization: Bearer <your-api-token>' \
    ```

2. Copy the `id` of your token from the API's response.
3. Run the following command to update the description of your Workspace API token:

    ```bash
    curl -X POST https://api.astronomer-dev.io/v1alpha1/organizations/<your-organization-id>/workspaces/<your-workspace-id>/api-tokens/<your-token-id> \
    --H 'Authorization: Bearer <your-api-token>' \
    -D Copy { "description": "I updated this description using the Astro API!", "name": "<your-api-token-name>", "role": "WORKSPACE_OWNER" }
    ```

    If the request was successful, the API will return a response with all of your token's details, including the updated description.

4. In the Cloud UI, go to **Workspace Settings** > **Access Management** > **API Tokens** and find your Workspace API token. You should see your updated description under **Description**.

## Next steps

Get familiar with the Astro API using the API reference guides:

- [Platform API reference](api/platform-api-reference.mdx)
- [IAM API reference](api/iam-api-reference.mdx)