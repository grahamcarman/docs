---
sidebar_label: "Automation"
title: "Automate processes to manage your Astro Deployment"
id: cli-access-automate
description: "How to automate common processes to manage your Astro Deployment using Astro CLI"
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

An Astro Deployment is part of an Astro Workspace that can be accessed by a specific group of users. You can manage a Deployment using Astronomer's [Cloud UI](log-in-to-astro.md#log-in-to-the-cloud-ui) or [Astro CLI](cli/overview.md). By default every Astro user has programmatic access to Astro Deployments using Astro CLI. This access is based on the [permissions](user-permissions.md) assigned to a user at the Organization and Worksapce level.

Though, Cloud UI is self-explanatory and easy to use, programmatic access to Deployments is required for deploying your Astro project from local system, for automated deploys using CI/CD, or to automate common processes to manage your Deployments. 

While your Astro user credentials can be used for programmatic access to your Deployments, API tokens are recommended to be used for CI/CD and other common processes for production-grade Deployments. Astronomer recommends not to give direct access to users for deploying code to your production Deployments. You can implement appropriate controls for users using Astro's heirarchical role-based access control (RBAC) for Astro Organization and Workspaces, and also enforce CI/CD deploys for your Deployments.



If you need help with choosing the right authorization for your users, reach out to [Astronomer support](astro-support.md).

## Common use-cases that you can automate

