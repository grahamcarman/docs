---
sidebar_label: "astro workspace team add"
title: "astro workspace team add"
id: astro-workspace-team-add
description: Reference documentation for astro workspace team add.
hide_table_of_contents: true
---

Add a Team to your current Workspace and grant it a Workspace role.

## Usage

```sh
astro workspace team add <options>
```

To find a Team ID using the Astro CLI, run `astro organization team list`.

To find a Team ID in the Cloud UI, click your Workspace name, then click **Organization Settings** > **Access Management** > **Teams**. Search for your Team in the **Teams** table and copy its **ID**. The ID should look something like `clk17xqgm124q01hkrgilsr49`.

To find a Workspace ID using the Astro CLI, run `astro workspace list`.

If you want to add a team to the current Workspace with the default role of Workspace Member, you can also run `astro workspace team add`. This command lists the available Teams in your Orgranization and prompts you to enter the serial number for team you want to add.

## Options

| Option           | Description                                      | Valid Values                                                                                                                           |
| ---------------- | ------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| <team-id>             | Specifies the Team ID to add to a Workspace and bypasses the Team selection prompt | Any valid Team ID |
| `-r`, `--role`         | The Team's role in the Workspace.                | Possible values are `WORKSPACE_MEMBER`, `WORKSPACE_AUTHOR`, `WORKSPACE_OPERATOR`, or `WORKSPACE_OWNER`. Default is `WORKSPACE_MEMBER`. |
| `--workspace-id` | The Workspace ID where you want to add the Team. | Any valid Workspace ID. Default is the current Workspace context you are working in.                                                   |

## Related commands

- [`astro workspace team remove`](cli/astro-workspace-team-remove.md)
- [`astro organization team create`](cli/astro-organization-team-create.md)
- [`astro workspace switch`](cli/astro-workspace-switch.md)
