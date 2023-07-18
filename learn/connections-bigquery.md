---
title: "Create a BigQuery connection in Airflow"
id: bigquery
sidebar_label: BigQuery
description: Learn how to create a BigQuery connection.
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

[BigQuery](https://cloud.google.com/bigquery) is Google's fully managed and serverless data warehouse. Integrating BigQuery with Airflow lets you execute BigQuery jobs from a DAG. 

There are multiple ways to connect Airflow and BigQuery, all of which require a [GCP Service Account](https://cloud.google.com/docs/authentication#service-accounts):

- Use the contents of a service account key file directly in an Airflow connection.
- Mount the service account key file in Airflow containers.
- Store the contents of a service account key file in a secrets backend.
- Use a Kubernetes service account to integrate Airflow and BigQuery. This is possible only if you run Airflow on Astro or Google Kubernetes Engine (GKE).

Using a Kubernetes service account is the most secure method because it doesn't require storing a secret in Airflow's metadata database, on disk, or in a secrets backend. The next most secure connection method is to store the contents of your service account key file in a secrets backend.

## Prerequisites
- The [Astro CLI](https://docs.astronomer.io/astro/cli/overview).
- A locally running [Astro project](https://docs.astronomer.io/astro/cli/get-started-cli).
- A Google Cloud project with [BigQuery API](https://cloud.google.com/bigquery/docs/enable-transfer-service#enable-api) enabled.
- Permissions to create an IAM Service Account or use an existing one. See [Google documentation](https://cloud.google.com/iam/docs/manage-access-service-accounts).

## Get connection details

A connection from Airflow to Google BigQuery requires the following information:

- Service account name
- Service account key file
- Google Cloud Project ID

<Tabs
    defaultValue="key-file-value"
    groupId= "bigquery-connection"
    values={[
        {label: 'Key file value', value: 'key-file-value'},
        {label: 'Key file in container', value: 'key-file-in-container'},
        {label: 'Key file in secrets backend', value: 'key-file-in-secrets-backend'},
        {label: 'Kubernetes service account', value: 'kubernetes-service-account'}
    ]}>

<TabItem value="key-file-value">

This method requires you to save the contents of your service account key file in your Airflow connection. 

1. In the [Google Cloud console](https://console.cloud.google.com), select your Google Cloud project and copy its **ID**.
2. [Create a new service account](https://cloud.google.com/iam/docs/service-accounts-create). 
3. [Grant roles](https://cloud.google.com/iam/docs/grant-role-console) to your service account so that it can access BigQuery. See [BigQuery roles](https://cloud.google.com/bigquery/docs/access-control#bigquery) for a list of available roles and the permissions.
4. [Add a new JSON key file](https://cloud.google.com/iam/docs/keys-create-delete#iam-service-account-keys-create-console) to the service account.
5. Copy the contents of the key file.

</TabItem>

<TabItem value="key-file-in-container">

This method requires you to mount your service account key file to your Airflow containers.

In your Google Cloud console, follow the below steps to retrieve the required values:

1. Select your Google Cloud project and copy it's **ID**.
2. Go to the IAM page, click the **Service Accounts** tab, and follow Google's documentation to [create a new service account](https://console.cloud.google.com/iam-admin/serviceaccounts/create). 
3. Follow Google's documentation [to grant roles](https://cloud.google.com/iam/docs/grant-role-console) to your service account to access BigQuery. See [BigQuery roles](https://cloud.google.com/bigquery/docs/access-control#bigquery) for list of available roles and the permissions.
4. Follow Google's documentation [to add a new key file](https://cloud.google.com/iam/docs/keys-create-delete#iam-service-account-keys-create-console) to the service account in JSON format.
5. Download the key file to your local computer to mount it to Airflow containers for [creating a connection](#create-your-connection).

</TabItem>

<TabItem value="key-file-in-secrets-backend">

You can save your service account key file to any secrets backend. See [Configure a secrets backend](https://docs.astronomer.io/astro/secrets-backend) for steps on how to configure several popular secrets backend services to use with Airflow. For example, if you use Google Secret Manager as a secrets backend:

1. In the Google Cloud console, open your Google Cloud project and copy its **ID**.
2. Go to the IAM page, click the **Service Accounts** tab, and follow Google's documentation to [create a new service account](https://console.cloud.google.com/iam-admin/serviceaccounts/create). 
3. Follow Google's documentation [to grant roles](https://cloud.google.com/iam/docs/grant-role-console) to your service account to access BigQuery. See [BigQuery roles](https://cloud.google.com/bigquery/docs/access-control#bigquery) for list of available roles and the permissions.
4. Follow Google's documentation [to add a new key file](https://cloud.google.com/iam/docs/keys-create-delete#iam-service-account-keys-create-console) to the service account in JSON format.
5. Download the key file.
6. [Create a secret](https://cloud.google.com/secret-manager/docs/create-secret-quickstart) in Google Secret Manager and upload the key file from Step 5 as a secret value. Then, copy the ID of your secret name.
7. Follow [Astro documentation](https://docs.astronomer.io/astro/secrets-backend) to configure secrets backend for your Astro project.

:::tip

This method is an example wherein you use non-Airflow secrets from the secrets backend directly in Airflow connections and variables. 

This allows you to reuse a secret in different Airflow connections without having to maintain multiple copies.

:::

</TabItem>

<TabItem value="kubernetes-service-account">

A [Kubernetes service account](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/) provides an identity to the processes running in a Pod. The process running inside a Pod can use this identity of its associated service account to authenticate cluster's API server. This is also referred to as Workload Identity in [GCP](https://cloud.google.com/kubernetes-engine/docs/concepts/workload-identity) and [Azure](https://learn.microsoft.com/en-us/azure/aks/learn/tutorial-kubernetes-workload-identity).

This method cannot be used in local Airflow environment. It is available to use with Airflow on Astro or OSS Airflow running on Kubernetes clusters. 

If you're running [Airflow on Astro](https://docs.astronomer.io/astro/trial), Workload Identity is enabled by default.  See [Connect to GCP](https://docs.astronomer.io/astro/connect-gcp?tab=Workload%20Identity#authorization-options) or [Connect to AWS](https://docs.astronomer.io/astro/connect-aws#authorization-options) to grant a Deployment access to BigQuery on Astro. After you complete these steps, any Google Cloud connection you create in the Deployment will use your workload identity by default to access BigQuery.

If you're running Airflow in a GKE cluster, complete the following steps:

1. In your Google Cloud console, open the Google Cloud project where you're running BigQuery and copy its **ID**.
2. [Enable Workload Identity](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity) and [configure Airflow to use workload identity](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity#authenticating_to). Copy the name for the Kubernetes service account that you create. 
3. Go to **IAM**, then click **Service Accounts** tab, and search your Kubernetes service account. If you don't see your service account, click **+ ADD** to add your service account to your Google Cloud project.
4. [Grant roles](https://cloud.google.com/iam/docs/grant-role-console) to your service account to access BigQuery. See [BigQuery roles](https://cloud.google.com/bigquery/docs/access-control#bigquery) for list of available roles and the permissions.

</TabItem>

</Tabs>

## Create your connection

<Tabs
    defaultValue="key-file-value"
    groupId= "bigquery-connection"
    values={[
        {label: 'Key file value', value: 'key-file-value'},
        {label: 'Key file in container', value: 'key-file-in-container'},
        {label: 'Key file in secrets backend', value: 'key-file-in-secrets-backend'},
        {label: 'Kubernetes service account', value: 'kubernetes-service-account'}
    ]}>

<TabItem value="key-file-value">

1. Open your Astro project and add the following line to your `requirement.txt` file to install and use the package for Google Cloud in Airflow:

    ```
    apache-airflow-providers-google
    ```

2. Restart your local Airflow using `astro dev restart`. If you're not currently running Airflow locally, you can also run `astro dev start`.
3. In the Airflow UI, go to **Admin** > **Connections**. Click the **+** sign to add a new connection, select the connection type as **Google Cloud** and give it a name in the **Connection Id** field.
4. Paste the value of the key file copied in step #4 of [Get connection details](#get-connection-details) in the connection fields:
    - **Keyfile JSON**: `<mykeyfile-json>`
5. Click on **Test** connection to test and then **Save** your connection.

    ![gcp-connection-key-in-ui](/img/guides/connection-gcp-key-in-ui.png)

</TabItem>

<TabItem value="key-file-in-container">

Follow the below steps to create a connection:

1. Open your Astro project and add the following line to your `requirement.txt` file to install and use the package for Google Cloud in Airflow:
    ```
    apache-airflow-providers-google
    ```
2. Add the key file to your Astro project `include` folder. This will make it available to Airflow at `/usr/local/airflow/include/<your-key-file>.json`.
3. Restart your local Airflow using `astro dev restart`. If you're not currently running Airflow locally, you can also run `astro dev start`.
4. In the Airflow UI, go to **Admin** > **Connections**. Click the **+** sign to add a new connection, select the connection type as **Google Cloud** and give it a name in the **Connection Id** field.
5. Paste the path of the service account key file in the Airflow container in the connection field:
    - **Keyfile Path**: `/usr/local/airflow/include/<my-key-file.json>`
6. Click on **Test** connection to test and then **Save** your connection.

    ![gcp-connection-key-in-airflow-container](/img/guides/connection-gcp-key-in-airflow-container.png)

</TabItem>

<TabItem value="key-file-in-secrets-backend">


1. Open your Astro project and add the following line to your `requirement.txt` file to install and use the package for Google Cloud in Airflow:
    ```
    apache-airflow-providers-google
    ```
2. Restart your local Airflow using `astro dev restart`. If you're not currently running Airflow locally, you can also run `astro dev start`.
3. In the Airflow UI, go to **Admin** > **Connections**. Click the **+** sign to add a new connection, select the connection type as **Google Cloud** and give it a name in the **Connection Id** field.
4. Paste the GCP project ID copied in step #1 and secret name from step #6 of [Get connection details](#get-connection-details) in the connection fields:
    - **Keyfile Secret Project Id**: `<gcp-project-id>`
    - **Keyfile Secret Name**: `<keyfile-secret-name>`
5. Click on **Test** connection to test and then **Save** your connection.

    ![gcp-connection-key-in-secret-manager](/img/guides/connection-gcp-key-in-secret-manager.png)

</TabItem>

<TabItem value="kubernetes-service-account">

To create a connection using workfload identity, follow the below steps:

1. Login to your Airflow UI
2. Go to **Admin** > **Connections**. Click the **+** sign to add a new connection, select the connection type as **Google Cloud** and give it a name in the **Connection Id** field.
3. Paste the GCP project ID copied in step #1 of [Get connection details](#get-connection-details) to the connection field:
    - **Project Id**: `<project-id>`
4. Click on **Save** to save your connection.

    ![gcp-connection-using-workload-identity](/img/guides/connection-gcp-workload-identity.png)


</TabItem>

</Tabs>

## How it works

Airflow uses [`python-bigquery`](https://github.com/googleapis/python-bigquery) library to connect to GCP BigQuery using [BigQueryHook](https://airflow.apache.org/docs/apache-airflow-providers-google/stable/_api/airflow/providers/google/cloud/hooks/bigquery/index.html). If you don't define any specific key credentials in the connection, Google defaults to using [Application Default Credentials (ADC)](https://cloud.google.com/docs/authentication/application-default-credentials). This means when you use Workload Identity to connect to BigQuery, Airflow relies on ADC to authenticate.

## See also

- [Apache Airflow Google provider OSS docs](https://airflow.apache.org/docs/apache-airflow-providers-google/stable/connections/gcp.html)
- BigQuery [Modules](https://registry.astronomer.io/modules?query=bigquery) and [Example DAGs](https://registry.astronomer.io/dags?limit=24&sorts=updatedAt%3Adesc&query=bigquery) in Astronomer Registry
- [Configure a secrets backend](https://docs.astronomer.io/astro/secrets-backend) to store your Airflow connections.