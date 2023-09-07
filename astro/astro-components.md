---
sidebar_label: 'Concepts'
title: 'Astro concepts and components'
id: astro-components
description: Learn about each of the components you can interact with in Astro.
---

Astro is a SaaS application that provides fully managed Airflow environments for teams of all sizes. Astro provides you a true serverless Airflow experience making it more reliable, scalable and easy to use. The Astro platform includes a few concepts and components that don't exist in open source Apache Airflow. 

Use this document to learn about key Astro concepts and how they work together on the platform.

## Astro Runtime

The _Astro Runtime_ is a [debian-based Docker image](https://quay.io/repository/astronomer/astro-runtime) that uses OSS Apache Airflow at its core and bunldes it with optimized configurations and commercial add-ons that make your Airflow experience reliable, faster, efficient and more powerful. 

Every version of Astro Runtime correlates to an OSS Apache Airflow version. Astro Runtime is the basis for running Airflow on Astro and is built and maintained by Astronomer. It is a must for your local Astro project and your Airflow environment on Astro, also called a [Deployment](#deployment).

## Cloud UI

The [_Cloud UI_](https://cloud.astronomer.io) is your entry point to use the Astro platform which provides you a serverless Airflow experience from a single pane of glass. The Cloud UI provides features like:

- Customize and Configure your Airflow environment without editing any configuration files
- Manage access and security using role-based access control (RBAC)
- Ability to manage, organize and monitor multiple Airflow environments
- Create your own dedicated and optimized infrastructure in the cloud of your choice which is managed by Astronomer
- Data Lineage visualization

## Deployment

An Astro _Deployment_ is an Airflow environment on Astro powered by [Astro Runtime](runtime-overview.md). It encompasses all core Airflow components, including the Airflow webserver, scheduler, and workers, along with additional tools for reliability and observability. It runs in an isolated Kubernetes namespace in an [Astro cluster](#cluster) and has a set of attached resources to run your Airflow tasks.

Compared to a regular Airflow environment, an Astro Deployment provides you the flexibility to fine-tune and customize your Airflow environment's settings directly from the Cloud UI, without the need to edit any configuration files. You can easily switch executors, enable or disable high availability, enforce CI/CD, adjust the scheduler size, and much more. 

Deployments also make your Airflow experience more reliable by providing your tasks with a 24-hour grace period to complete in case of any restarts, and they are scalable with auto-scaling workers based on your workload. You can also add and update environment variables to your Deployment directly using the Cloud UI.

By default, each Deployment on Astro runs:

- 1 small scheduler
- Minimum 0 workers and can scale up to 10 workers
- Each worker with a concurrency of maximum 5 tasks
- Celery Executor
- The latest OSS Airflow version using Astro Runtime version. See [Astro Runtime and Airflow version parity](runtime-image-architecture.md#astro-runtime-and-apache-airflow-parity).

To configure your Deployment and change the default settings see [Deployment settings](configure-deployment-resources.md).

To run DAGs in a Deployment, you first deploy them manually using Astro CLI from your local machine or automatically from your code repository. Then, you can open the Airflow UI from your Deployment view in the Cloud UI and run your deployed DAGs. See [Run your first DAG](create-first-dag.md) for more details.

### Cluster

An _Astro cluster_ is a Kubernetes cluster that hosts the infrastructure required to run your Airflow environment, also known as [Deployment](#deployment) in Astro. You must select a cluster to create a Deployment. You can be either choose a _Standard cluster_ or a _Dedicated cluster_ in the cloud of your choice. In both the cases, Astronomer creates and manages the clusters and other necessary infrastructure required to run your Astro Deployments. 

A Standard cluster is a multi-tenant cluster that is shared across many organizations whereas a Dedicated cluster is solely for a single organization. You can choose to use either one or both types of clusters.

You can create multiple dedicated clusters for your organization and manage them from the Astro cloud UI. For all available resources see [Hosted resource reference](resource-reference-hosted.md).

## Workspace

A _Workspace_ is a collection of Deployments that can be accessed by a specific group of users. You can use a Workspace to group Deployments that share a business use case or environment trait. Workspace is a logical concept and does not create any resources for your organization. However, you must have access to a Workspace where you want to create a Deployment.

Workspaces allow you to control access to your Deployments, by using different [Workspace roles](user-permissions.md#workspace-roles). You can also interact with Workspaces programmatically using Astro CLI.

For enhanced security, workspaces can be granted access to specific clusters to host Deployments in case of Dedicated cluster or Astro Hybrid.

## Organization

An Astro _Organization_ is the top most entity in Astro created for you when you sign up with Astro. It contains Workspaces, which are collections of Deployments, that are typically owned by a single team. Within an organization you can:

- [Invite users, grant roles, and manage permissions](manage-organization-users.md)
- [Compare DAG runs across environments](organization-metrics.md#astro-usage)
- [Monitor usage and manage Billing](manage-billing.md)
- [Manage Workspaces](manage-workspaces.md) and [Deployments](create-deployment.md)
- [Manage clusters](create-dedicated-cluster.md)

## Users and teams

When you sign up with Astro, a user is created for you along with your Organization. As the first user in your Astro Organization, Astro assigns you the role **Organization Owner** and you have the ability to invite new users to your organization and manage other users' permissions. 

Each Astro user can also be added to a _Team_. A team in Astro is a group of users in an Organization that you can grant the same Organization and Workspace permissions. 

You must invite a user to your Organization to grant permissions and add them to teams or Workspaces.

Astro's user access model also allows you to [configure SSO and authenticate using your IdP](configure-idp.md), supports [integration with SCIM](set-up-scim-provisioning.md), and allows you to [manage multiple domains](manage-domains.md). 

## See also

- [Astro CLI](cli/overview.md)
- [Create a Deployment](create-deployment.md)
- [Deployment resources](resource-reference-hosted.md)

