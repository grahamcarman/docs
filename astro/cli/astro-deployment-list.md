---
sidebar_label: "astro deployment list"
title: "astro deployment list"
id: astro-deployment-list
description: Reference documentation for astro deployment list.
hide_table_of_contents: true
---

List all Deployments within your current Workspace.

## Usage

```sh
astro deployment list
```

## Options

| Option           | Description                                                               | Possible Values        |
| ---------------- | ------------------------------------------------------------------------- | ---------------------- |
| `-a`,`--all`     | Show Deployments across all Workspaces that you have access to.           | None                   |
| `--workspace-id` | Specify a Workspace to list Deployments outside of your current Workspace | Any valid Workspace ID |

## Examples

```sh
$ astro deployment list --all
# Shows Deployments from all Workspaces that you're authenticated to
```

## Output

| Output               | Description                                                                                        | Data Type                                  |
| -------------------- | -------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| `NAME`               | The name of the Deployment.                                                                        | String                                     |
| `NAMESPACE`          | A unique, auto-generated label for your Deployment.                                                | String                                     |
| `CLUSTER`            | The name of the Astro cluster where the Deployment runs.                                           | String                                     |
| `DEPLOYMENT ID`      | The Deployment ID                                                                                  | String                                     |
| `RUNTIME VERSION`    | Which version of the Astro Runtime the Deployment uses, and the Airflow version it corresponds to. | String. (`X.X.X (based on Airflow X.X.X)`) |
| `DAG DEPLOY ENABLED` | Whether the deployment method is DAG only or a full image deployment.                              | Boolean                                    |

## Related Commands

- [`astro login`](cli/astro-login.md)
- [`astro deploy`](cli/astro-deploy.md)
