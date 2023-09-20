---
sidebar_label: "astro workspace team add"
title: "astro workspace team add"
id: astro-workspace-team-add
description: Reference documentation for astro workspace team add.
hide_table_of_contents: true
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

:::info
The behavior and format of this command differs depending on what Astronomer product you're using. Use the following tabs to change between product contexts.
:::

Add a Team to your current Workspace and grant it a Workspace role.

<Tabs
    defaultValue="astro"
    values={[
        {label: 'Astro', value: 'astro'},
        {label: 'Software', value: 'software'},
    ]}>
<TabItem value="astro">

## Usage

```sh
astro workspace team add <options>
```

If you want to add a team to the current Workspace with the default role of Workspace Member, you can also run `astro workspace team add`. This command lists the available Teams in your Orgranization and prompts you to enter the serial number for team you want to add.

If you want to add a team to a specific Workspace, you can use the `--workspace-id` flag. To find a Workspace ID, run `astro workspace list`.

## Options

| Option           | Description                                                                         | Valid Values                                                                                                                           |
| ---------------- | ----------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `--team-id`      | Specifies the Team ID to add to a Workspace and bypasses the Team selection prompt. | Any valid Team ID.                                                                                                                     |
| `-r`, `--role`   | The Team's role in the Workspace.                                                   | Possible values are `WORKSPACE_MEMBER`, `WORKSPACE_AUTHOR`, `WORKSPACE_OPERATOR`, or `WORKSPACE_OWNER`. Default is `WORKSPACE_MEMBER`. |
| `--workspace-id` | The Workspace ID where you want to add the Team.                                    | Any valid Workspace ID. Default is the current Workspace context you are working in.                                                  |

You can retrieve a Team's ID in one of two ways:

- To find a Team ID using the Astro CLI, run `astro organization team list`.
- To find a Team ID in the Cloud UI, click your Workspace name, then click **Organization Settings** > **Access Management** > **Teams**. Search for your Team in the **Teams** table and copy its **ID**. The ID should look something like `clk17xqgm124q01hkrgilsr49`.

</TabItem>

<TabItem value="software">

Manage Astronomer Software [Teams](https://docs.astronomer.io/software/import-idp-groups).

#### Usage

If you want to add a team to the current Workspace with the default role of Workspace Member, you can also run `astro workspace team add`. This command lists the available Teams in your Orgranization and prompts you to enter the serial number for team you want to add. 

```sh
astro workspace team <options>
```

You can retrieve a Team's ID in one of two ways:

- Access the Team in the Software UI and copy the last part of the URL in your web browser. For example, if your Team is located at `BASEDOMAIN.astronomer.io/w/cx897fds98csdcsdafasdot8g7/team/cl4iqjamcnmfgigl4852flfgulye`, your Team ID would be `cl4iqjamcnmfgigl4852flfgulye`.
- Run `astro organization team list` and copy the value in the ID column

#### Options

| Option                   | Description                                      | Possible Values                                                                                                                        |
| ------------------------ | ------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| `--team-id` (_Required_) | The Team's ID                                    | Any valid Team ID                                                                                                                      |
| `--role`                 | The Team's role in the Workspace.                | Possible values are `WORKSPACE_MEMBER`, `WORKSPACE_AUTHOR`, `WORKSPACE_OPERATOR`, or `WORKSPACE_OWNER`. Default is `WORKSPACE_MEMBER`. |

</TabItem>
</Tabs>

## Related commands

- [`astro workspace team remove`](cli/astro-workspace-team-remove.md)
- [`astro organization team create`](cli/astro-organization-team-create.md)
- [`astro workspace switch`](cli/astro-workspace-switch.md)
