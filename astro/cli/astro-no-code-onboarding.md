---
sidebar_label: 'Run your first DAG in the Astro UI'
title: 'Run your first DAG with the Cloud UI'
id: 'run-dag-ui'
---

<head>
  <meta name="description" content="Learn how to run your first Apache Airflow DAG on Astro using the Cloud UI." />
  <meta name="og:description" content="Learn how to run your first Apache Airflow DAG on Astro using the Cloud UI." />
</head>

import {siteVariables} from '@site/src/versions';

Astro is the industry's leading managed service for Apache Airflow. Without writing any custom code or installing any prerequisites, you can run an Apache Airflow DAG using only the Cloud UI. 

This quickstart explains the steps required to deploy an example DAG to Astro and trigger a DAG run from the Cloud UI.

Specifically, you will:

- Start an Astro trial
- Authenticate and log in to Astro. 
- Create a Deployment. 
- Create an Astro project. 
- Deploy DAGs to Astro in the Cloud UI.
- Trigger a run of an example DAG in the Airflow UI. 

This tutorial takes about 15 minutes. You can also create and run your first DAG [from the Astro CLI](create-first-dag.md) in about 15 minutes. However, if you're new to Airflow and want a more in-depth tutorial, see [Write your First DAG](https://docs.astronomer.io/learn/get-started-with-airflow).

## Prerequisites

- An Astro account. To start an Astro trial, see [Start a trial](trial.md). 

