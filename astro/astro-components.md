---
sidebar_label: 'Key Concepts'
title: 'Astro concepts and components'
id: astro-components
description: Learn about the components of Astro
---

Astro is a SaaS application that provides fully managed Apache Airflow environments for teams of all sizes. You have the option to choose a standard or a dedicated cluster for your Deployments. In both the cases, you will come across some terms about the core components of Astro.

Use the following document to understand these core components of Astro and how they are related to each other.

## Organization

An Astro _Organization_ is the top most entity in Astro created for you when you sign-up with Astro. It contains Workspaces, which are collections of Deployments, or Airflow environments, that are typically owned by a single team. Within an organization you can :

- Invite users, grant roles and manage permissions
- Compare DAG runs across environments
- Monitor usage and manage Billing
- Manage Workspaces and Deployments
- Manage clusters (Hosted Dedicated or Hybrid only)

## Cluster

An Astro cluster is a Kubernetes cluster that hosts the infrastructure required to run your Airflow environment (also known as Deployments in Astro). You must select a cluster to create a Deployment. Clusters can be standard (shared) or dedicated to your organization. They are created and managed by Astronomer. 

You can create multiple dedicated clusters for your organization and manage them from the Astro cloud UI.

## Workspace

A Workspace is a collection of Deployments that can be accessed by a specific group of users. A Workspace is used to group Deployments that share a business use case or environment trait. Workspace is a logical concept and does not create any resources for your organization. It is must for creating a Deployment.

For Astro Hosted Dedicated and Astro Hybrid, workspaces can be granted access to specific clusters to host Deployments.

## Deployment

An Astro Deployment is an Airflow environment that is powered by [Astro Runtime](runtime-overview.md). It encompasses all core Airflow components, including the Airflow webserver, scheduler, and workers, along with additional tools for reliability and observability. It runs in a kubernetes namespace in an Astro cluster and has a set of resources attached to run your Airflow tasks.

By default, each Deployment on Astro runs:

- 1 small scheduler
- Minimum 0 worker and can scale up to 10 workers
- Each worker with a concurrency of maximum 5 tasks
- Celery Executor
- The latest Astro Runtime version

You can configure your [Deployment settings](configure-deployment-resources.md) and change these default settings from the Cloud UI or using Astro CLI. See [Hosted resource reference](resource-reference-hosted.md) for available resources.

After you create a Deployment, you can deploy DAGs to it using Astro CLI from your local machine or automatically using a CI/CD workflow. All DAGs and tasks on Astro are executed within a Deployment. 

## Users and teams

When you sign-up with Astro, a user is created for you along with your Organization. As the first user in your Astro Organization, you get the role `Organization Owner` and have the ability to invite new users to your organization and manage permissions. 

Each Astro user can also be added to Team. A team in Astro is a group of users in an Organization that you can grant the same Organization and Workspace permissions. 

You must invite a user to your Organization to grant permissions and add them to teams or Workspaces.

<!-- ## Interaction of Astro components

The following diagrams represent an example of how different components are related to each other in Astro based on the type of cluster you choose.

### In a standard cluster

A _standard_ cluster is a multi-tenant cluster that's hosted and managed by Astronomer.

![Astro standard cluster](/img/docs/astro-standard.png)

### In a dedicated cluster

A _dedicated_ cluster is a cluster that's hosted and managed by Astronomer solely for use by your Organization. 

![Astro dedicated cluster](/img/docs/astro-dedicated.png) -->


## See also

- [Create a Deployment](create-deployment.md)
- [Deployment resources](./resource-reference-hosted.md)
- [User permissions](user-permissions.md)

