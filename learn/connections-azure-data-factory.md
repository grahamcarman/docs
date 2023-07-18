---
title: "Create an Azure Data Factory connection in Airflow"
id: azure-data-factory
sidebar_label: Azure Data Factory
description: Learn how to create an Azure Data Factory connection in Airflow.
---

[Azure Data Factory](https://azure.microsoft.com/en-in/products/data-factory#overview) is a cloud-based data integration and transformation service used to build data pipelines and jobs. Integrating ADF with Airflow allows you to run ADF pipelines and check their status from an Airflow DAG. 

This guide provides the basic setup for creating an ADF connection. For a complete integration tutorial, see [Run Azure Data Factory pipelines in Airflow](airflow-azure-data-factory-integration.md).

## Prerequisites

- The [Astro CLI](https://docs.astronomer.io/astro/cli/overview)
- A locally running [Astro project](https://docs.astronomer.io/astro/cli/get-started-cli)
- [Access](https://learn.microsoft.com/en-us/azure/data-factory/concepts-roles-permissions#roles-and-requirements) to your Azure Data Factory
- [Azure AD application](https://learn.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal)

## Get connection details

A connection from Airflow to Azure data factory requires the following information:

- Subscription ID
- Data factory name
- Resource group name
- Application Client ID
- Tenant ID
- Client secret

In your Azure portal, complete the following steps to retrieve all of these values:

1. Go to your [data factory](https://portal.azure.com/#view/HubsExtension/BrowseResource/resourceType/Microsoft.DataFactory%2FdataFactories) service and select the subscription that contains your data factory.
2. Copy the **Name** of your data factory and the **Resource group**.
3. Click on the subscription for your data factory, then copy the **Subscription ID** from the subscription window.
4. Select your [Azure AD application](https://portal.azure.com/#view/Microsoft_AAD_RegisteredApps/ApplicationsListBlade). Then, from the **Overview** tab, copy the **Application (client) ID** and **Directory (tenant) ID**.
5. Follow the Azure documentation to [create a new client secret](https://learn.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#option-3-create-a-new-application-secret) for your application to be used in the Airflow connection. Copy the **VALUE** of the client secret that appears.
6. Follow the Azure documentation to [assign](https://learn.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#assign-a-role-to-the-application) the [Data Factory Contributor](https://learn.microsoft.com/en-us/azure/data-factory/concepts-roles-permissions#set-up-permissions) role to your app so that Airflow can access the data factory.

## Create your connection

1. Open your Astro project and add the following line to your `requirements.txt` file:

    ```
    apache-airflow-providers-microsoft-azure
    ```

    This installs the Azure provider package, which contains the Azure Data Factory connection type.

2. Run `astro dev restart` to restart your local Airflow environment. If you're not currently running Airflow locally, run `astro dev start`.
3. In the Airflow UI for your local Airflow environment, go to **Admin** > **Connections**. Click **+** to add a new connection, select the connection type as **Azure Data Factory**.
4. Fill out the following connection fields using the information you retrieved from [Get connection details](#get-connection-details):

    - **Connection Id**: Enter a name for the connection.
    - **Client ID**:  Enter the **Application (client) ID**.
    - **Secret**: Enter the client secret **VALUE**.
    - **Tenant ID**: Enter the **Directory (tenant) ID**.
    - **Subscription ID**: Enter the **Subscription ID**.
    - **Resource Group Name**: Enter your data factory **Resource group**.
    - **Factory Name**: Enter your data factory **Name**.

5. Click **Test connection**. When the connection test succeeds, click then **Save**.

![azure-connection-data-factory](/img/guides/connection-azure-data-factory.png)

:::tip

To use the same connection for multiple data factories or multiple resource groups, you can skip the **Resource Group** and **Factory Name** fields in the connection configuration. Instead, you can pass these values to the `default_args` of a DAG or as parameters to the AzureDataFactoryOperator. For example:

```python
  "azure_data_factory_conn_id": "adf",
  "factory_name": "my-factory", 
  "resource_group_name": "my-resource-group",
```

:::

## How it works

Airflow uses the [`azure-mgmt-datafactory`](https://pypi.org/project/azure-mgmt-datafactory/) library from [Azure SDK for Python](https://github.com/Azure/azure-sdk-for-python) to connect to Azure Data Factory using [AzureDataFactoryHook](https://airflow.apache.org/docs/apache-airflow-providers-microsoft-azure/stable/_api/airflow/providers/microsoft/azure/hooks/data_factory/index.html).

## See also

- [Apache Airflow Microsoft Azure provider package documentation](https://airflow.apache.org/docs/apache-airflow-providers-microsoft-azure/6.1.1/connections/adf.html)
- [Run Azure Data Factory pipelines in Airflow](airflow-azure-data-factory-integration.md)
- [Import and export Airflow connections using Astro CLI](https://docs.astronomer.io/astro/import-export-connections-variables#from-environment-variables)