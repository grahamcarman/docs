---
sidebar_label: 'Hybrid Billing'
title: 'Hybrid Astro billing'
id: hybrid-billing
description: "Change or view your billing details and view your current spend from the Cloud UI."
---

Astro Hybrid meters and bills based on consumption of resources associated with clusters, Deployments, and workers. 

## Understand your billing

Pricing for Astro Hybrid is charged depending on the number of tasks run on Astronomer resources.

### Dynamic task mapping and Cosmos

[Cosmos](https://astronomer.github.io/astronomer-cosmos/#) allows you to run dbt Core projects on Apache Airflow using DAGs and task groups.

When you use the Cosmos `dbtTaskGroup`operator for dynamic tasks, it counts as only one task run for the task group instead of charging for individual tasks or DAGs contained in the group.