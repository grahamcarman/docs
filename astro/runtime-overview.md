---
sidebar_label: "Astro Runtime"
title: "Astro Runtime architecture and features"
id: runtime-overview
description: Learn about what is Astro Runtime, it's architecture and features
---

The _Astro Runtime_ is a [debian-based Docker image](https://quay.io/repository/astronomer/astro-runtime) for running Airflow that's built and maintained by Astronomer. It is the most reliable and efficient way to run data pipelines, making the Airflow experience reliable, fast, and scalable. All Astro projects and Astro Deployments use Astro Runtime.

Astro Runtime bundles the classic Apache Airflow experience with tools and configurations for improved functionality, reliability, efficiency, and performance. Astronomer releases an Astro Runtime distribution for each new version of Apache airflow.

## Features

- Timely support for new patch, minor, and major versions of Apache Airflow. This includes bug fixes that have not been released by the open source project but are backported to Astro Runtime.
- Smart task concurrency defaults and high availability configurations.
- The [`astronomer-providers`](https://astronomer-providers.readthedocs.io/en/stable/index.html) package, which is an open source collection of Apache Airflow providers and modules maintained by Astronomer. It includes deferrable versions of popular operators such as `ExternalTaskSensor`, `DatabricksRunNowOperator`, and `SnowflakeOperator`.
- A custom logging module that ensures Airflow task logs are reliably available in Astro.
- A custom security manager that enforces Astro's [user roles and permissions](user-permissions.md) in the Airflow UI.
- A custom Airflow UI that includes links to Astronomer resources and exposes the currently running Docker image tag in the footer of all UI pages.

## See also

- [Create a Deployment](create-deployment.md)
- [Choosing Astro Runtime](develop-project.md#choosing-astro-runtime)
- [Astro Runtime release notes](runtime-release-notes.md)
