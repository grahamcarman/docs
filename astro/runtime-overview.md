---
sidebar_label: "Astro Runtime"
title: "Astro Runtime architecture and features"
id: runtime-overview
description: Learn about what is Astro Runtime, it's architecture and features
---

The _Astro Runtime_ is a [debian-based Docker image](https://quay.io/repository/astronomer/astro-runtime) for running Airflow that's built and maintained by Astronomer. It is the most reliable and efficient way for teams to run data pipelines, bundling Apache Airflow with optimized configurations and commercial add-ons that make it faster and more powerful.

Astro Runtime uses OSS Apache Airflow at its core and extends it to provide your organization with improved functionality, reliability, efficiency, and performance. With OSS Airflow as its base, Astro Runtime focuses on improving the following areas:

<center>

| Area | Improvements | 
|:----:|---------|
| Performance | - Capability for deferrable operators, hooks, and sensors using [Astronomer providers package](https://registry.astronomer.io/providers/astronomer-providers/versions/1.17.3) <p> - Capability to develop ETL workflows with ease using [Astro SDK package](https://github.com/astronomer/astro-sdk) </p> |
| Stability | - Custom logging for task logs recorded directly to cloud storage <p> - Ability to auto-scale </p> | 
| Security | - Expedited [vulnerability fixes and backport support](runtime-version-lifecycle-policy.md#backport-policy-for-bug-and-security-fixes) <p> - Support for role-based access control (RBAC) using a custom security manager </p> |

</center>

Every version of Astro Runtime correlates to an Apache Airflow version. Astro Runtime is a requirement for using the Astro CLI and deploying code to Astro. Therefore, every Astro project and Deployment is configured with a version of Astro Runtime. See [Astro Runtime and Airflow version mapping](runtime-version-lifecycle-policy.md#astro-runtime-airflow-and-python-version-mapping).

## Features

- Timely support for new patch, minor, and major versions of Apache Airflow. This includes bug fixes that have not been released by the open source project but are backported to Astro Runtime and available to users earlier.
- Exclusive features to enrich the task execution experience, including smart task concurrency defaults and high availability configurations.
- The [`astronomer-providers`](https://astronomer-providers.readthedocs.io/en/stable/index.html) package is an open source collection of Apache Airflow providers and modules maintained by Astronomer. It includes deferrable versions of popular operators such as `ExternalTaskSensor`, `DatabricksRunNowOperator`, and `SnowflakeOperator`.
- A custom logging module that ensures Airflow task logs are reliably available to the Astro data plane.
- A custom security manager that enforces [user roles and permissions](user-permissions.md) as defined by Astro.
- A custom Airflow UI that includes links to Astronomer resources and exposes the currently running Docker image tag in the footer of all UI pages.
- Ability to choose between CeleryExecutor and KubernetesExecutor in an Astro Deployment.

## See also

- [Create a Deployment](create-deployment.md)
- [Choosing Astro Runtime](develop-project.md#choosing-astro-runtime)
- [Astro Runtime release notes](runtime-release-notes.md)
