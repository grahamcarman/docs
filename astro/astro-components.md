---
sidebar_label: 'Key concepts'
title: 'Astro concepts and components'
id: astro-components
description: Learn about the components of Astro
---

Astro is a SaaS application that provides fully managed Apache Airflow environments for teams of all sizes. You have the option to choose a standard or a dedicated cluster for your Deployments. In both the cases, you will come across some terms about the core components of Astro.

Use the following document to understand these core components of Astro and how they are related to each other.

## Deployment

An Astro _Deployment_ is an Airflow environment powered by [Astro Runtime](runtime-overview.md). It encompasses all core Airflow components, including the Airflow webserver, scheduler, and workers, along with additional tools for reliability and observability. It runs in a Kubernetes namespace in an Astro cluster and has a set of attached resources to run your Airflow tasks.

By default, each Deployment on Astro runs:

- 1 small scheduler
- Minimum 0 workers and can scale up to 10 workers
- Each worker with a concurrency of maximum 5 tasks
- Celery Executor
- The latest [Astro Runtime version](runtime-release-notes.md)

You can configure your [Deployment settings](configure-deployment-resources.md) and change these default settings in the Cloud UI or with the Astro CLI. 

After you create a Deployment, you can deploy DAGs to it using Astro CLI from your local machine or automatically push from your code repository using [a CI/CD workflow](set-up-ci-cd.md). Astro executes all DAGs within a Deployment. 

### Cluster

An _Astro cluster_ is a Kubernetes cluster that hosts the infrastructure required to run your Airflow environment, which are also known as Deployments in Astro. You must select a cluster to create a Deployment. Clusters can be either standard (shared) or dedicated to your organization. Astronomer creates and manages the clusters necessary to run your Airflow environment. 

You can create multiple dedicated clusters for your organization and manage them from the Astro cloud UI. For all available resources see [Hosted resource reference](resource-reference-hosted.md).

## Workspace

A _Workspace_ is a collection of Deployments that can be accessed by a specific group of users. You can use a Workspace to group Deployments that share a business use case or environment trait. Workspace is a logical concept and does not create any resources for your organization. However, you must have access to a Workspace where you want to create a Deployment.

For Astro Hosted Dedicated and Astro Hybrid, workspaces can be granted access to specific clusters to host Deployments.

## Organization

An Astro _Organization_ is the top most entity in Astro created for you when you sign up with Astro. It contains Workspaces, which are collections of Deployments, that are typically owned by a single team. Within an organization you can:

- [Invite users, grant roles, and manage permissions](manage-organization-users.md)
- [Compare DAG runs across environments](organization-metrics.md#astro-usage)
- [Monitor usage and manage Billing](manage-billing.md)
- [Manage Workspaces](manage-workspaces.md) and [Deployments](create-deployment.md)
- [Manage clusters](create-dedicated-cluster.md)

## Users and teams

When you sign up with Astro, a user is created for you along with your Organization. As the first user in your Astro Organization, Astro assigns you the role `Organization Owner` and you have the ability to invite new users to your organization and manage other users' permissions. 

Each Astro user can also be added to a _Team_. A team in Astro is a group of users in an Organization that you can grant the same Organization and Workspace permissions. 

You must invite a user to your Organization to grant permissions and add them to teams or Workspaces.


## See also

- [Create a Deployment](create-deployment.md)
- [Deployment resources](./resource-reference-hosted.md)
- [User permissions](user-permissions.md)

