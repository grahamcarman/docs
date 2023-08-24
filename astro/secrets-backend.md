---
title: 'Configure an external secrets backend on Astro'
sidebar_label: 'Configure a secrets backend'
id: secrets-backend
---

<head>
  <meta name="description" content="Learn how you can configure a secrets backend on Astro to store Airflow variables and connections in a secure, centralized location that complies with your organization's security requirements." />
  <meta name="og:description" content="Learn how you can configure a secrets backend on Astro to store Airflow variables and connections in a secure, centralized location that complies with your organization's security requirements." />
</head>

Apache Airflow [variables](https://airflow.apache.org/docs/apache-airflow/stable/howto/variable.html) and [connections](https://airflow.apache.org/docs/apache-airflow/stable/howto/connection.html#) often contain sensitive information about your external systems that should be kept [secret](https://airflow.apache.org/docs/apache-airflow/stable/_api/airflow/secrets/index.html) in a secure, centralized location that complies with your organization's security requirements.

While secret values of Airflow variables and connections are encrypted in the Airflow metadata database of every Deployment, Astronomer recommends integrating with a secrets backend tool. This guide explains how to configure connections to various secrets backend tools on Astro.

:::tip

If you only need a local connection to your cloud for testing purposes, consider mounting your user credentials to a local Airflow environment. While this implementation is not recommended for deployed environments, it lets you quickly test pipelines with data hosted in your cloud. See [Authenticate to cloud services](cli/authenticate-to-clouds.md).

:::

## Benefits

Integrating a secrets backend tool with Astro allows you to:

- Store Airflow variables and connections in a centralized location alongside secrets from other tools and systems used by your organization, including Kubernetes secrets, SSL certificates, and more.
- Comply with internal security postures and policies that protect your organization.
- Recover in the case of an incident.
- Automatically pull Airflow variables and connections that are already stored in your secrets backend when you create a new Deployment instead of having to set them manually in the Airflow UI.

Astro integrates with the following secrets backend tools:

- Hashicorp Vault
- AWS Systems Manager Parameter Store
- AWS Secrets Manager
- Google Cloud Secret Manager
- Azure Key Vault

Secrets backend integrations are configured individually with each Astro Deployment.

:::info

If you enable a secrets backend on Astro, you can continue to define Airflow variables and connections either [as environment variables](environment-variables.md) or in the Airflow UI. If you set Airflow variables and connections in the Airflow UI, they are stored as encrypted values in the Airflow metadata database.

Airflow checks for the value of an Airflow variable or connection in the following order:

1. Secrets backend
2. Environment variable
3. The Airflow UI

:::

Using secrets to set Airflow connections requires knowledge of how to generate Airflow connection URIs. If you plan to store Airflow connections on your secrets backend, see [URI format](https://airflow.apache.org/docs/apache-airflow/stable/howto/connection.html#connection-uri-format) for guidance on how to generate a connection URI.