| API token scope | Use-cases                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Deployment      | - Deploy code to your Deployment [using CI/CD tools](set-up-ci-cd.md). <br /> - Update your Deployment using the Deployment file. <br /> - [Update your Deployment](cli/astro-deployment-update.md), such as changing the executor or number scheduler AUs, etc. <br /> - [Enforce CI/CD](configure-deployment-resources.md#enforce-cicd-deploys) or [enable DAG-only deploys](deploy-dags.md#enable-dag-only-deploys-on-a-deployment) for your Deployment <br /> - Fetch a short-lived access token to use [Airflow REST API](airflow-api.md) for your Deployment. This assumes the same permissions as your API key. <br /> |
| Workspace       | - Create and update Deployments in your Workspace using a Deployment file. <br /> - [Manage users, teams, and tokens](cli/astro-workspace-team.md) in your Workspace. <br /> - Create ephemeral Deployments in your Workspace using your CI/CD process. <br /> - Perform all Deployment-level actions in any Deployment in your Workspace. <br />                                                                                                                                                                                                                   |
| Organization    | - [Manage Workspaces](cli/astro-workspace-create.md), such as creating, deleting, and switching Workspaces. <br /> - [Manage users, teams, and tokens](cli/astro-organization-list.md), such as inviting new users, rotating existing tokens, etc. <br /> - [Exporting audit logs](audit-logs.md#export-audit-logs). <br /> - Perform all Workspace-level actions in any Workspace in your Organization <br />                                                                                           |


### Create ephemeral Deployments based on branch name

To ensure smooth transition of your new or changed DAGs from lower to production environment, it is imperative to have some processes or controls on how your team tests. you can enforce this easily using Astro's ability to output your Deployment configuration as code. 

For example, if you want your team to test the new or updated DAGs using the same set of configurations as in your production Deployment, follow these steps:

1. Create a [reference template file](#generate-a-deployment-template-file) for your production Deployment.
2. Check-in your Deployment template file to your GitHub or version control system.
3. As soon as a team member creates a new `dev` branch, your CI/CD process can [use the reference template file to create a new Deployment](#create-a-deployment-from-a-template-file) and push the changes.
4. When the testing is complete, and the `dev` branch is merged in the `staging` or `main` branch, you can automatically delete the Deployment on Astro as part of your CI/CD process.

This process will help you save costs by auto-creating and removing the test Deployments and also ensure your test environment is similar to your production environment.


## Generate a Deployment template file

You can use Astro CLI's [`astro deployment inspect`](/cli/astro-deployment-inspect.md) command to create a Deployment file for an existing Deployment. A deployment file is created in `yaml` or `json` format and includes all information about a Deployment. 

A Deployment template file is a special type of Deployment file which does not have the `metadata` section, and the `name` and `description` fields are replaced with placeholder values (`""`). You can then use this template file to automate the creation of new Deployments in your CI/CD process. 

To create a template file in `yaml` format, run the following command:

```bash

astro deployment inspect <deployment-name> --template > <your-deployment-template-file-name>.yaml

```

To create a template file in `json` format, run the following command:

```bash

astro deployment inspect <deployment-name> --template --output json > <your-deployment-template-file-name>.json

```


## Create a Deployment from a template file

<Tabs
    defaultValue="Hosted"
    groupId= "create-deployment"
    values={[
        {label: 'Hosted', value: 'Hosted'},
        {label: 'Hybrid', value: 'Hybrid'},
    ]}>
<TabItem value="Hosted">

You can use Astro CLI's [`astro deployment create`](cli/astro-deployment-create.md) command to create a Deployment by using an existing Deployment template file as reference. Before you do, keep the following in mind:

- Deployment names must be unique within a single Workspace. Make sure that you replace the `name` field in the file with the desired name of your new Deployment.

- Only `name` field needs to be populated in the template file to create a Deployment. Rest all fields will have the values populated based on the existing Deployment. You can also update or omit the remaining fields as required based on the [template file reference](#template-file-reference). The CLI will create the Deployment using default values for each unspecified configuration. These default values are the same default values that are used when you create a Deployment from the Cloud UI.

- When you create worker queues, the `name` and `astro_machine` fields are required. Any unspecified fields are populated with smart defaults based on the worker types available in your cluster.

- When you create environment variables, each variable must include a `key` and a `value`.

</TabItem>

<TabItem value="Hybrid">

You can use Astro CLI's [`astro deployment create`](cli/astro-deployment-create.md) command to create a Deployment by using an existing Deployment template file as reference. Before you do, keep the following in mind:

- Deployment names must be unique within a single Workspace. Make sure that you replace the `name` field in the file with the desired name of your new Deployment.

- Only `name` and `cluster_name` fields are required to create a Deployment. Rest all fields will have the values populated based on the existing Deployment. You can also modify or omit the remaining fields as required based on the [Deployment file reference](#deployment-file-reference). The CLI will create the Deployment using default values for each unspecified configuration. These default values are the same default values that are used when you create a Deployment from the Cloud UI.

- When you create worker queues, the `name` and `worker_type` fields are required. Any unspecified fields are populated with smart defaults based on the worker types available in your cluster.

- When you create environment variables, each variable must include a `key` and a `value`.

</TabItem>
</Tabs>

To create a new Deployment from an existing template file:

1. In your template file, provide a name for the new Deployment.
2. Run:

    ```bash

    astro deployment create --deployment-file <deployment-template-file-name>

    ```

3. (Optional) Confirm that your Deployment was successfully created by running the following command in your current Workspace:

   ```bash

   astro deployment list

   ```
   
   You can also go to the Workspace page in the Cloud UI.

4. (Optional) Reconfigure any Airflow connections or variables from the Deployment that you copied into the template file. Airflow connections and variables cannot be configured using template files. See [Manage connections in Airflow](https://docs.astronomer.io/learn/connections).


## Update a Deployment using a Deployment file

:::warning 

You must push a complete Deployment file that lists all valid configurations whenever you update a Deployment with a Deployment file. If a configuration exists on Astro but doesn't exist in your Deployment file, such as a worker queue, that configuration is deleted when you push your Deployment file. 

:::

A Deployment file is a complete snapshot of an existing Deployment at the point you inspected it. It's similar to a template file, but also contains your Deployment's name, description, and metadata. In the same way you use a template file to create a new Deployment, you use a Deployment file to update an existing Deployment with a new set of configurations.

You can create a Deployment file by running the following command:

```bash

astro deployment inspect <deployment-name> --template > <your-deployment-template-file-name>.yaml

```

When you update a Deployment with a Deployment file, keep in mind that:

- You can’t change the cluster or Workspace the Deployment runs on. To transfer a Deployment to a different Workspace, see [Transfer a Deployment](configure-deployment-resources.md#transfer-a-deployment-to-another-workspace).
- You can't change the Astro Runtime version of the Deployment. To upgrade Astro Runtime, you must update the Dockerfile in your Astro project. See [Upgrade Astro Runtime](upgrade-runtime.md).
- Environment variables marked as secret in the Cloud UI are not exported to your Deployment file. Hence, these will be deleted from your Deployment if you do not add them to your deployment file. See [`deployment.environment_variables`](#deploymentenvironment_variables) for more details.

To update a Deployment using a Deployment file:

1. Inspect an existing Deployment and create a Deployment file for its current configurations:

  ```bash
  
  astro deployment inspect -n <deployment-name> > <your-deployment-file-name>

  ```

2. Modify the Deployment file and save your changes. See [Deployment file reference](#deployment-file-reference) for fields that you can modify.

  <!-- You can modify any value in the `environment_variables` and `worker_queues` sections, and most values in the `configuration` section. -->

3. Update your Deployment according to the configurations in the Deployment file:

  ```bash

  astro deployment update <deployment-name> --deployment-file <your-deployment-file>

  ```

4. (Optional) Confirm that your Deployment was updated successfully by running the following command. You can also go to the Deployment page in the Cloud UI to confirm the new values.

  ```bash
  
  astro deployment inspect -n <deployment-name>

  ```

## Example use-cases

This section prvides examples in detail that you can implement using Astro CLI, your version-control system (VCS) and your CI/CD process. In these examples, we are using GitHub as the VCS and GitHub Actions as the CI/CD process.

### Create ephemeral Deployments based on branch name

To ensure smooth transition of your new or changed DAGs from lower to production environment, it is imperative to have some processes or controls on how your team tests. you can enforce this easily using Astro's ability to output your Deployment configuration as code. 

For example, if you want your team to test the new or updated DAGs using the same set of configurations as in your production Deployment, follow these steps:

1. Create a [reference template file](#generate-a-deployment-template-file) for your production Deployment.
2. Check-in your Deployment template file to your GitHub or version control system.
3. As soon as a team member creates a new `dev` branch, your CI/CD process can [use the reference template file to create a new Deployment](#create-a-deployment-from-a-template-file) and [deploy your code](astro/deploy-code.md).
4. When the testing is complete, and the `dev` branch is merged in the `staging` or `main` branch, you can automatically [delete the Deployment](cli/astro-deployment-delete.md) on Astro as part of your CI/CD process.

This process will help you save costs by auto-creating and removing the test Deployments and also ensure your test environment is similar to your production environment.

### Smart deploys based on your code changes

Automation of your CI/CD process depends on how your organization manages your Airflow DAGs in a version-control system, such as using a single repository for all your Airflow DAGs, using multiple repostitories for different teams, and how it segregates various Airflow environments.

Let's consider a scenario:

- You have a mono-repo and two Airflow environments, one for staging and one for production. 
- All team members use `dev` prefixed branches to build their DAGs and test locally.

To auto-detect if you need DAG-based deploy or image-based deploy to staging, follow these steps:

1. Create a GitHub action to copy all DAGs from the `dags` directory to a `astro_deploy` branch. This should get triggered when a Pull Request (PR) is created to `staging` branch from a `dev` prefixed branch.
2. Create a GitHub action to check if only the files in `dags` directory have changed or other files have also changed. This should get triggered when changes are pushed to `astro deploy` branch. When changes are detected only in `dags` directory, then trigger DAG-based deploy. Otherwise, if there are changes to files outside the `dags` directory, then trigger an image-based deploy.
3. Create a GitHub action to deploy changes from `astro_deploy` branch to `main` branch. This should get triggered when a PR is created to `main` from `astro_deploy`. This Github action will do an image-based deploy to your production environment.


## Deployment file reference

When you inspect a Deployment to generate a Deployment file, its current configuration is generated. This configuration is also available in the Cloud UI. It incldues the the following sections:

- `environment_variables`
- `configuration`
- `worker_queues`
- `alert_emails`
- `metadata`

A Deployment template file does not have the `metadata` section. You can remove the `metadata` section when using a deployment file as a template to create new Deployment.

<Tabs
    defaultValue="Hosted"
    groupId= "deployment-file-reference"
    values={[
        {label: 'Hosted', value: 'Hosted'},
        {label: 'Hybrid', value: 'Hybrid'},
    ]}>
<TabItem value="Hosted">

```yaml
deployment:
    configuration:
        name: test
        description: ""
        runtime_version: 8.7.0
        dag_deploy_enabled: false
        ci_cd_enforcement: false
        scheduler_size: small
        is_high_availability: false
        executor: CeleryExecutor
        scheduler_au: 10
        scheduler_count: 1
        cluster_name: us-central1
        workspace_name: least-permission
        deployment_type: HOSTED_SHARED
        cloud_provider: gcp
        region: us-central1
    worker_queues:
        - name: default
          max_worker_count: 10
          min_worker_count: 0
          worker_concurrency: 5
          worker_type: a5
    metadata:
        deployment_id: clkcbz5d01458926ewbjzubt3fx
        workspace_id: clk7zoqbf00f901hka4c66q2d
        cluster_id: us-central1
        release_name: N/A
        airflow_version: 2.6.3
        current_tag: 8.7.0
        status: CREATING
        created_at: 2023-07-21T08:40:02.531Z
        updated_at: 2023-07-21T08:40:02.532Z
        deployment_url: cloud.astronomer.io/clk7zoqbf00f901hka4c66q2d/deployments/clkcbz5d01458926ewbjzubt3fx/analytics
        webserver_url: org.astronomer.run/dzubt3fx
        workload_identity: astro-native-magnify-8566@proj.iam.gserviceaccount.com
```
</TabItem>

<TabItem value="Hybrid">

```yaml
deployment:
    configuration:
        name: dev
        description: Dev Deployment
        runtime_version: 8.7.0
        dag_deploy_enabled: false
        ci_cd_enforcement: false
        scheduler_size: ""
        is_high_availability: false
        executor: CeleryExecutor
        scheduler_au: 5
        scheduler_count: 1
        cluster_name: GCP Cluster
        workspace_name: Development
        deployment_type: HYBRID
        cloud_provider: gcp
        region: us-central1
    worker_queues:
        - name: default
          max_worker_count: 10
          min_worker_count: 0
          worker_concurrency: 16
          worker_type: e2-standard-4
    metadata:
        deployment_id: clkccocbd1801846ewb7i5apkgx
        workspace_id: cku7t3fvx59046554xr4g0siv7r
        cluster_id: cl3sm2lb500kh0synhlc7bh14
        release_name: flickering-protogalaxy-2555
        airflow_version: 2.6.3
        current_tag: 8.7.0
        status: HEALTHY
        created_at: 2023-07-21T08:59:37.945Z
        updated_at: 2023-07-21T08:59:37.945Z
        deployment_url: cloud.astronomer.io/cku7t3fvx59046554xr4g0siv7r/deployments/clkccocbd1801846ewb7i5apkgx/analytics
        webserver_url: astronomer.astronomer.run/di5apkgx
        workload_identity: astro-flickering-protogalaxy-2@proj.iam.gserviceaccount.com
```
</TabItem>
</Tabs>

:::info Alternative Astro Hybrid values

Astro Hybrid deployment files have the following differences compared to Astro Hosted:

- `scheduler_au` replaces `scheduler_size`.
- `deployment_type`, `cloud_provider`, and `region` don't exist.
- `is_high_availability` doesn't exist.

:::

### `deployment.environment_variables`

You can create, update, or delete environment variables in the `environment_variables` section of the template file. This is equivalent to configuring environment variables in the **Variables** page of a Deployment in the Cloud UI.

When you inspect a Deployment, the value of any environment variable that is set as secret in the Cloud UI will not appear in the template file. To set any new or existing environment variables as secret in the file, specify `is_secret: true` next to the key and value. If you commit a template file to a GitHub repository, Astronomer recommends that you update the secret values manually in the Cloud UI and leave them blank in the file. This ensures that you do not commit secret values to a version control tool in plain-text.

### `deployment.configuration`

The `configuration` section contains all basic settings that you can configure from the Deployment **Details** page in the Cloud UI. See:

- [Create a Deployment](create-deployment.md#create-a-deployment).
- [Update a Deployment name and description](configure-deployment-resources.md#update-a-deployment-name-and-description).
- [Scheduler resources](configure-deployment-resources.md#scheduler-resources).

### `deployment.worker_queues`

The `worker_queues` section defines the [worker queues](configure-worker-queues.md) for a Deployment. If you don't enter specific values for the `default` worker queue for a Deployment with CeleryExecutor, default values based on the worker types available on your cluster are applied. This section is not applicable to Deployments running KubernetesExecutor. 

## See also

- [Manage Deployment API keys](api-keys.md)
- [Deploy Code](deploy-code.md)
- [Choose a CI/CD Strategy for deploying code to Astro](set-up-ci-cd.md)


