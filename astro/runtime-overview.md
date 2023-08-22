---
sidebar_label: "Astro Runtime"
title: "Astro Runtime "
id: runtime-overview
description: Learn about what is Astro Runtime, it's architecture and features
---

[Astro Runtime](https://quay.io/repository/astronomer/astro-runtime) is a debian-based Docker image for running Airflow that's built and maintained by Astronomer. Every Astro project and Deployment is configured with a version of Astro Runtime. Astro Runtime is a requirement for deploying to Astro and using Astro CLI.

Use this document to understand the architecture of Astro Runtime and its features.

## Architecture

Astro Runtime utilizes OSS Apache Airflow at its core and extends it with additional layers to provide your organization with improved functionality, reliability, efficiency, and performance.

<!-- ![Astro Runtime overview](/img/docs/runtime-overview.png)  -->

<div style={{textAlign: 'center'}}>

![Astro Runtime overview](/img/docs/runtime-overview.png)

</div>

The Astro Runtime includes the following:

- OSS Airflow with expedited [vulnerability fixes and backport support](./runtime-version-lifecycle-policy.md#backport-policy-for-bug-and-security-fixes)
- [Pre-installed packages](#provider-packages):
  - [Astronomer providers package](https://registry.astronomer.io/providers/astronomer-providers/versions/1.17.3) which provides the capability for defferable operators, hooks and sensors
  - [Astro SDK package](https://github.com/astronomer/astro-sdk) which enables users to develop ETL workflows with ease
  - OpenLineage package to gather lineage metadata and power Astro alerts
- Advanced logging and security:
  - Custom logging to cloud storage for task logs
  - Custom security manager for role-based access

## Features

- Timely support for new patch, minor, and major versions of Apache Airflow. This includes bug fixes that have not been released by the open source project but are backported to Astro Runtime and available to users earlier.
- Exclusive features to enrich the task execution experience, including smart task concurrency defaults and high availability configurations.
- The [`astronomer-providers`](https://astronomer-providers.readthedocs.io/en/stable/index.html) package. This package is an open source collection of Apache Airflow providers and modules maintained by Astronomer. It includes deferrable versions of popular operators such as `ExternalTaskSensor`, `DatabricksRunNowOperator`, and `SnowflakeOperator`.
- The `openlineage-airflow` package. [OpenLineage](https://openlineage.io/) standardizes the definition of data lineage, the metadata that forms lineage metadata, and how data lineage metadata is collected from external systems. This package enables [data lineage on Astro](data-lineage-concepts.md).
- A custom logging module that ensures Airflow task logs are reliably available to the Astro data plane.
- A custom security manager that enforces [user roles and permissions](user-permissions.md) as defined by Astro.
- A custom Airflow UI that includes links to Astronomer resources and exposes the currently running Docker image tag in the footer of all UI pages.
- (Astro Hybrid only) A monitoring DAG that the Astronomer team uses to monitor the health of Astro Deployments.

### Executors

In Airflow, the executor is responsible for determining how and where a task is completed.

In all local environments created with the Astro CLI, Astro Runtime runs the [Local executor](https://airflow.apache.org/docs/apache-airflow/stable/executor/local.html). On Astro, Astro Runtime supports the [Celery executor](https://airflow.apache.org/docs/apache-airflow/stable/executor/celery.html) and the [Kubernetes executor](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/executor/kubernetes.html).

### Provider packages

Astro Runtime comes pre-packaged with many open source provider packages along with Astronomer provider packages. Some of these packages are not installed by default in Apache Airflow.

<!-- The latest version of the Astro Runtime image has the following open source provider packages pre-installed. Providers marked with an asterisk (*) are installed only in Astro Runtime and not installed by default on Apache Airflow. -->

| Provider package                                                                                                 | Astro Runtime | OSS Airflow |
| ---------------------------------------------------------------------------------------------------------------- | :-----------: | :---------: |
| [`astronomer-providers`](https://pypi.org/project/astronomer-providers/)                                         |       ✓       |             |
| [`apache-airflow-providers-amazon`](https://pypi.org/project/apache-airflow-providers-amazon/)                   |       ✓       |             |
| [`astro-sdk-python`](https://pypi.org/project/astro-sdk-python/)                                                 |       ✓       |             |
| [`apache-airflow-providers-datadog`](https://pypi.org/project/apache-airflow-providers-datadog/)                 |       ✓       |             |
| [`apache-airflow-providers-elasticsearch`](https://pypi.org/project/apache-airflow-providers-elasticsearch/)     |       ✓       |             |
| [`apache-airflow-providers-google`](https://pypi.org/project/apache-airflow-providers-google/)                   |       ✓       |             |
| [`apache-airflow-providers-microsoft-azure`](https://pypi.org/project/apache-airflow-providers-microsoft-azure/) |       ✓       |             |
| [`openlineage-airflow`](https://pypi.org/project/openlineage-airflow/)                                           |       ✓       |             |
| [`apache-airflow-providers-postgres`](https://pypi.org/project/apache-airflow-providers-postgres/)               |       ✓       |             |
| [`apache-airflow-providers-sqlite`](https://pypi.org/project/apache-airflow-providers-sqlite/)                   |       ✓       |      ✓      |
| [`apache-airflow-providers-celery`](https://pypi.org/project/apache-airflow-providers-celery/)                   |       ✓       |      ✓      |
| [`apache-airflow-providers-cncf-kubernetes`](https://pypi.org/project/apache-airflow-providers-cncf-kubernetes/) |       ✓       |      ✓      |
| [`apache-airflow-providers-ftp`](https://pypi.org/project/apache-airflow-providers-ftp/)                         |       ✓       |      ✓      |
| [`apache-airflow-providers-http`](https://pypi.org/project/apache-airflow-providers-http/)                       |       ✓       |      ✓      |
| [`apache-airflow-providers-imap`](https://pypi.org/project/apache-airflow-providers-imap/)                       |       ✓       |      ✓      |


### Default environment variables

Astro Runtime images include baked-in environment variables that have different default values than on OSS Apache Airflow to improve your Airflow experience in most use-cases. The following table lists these environment variables.

| Environment Variable                                            | Description                                                                                                                                                                                 | Value                                                                        |
| --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| `AIRFLOW__SCHEDULER__DAG_DIR_LIST_INTERVAL`                     | The time in seconds that Airflow waits before re-scanning the `dags` directory for new files. Note that this environment variable is set for all Deployments regardless of Runtime version. | `30`                                                                         |
| `AIRFLOW__CELERY__STALLED_TASK_TIMEOUT`                         | The maximum time in seconds that tasks running with the Celery executor can remain in a `queued` state before they are automatically rescheduled.                                           | `600`                                                                        |
| `AIRFLOW_CORE_PARALLELISM`                                      | The maximum number of task instances that can run concurrently for each scheduler in your Deployment.                                                                                       | `[number-of-running-workers-for-all-worker-queues] * [max-tasks-per-worker]` |
| `AIRFLOW__KUBERNETES_EXECUTOR__WORKER_PODS_CREATION_BATCH_SIZE` | The number of worker Pods that can be created each time the scheduler parses DAGs. This setting limits the number of tasks that can be scheduled at one time.                               | `16`                                                                         |

Unlike [global environment variables](platform-variables.md), you can override the values of these variables for specific use cases. To edit the values of the default Airflow environment variables, see [Set environment variables on Astro](environment-variables.md).


### Astro monitoring DAG (Hybrid only)

Astro Runtime includes a monitoring DAG that is pre-installed in the Docker image and enabled for all Deployments on Astro Hybrid. In addition to generating Deployment health and metrics functionality, this DAG allows the Astronomer team to monitor the health of your data plane by enabling real-time visibility into whether your workers are healthy and tasks are running.

The `astronomer_monitoring_dag` runs a simple bash task every 5 minutes to ensure that your Airflow scheduler and workers are functioning as expected. If the task fails twice in a row or is not scheduled within a 10-minute interval, Astronomer support receives an alert and will work with you to troubleshoot. The DAG runs and appears in the Airflow UI only on Astro Deployments.

Because this DAG is essential to Astro's managed service, you are not charged for its task runs. For the same reasons, this DAG can't be modified or disabled through the Airflow UI. To modify when this DAG runs on a Deployment, set the following [Deployment environment variable](environment-variables.md):

- Key: `AIRFLOW_MONITORING_DAG_SCHEDULE_INTERVAL`
- Value: An alternative schedule defined as a [cron expression](https://crontab.guru/)


## Astro Runtime, Airflow and Python versions

| Astro Runtime | Python version | Airflow version |
| :-----------: | :------------: | :-------------: |
|       4       |      3.9       |       2.2       |
|       5       |      3.9       |       2.3       |
|       6       |      3.9       |       2.4       |
|       7       |      3.9       |       2.5       |
|       8       |      3.10      |       2.6       |
|       9       |      3.11      |       2.7       |

Starting with Astro Runtime 9, if you require a different version of Python than what's included in the base distribution, you can use a Python distribution of Astro Runtime. See [Distribution](runtime-version-lifecycle-policy.md#distribution) for how to use these images in your `Dockerfile`.

To run different Python versions in Airflow for Astro Runtime 6, 7 and 8, Astronomer recommends to you use the [`ExternalPythonOperator`](http://docs.astronomer.io/learn/external-python-operator). For Astro Runtime versions older than 6, you can use the `KubernetesPodOperator` or the `PythonVirtualenvOperator`. See [Run Airflow tasks in isolated environments](http://docs.astronomer.io/learn/external-python-operator#other-methods-for-running-tasks-in-isolated-environments).

For version compatibility information, see the [Runtime release notes](runtime-release-notes.md).

## See also

- [Create a Deployment](create-deployment.md)
- [Upgrade Astro Runtime](upgrade-runtime.md)
- [Astro Runtime release notes](runtime-release-notes.md)
