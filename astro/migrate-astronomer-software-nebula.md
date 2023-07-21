---
title: 'Migrate from Astronomer Software or Nebula to Astro'
sidebar_label: 'From Astronomer Software/ Nebula'
id: migrate-astronomer-software-nebula
---

Astro includes most of the same features that are available in Astronomer Software and Astronomer Nebula and Software. After Astronomer completes your migration, use this document to familiarize yourself with Astro and relocate key workflows and features from other Astronomer products. 

## Prerequisites 

- You have finished migrated your Airflow Deployments to Astro, or you're in the process of migrating. 
- You no longer need to access your Nebula Deployments from the Astro CLI.

## Step 1: Upgrade and configure the Astro CLI

To access Astro features from the command line, all of your team members should be using the latest version of the Astro CLI.

1. Install the latest version of the Astro CLI. See [Install the CLI](https://docs.astronomer.io/astro/cli/install-cli#install-the-cli) and [Upgrade the CLI](https://docs.astronomer.io/astro/cli/install-cli#install-the-cli).
   
2. Run the following command to delete the Nebula login context:

    ```sh
    astro context delete app.gcp0001.us-east4.astronomer.io
    ```

3. Run the following command to log in to Astro:
   
    ```sh
    astro login cloud.astronomer.io
    ```

    Log in with your new Astro user credentials. 

:::tip

The Astro CLI includes several new features for managing Deployments that were not available on Astronomer Nebula. If you have time, review the [CLI reference guide](https://docs.astronomer.io/astro/cli/reference) to see all available commands.

:::

## Step 2: Reestablish workflows in Astro

Most Astronomer Nebula and Software features are available in Astro, but they might be renamed or not work in the exact same way. 

### Key similarities and differences

- You can still configure CI/CD for Astro Deployments. However, your CI/CD scripts have to be updated to use Astro CLI commands instead of manual image pushes.
- You can now have the option of deploying either a Docker image or just your DAGs folder to a Deployment. 
- You can no longer use the Houston API to manage your platform. However, the Astro CLI now supports a number of workflows for programmatically updating Deployments, Workspaces, and user permissions. 
- Just like on Astronomer Nebula and Software, Deployments on Astro are organized into Workspaces. Workspaces are now additionally grouped in an Organization, which is where you can manage users, authentication, and settings for all Workspaces. Users and API tokens can have Organization-level permissions, which are similar to "System Admin" permissions in Astronomer Software. 

### Related documentation

Review the following table to see the equivalent Astro documentation for each feature you might have used in Astronomer Software and Nebula. 

| Nebula/ Software feature                   | Astro documentation                                                                                                                                                                                                                  |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Configure a Deployment                | [Create a Deployment](https://docs.astronomer.io/astro/create-deployment) / [Configure a Deployment](https://docs.astronomer.io/astro/configure-deployment-resources)                                                                |
| Make Requests to the Airflow API      | [Make Requests to the Airflow API (Astro)](https://docs.astronomer.io/astro/airflow-api)                                                                                                                                             |
| Use the Houston API                   | [Astro CLI command reference](https://docs.astronomer.io/astro/cli/reference) /  [Manage Deployments as code](https://docs.astronomer.io/astro/manage-deployments-as-code)                                                           |
| Configure a secrets backend           | [Configure a secrets backend  (Astro)](https://docs.astronomer.io/astro/secrets-backend)                                                                                                                                             |
| Run the KubernetesPodOperator         | [Run the KubernetesPodOperator on Astro](https://docs.astronomer.io/astro/kubernetespodoperator)                                                                                                                                     |
| Run the Kubernetes Executor           | [Run the Kubernetes executor on Astro](https://docs.astronomer.io/astro/kubernetes-executor)                                                                                                                                         |
| Manage User Permissions               | [Manage Astro users](https://docs.astronomer.io/astro/add-user)                                                                                                                                                                      |
| Access the Airflow database           | [Programmatically accessing the Airflow metadata database](https://docs.astronomer.io/learn/airflow-database)                                                                                                                        |
| Deploy DAGS via CLI                   | [Deploy code](https://docs.astronomer.io/astro/deploy-code)                                                                                                                                                                          |
| Manage Airflow versions               | [Upgrade Astro Runtime](https://docs.astronomer.io/astro/upgrade-runtime)                                                                                                                                                            |
| Configure environment variables       | [Set environment variables on Astro](https://docs.astronomer.io/astro/environment-variables)                                                                                                                                         |
| Configure CI/CD                       | [Develop a CI/CD workflow for deploying code to Astro](https://docs.astronomer.io/astro/set-up-ci-cd)                                                                                                                                |
| Create a Service Account              | [Deployment API Keys](https://docs.astronomer.io/astro/api-keys)/ [Workspace API tokens](https://docs.astronomer.io/astro/workspace-api-tokens), [Organization API tokens](https://docs.astronomer.io/astro/organization-api-tokens) |
| View Deployment logs                  | [View Deployment logs](https://docs.astronomer.io/astro/view-logs)                                                                                                                                                                   |
| Airflow alerts                        | [Astro alerts](https://docs.astronomer.io/astro/alerts)/ [Airflow email notifications](https://docs.astronomer.io/astro/airflow-email-notifications)                                                                                 |
| Import users as Teams (Software only) | [Manage Teams](https://docs.astronomer.io/astro/manage-teams)                                                                                                                                                                        |

### Renamed components

Additionally, review the following table to see how key component names have been updated from Nebula to Astronomer. If a component is not listed, it has the same name in both Nebula and Astro. Note that these renamed components include functionality changes in addition to name changes.

| Nebula/ Software component name | Astronomer component name |
| ------------------------------- | ------------------------- |
| Astronomer UI                   | Cloud UI                  |
| Service account                 | API key/ token            |
| Extra Capacity                  | Resource quotas           |

### Key new features

Astro also includes a number of new features that accompany the traditional workflows you're used to. After you have reestablished your Software/ Nebula workflows, see the following documentation to learn more about these features.

- [Data lineage integrations](https://docs.astronomer.io/astro/data-lineage)
- [Worker queues](https://docs.astronomer.io/astro/configure-worker-queues)
- [The Astro Cloud IDE](https://docs.astronomer.io/astro/cloud-ide)
- [Kubernetes Pod resource configurations in the Cloud UI](https://docs.astronomer.io/astro/configure-deployment-resources#configure-kubernetes-pod-resources)

## Step 3: Reconfigure CI/CD pipelines 

Existing Astronomer Nebula CI/CD pipelines need to be rewritten to work with Astro. Astronomer recommends rebuilding your CI/CD pipelines from scratch. For each of your new Deployments:

1. Determine how you want to deploy code from your pipeline. Astro supports image deploys, DAG-only deploys, and combinations of both through CI/CD. See [Develop a CI/CD workflow](https://docs.astronomer.io/astro/set-up-ci-cd).
2. Create a credential that your new CI/CD pipeline can use to access your Deployment. Use the following table to see how Astro credentials correlate to Nebula/ Software credentials.

  | Nebula/ Software credential       | Astro credential                               |
  | --------------------------------- | ---------------------------------------------- |
  | Deployment-level service account. | Deployment API key or Workspace API token.     |
  | Workspace-level service account.  | Workspace API token or Organization API token. |

  :::caution
  
  Deployment API keys will soon be deprecated in favor of Deployment API tokens, which is an upcoming Astro feature. If you have strict Deployment-level security requirements, you can continue to use Deployment API keys, but you will have to complete a one-time migration to Deployment API tokens in the future. Otherwise, Astronomer recommends using either [Workspace API tokens](workspace-api-tokens.md) or [Organization API tokens](organization-api-tokens.md) in place of Deployment API keys.
  
  :::

3. Delete your existing CI/CD pipeline that deployed your project's code to Astronomer Software or Nebula.
4. Create and enable a new CI/CD pipeline based on the templates provided for your version management tool. See [CI/CD templates](https://docs.astronomer.io/astro/ci-cd-templates/template-overview).

## Additional resources

- [(Video) Welcome to Astro!](https://www.youtube.com/watch?v=l48yg1ELARg)