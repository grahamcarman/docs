---
sidebar_label: "astro workspace team"
title: "astro workspace team"
id: astro-workspace-team
description: Reference documentation for astro workspace team.
hide_table_of_contents: true
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

:::info

The behavior and format of this command differs depending on what Astronomer product you're using. Use the following tabs to change between product contexts.

:::

<Tabs
defaultValue="astro"
values={[
{label: 'Astro', value: 'astro'},
{label: 'Software', value: 'software'},
]}>
<TabItem value="astro">

Manage Astro [Teams](https://docs.astronomer.io/astro/manage-teams).

## Usage

This command includes several subcommands.

### astro workspace team add

Add a Team to a Workspace.

#### Usage

```sh
astro workspace team add --team-id=<team-id> --role=<workspace_level_role> --workspace-id=<workspace-id>
```

You can retrieve a Team's ID in one of two ways:

- Access the Team in the Cloud UI and copy the last part of the URL in your web browser. For example, if your Team is located at `BASEDOMAIN.astronomer.io/w/cx897fds98csdcsdafasdot8g7/team/cl4iqjamcnmfgigl4852flfgulye`, your Team ID would be `cl4iqjamcnmfgigl4852flfgulye`.
- Run [`astro workspace team list`](#astro-workspace-team-list) and copy the value in the `ID` column.

#### Options

| Option           | Description                                                                         | Possible Values                                                                                                                        |
| ---------------- | ----------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `--team-id`      | Specifies the Team ID to add to a Workspace and bypasses the Team selection prompt. | Any valid Team ID                                                                                                                      |
| `--role`         | The Team's role in the Workspace.                                                   | Possible values are `WORKSPACE_MEMBER`, `WORKSPACE_AUTHOR`, `WORKSPACE_OPERATOR`, or `WORKSPACE_OWNER`. Default is `WORKSPACE_MEMBER`. |
| `--workspace-id` | The Workspace ID where you want to add the Team.                                    | Any valid Workspace ID. Default is the current Workspace context you are working in.                                                   |

### astro workspace team list

View a list of all Teams in a Workspace.

#### Usage

```sh
astro workspace team list
```

#### astro workspace team remove

Update a Team from a given Workspace.

#### Usage

```sh
astro workspace team remove <team-id> --workspace-id <workspace-id>
```

You can retrieve a Team's ID in one of two ways:

- Access the Team in the Software UI and copy the last part of the URL in your web browser. For example, if your Team is located at `BASEDOMAIN.astronomer.io/w/cx897fds98csdcsdafasdot8g7/team/cl4iqjamcnmfgigl4852flfgulye`, your Team ID would be `cl4iqjamcnmfgigl4852flfgulye`.
- Run [`astro workspace team list`](#astro-workspace-team-list) and copy the value in the `ID` column.

#### Options

| Option                        | Description                | Possible Values        |
| ----------------------------- | -------------------------- | ---------------------- |
| `--workspace-id` (_Required_) | The Workspace for the Team | Any valid Workspace ID |

### astro workspace team update

Update a Team's permissions in a given Workspace.

#### Usage

```sh
astro workspace team update <team-id> --workspace-id <workspace-id> --role=<system-role>
```

You can retrieve a Team's ID in one of two ways:

- Access the Team in the Software UI and copy the last part of the URL in your web browser. For example, if your Team is located at `BASEDOMAIN.astronomer.io/w/cx897fds98csdcsdafasdot8g7/team/cl4iqjamcnmfgigl4852flfgulye`, your Team ID would be `cl4iqjamcnmfgigl4852flfgulye`.
- Run [`astro workspace team list`](#astro-workspace-team-list) and copy the value in the `ID` column.

#### Related documentation

- [Import identity provider groups into Astronomer Software](https://docs.astronomer.io/software/import-idp-groups).

#### Options

| Option                        | Description                       | Possible Values                                                                   |
| ----------------------------- | --------------------------------- | --------------------------------------------------------------------------------- |
| `--workspace-id` (_Required_) | The Workspace for the Team        | Any valid Workspace ID                                                            |
| `<team-id>` (_Required_)      | The Team's ID                     | None                                                                              |
| `--role`                      | The Team's role in the Workspace. | Possible values are `WORKSPACE_VIEWER`, `WORKSPACE_EDITOR`, or `WORKSPACE_ADMIN`. |

</TabItem>
<TabItem value="software">

Manage Astronomer Software [Teams](https://docs.astronomer.io/software/import-idp-groups).

## Usage

This command includes several subcommands.

### astro workspace team add

Add a Team to a Workspace.

#### Usage

```sh
astro workspace team add --team-id=<team-id> --role=<workspace_level_role> --workspace-id=<workspace-id>
```

You can retrieve a Team's ID in one of two ways:

- Access the Team in the Software UI and copy the last part of the URL in your web browser. For example, if your Team is located at `BASEDOMAIN.astronomer.io/w/cx897fds98csdcsdafasdot8g7/team/cl4iqjamcnmfgigl4852flfgulye`, your Team ID would be `cl4iqjamcnmfgigl4852flfgulye`.
- Run [`astro workspace team list`](#astro-workspace-team-list) and copy the value in the `ID` column.

#### Options

| Option                   | Description                                      | Possible Values                                                                                                                        |
| ------------------------ | ------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| `--team-id` (_Required_) | The Team's ID                                    | Any valid Team ID                                                                                                                      |
| `--role`                 | The Team's role in the Workspace.                | Possible values are `WORKSPACE_MEMBER`, `WORKSPACE_AUTHOR`, `WORKSPACE_OPERATOR`, or `WORKSPACE_OWNER`. Default is `WORKSPACE_MEMBER`. |
| `--workspace-id`         | The Workspace ID where you want to add the Team. | Any valid Workspace ID. Default is the current Workspace context you are working in.                                                   |

### astro workspace team list

View a list of all Teams in a Workspace.

#### Usage

```sh
astro workspace team list
```

#### astro workspace team remove

Update a Team from a given Workspace.

#### Usage

```sh
astro workspace team remove <team-id> --workspace-id <workspace-id>
```

You can retrieve a Team's ID in one of two ways:

- Access the Team in the Software UI and copy the last part of the URL in your web browser. For example, if your Team is located at `BASEDOMAIN.astronomer.io/w/cx897fds98csdcsdafasdot8g7/team/cl4iqjamcnmfgigl4852flfgulye`, your Team ID would be `cl4iqjamcnmfgigl4852flfgulye`.
- Run [`astro workspace team list`](#astro-workspace-team-list) and copy the value in the `ID` column.

#### Options

| Option                        | Description                | Possible Values        |
| ----------------------------- | -------------------------- | ---------------------- |
| `--workspace-id` (_Required_) | The Workspace for the Team | Any valid Workspace ID |

### astro workspace team update

Update a Team's permissions in a given Workspace.

#### Usage

```sh
astro workspace team update <team-id> --workspace-id <workspace-id> --role=<system-role>
```

You can retrieve a Team's ID in one of two ways:

- Access the Team in the Software UI and copy the last part of the URL in your web browser. For example, if your Team is located at `BASEDOMAIN.astronomer.io/w/cx897fds98csdcsdafasdot8g7/team/cl4iqjamcnmfgigl4852flfgulye`, your Team ID would be `cl4iqjamcnmfgigl4852flfgulye`.
- Run [`astro workspace team list`](#astro-workspace-team-list) and copy the value in the `ID` column.

#### Related documentation

- [Import identity provider groups into Astronomer Software](https://docs.astronomer.io/software/import-idp-groups).

#### Options

| Option                        | Description                       | Possible Values                                                                   |
| ----------------------------- | --------------------------------- | --------------------------------------------------------------------------------- |
| `--workspace-id` (_Required_) | The Workspace for the Team        | Any valid Workspace ID                                                            |
| `<team-id>` (_Required_)      | The Team's ID                     | None                                                                              |
| `--role`                      | The Team's role in the Workspace. | Possible values are `WORKSPACE_VIEWER`, `WORKSPACE_EDITOR`, or `WORKSPACE_ADMIN`. |

</TabItem>
</Tabs>
