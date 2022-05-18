---
sidebar_label: 'Astro CLI'
title: 'Astro CLI Release Notes'
id: cli-release-notes
description: Release notes for the Astro CLI.
---

## Overview

This document provides a summary of all changes made to the [Astro CLI](install-cli.md). For general product release notes, go to [Astro Release Notes](release-notes.md).

If you have any questions or a bug to report, don't hesitate to reach out to us via Slack or Intercom. We're here to help.

## v1.5.0

Release date: April 28, 2022

### New Command to Update Deployment Environment Variables

A new `astrocloud deployment variable update` command allows you to more easily update an existing environment variable by typing a new value directly into your command line or adding the updated variable to a `.env` file.

This command replaces the `—update` flag that was previously released with the `astrocloud deployment variable create` command. For more information, see the [Astro CLI Command Reference](cli-reference/astrocloud-deployment-variable-create.md).

### Additional Improvements

- When you run `astrocloud workspace switch`, you can now specify a `<workspace-id>` as part of the command and avoid the prompt to manually select a Workspace
- You now need to provide an email address only the first time you run `astrocloud auth login`. After you run that command once successfully, the Astro CLI will cache your email address in your `config.yaml` file and not prompt you to enter it again
- The `astrocloud deploy` and `astrocloud dev start` commands will now inform you if there is a new version of Astro Runtime available

### Bug Fixes

- Fixed an issue were the `astrocloud deployment variable create —load` command would fail if the specified `.env` file had a comment (e.g. `#  <comment>`) in it
- Fixed an issue were Deployment API Keys would not work locally for some users

## v1.4.0

Release date: April 14, 2022

### New Command to Create and Update Environment Variables

`astrocloud deployment variable create` is a new Astro CLI command that allows you to create and update [environment variables](environment-variables.md) for a Deployment on Astro. New environment variables can be loaded from a file (e.g. `.env`) or specified as inputs to the CLI command itself. If you already set environment variables [via a `.env` file locally](develop-project.md#set-environment-variables-via-env-local-development-only), this command allows you to set environment variables on Astro from that file as well. More generally, this command makes it easy to automate creating or modifying environment variables instead of setting them manually via the Cloud UI.

For more information about this command and its options, see the [Astro CLI Command Reference](cli-reference/astrocloud-deployment-variable-create.md).

### New Command to List and Save Deployment Environment Variables

You can now list existing environment variables for a given Deployment and save them to a local `.env` file with a new `astrocloud deployment variable list` command. This command makes it easy to export existing environment variables for a given Deployment on Astro and test DAGs with them in a local Airflow environment.

For more information about this command and its options, see the [Astro CLI Command Reference](cli-reference/astrocloud-deployment-variable-list.md).

### Additional Improvements

- You can now specify a custom image name in your Astro project's `Dockerfile` as long as the image is based on an existing Astro Runtime image

## v1.3.4

Release date: April 11, 2022

### Additional Improvements

- Improved the performance of `astrocloud dev start`
- When you successfully push code to a Deployment via `astrocloud deploy`, the CLI now provides URLs for accessing the Deployment's Cloud UI and Airflow UI pages.

## v1.3.3

Release date: March 31, 2022

### Additional Improvements

- The `astrocloud dev start` command should now be ~30 seconds faster
- When `astrocloud dev parse` results in an error, the error messages now specify which DAGs they apply to
- If your DAGs don't pass the basic unit test that's included in your Astro project (`test_dag_integrity.py` ), running them with `astrocloud dev pytest` will now provide more information about which part of your code caused an error

### Bug Fixes

- Fixed an issue where running `astrocloud dev parse/pytest` would occasionally result in an "orphaned containers" warning
- Fixed an issue where `astrocloud dev parse/pytest` would crash when parsing projects with a large number of DAGs
- Fixed an issue were some `docker-compose.override.yml` files would cause `astrocloud dev parse/pytest` to stop working

## v1.3.2

Release date: March 17, 2022

:::info

Astro CLI 1.3.2 is a direct patch replacement for 1.3.1, which is no longer available for download because it includes a critical bug related to `astrocloud dev parse/pytest`. If you are currently using Astro CLI 1.3.1, then we recommend upgrading to 1.3.2+ as soon as possible to receive important bug fixes.

:::

### Support for Identity-Based Login Flow

To better integrate with Astro's identity-based login flow, the CLI now prompts you for your login email after you run `astrocloud auth login`. Based on your email, the CLI assumes your Astro Organization and automatically brings you to your Organization's login flow via web browser.

### Additional Improvements

- `astrocloud deploy` now builds and tests only one image per deploy. This should result in improved deployment times in CI/CD pipelines which use this command.
- The `test` directory generated by `astrocloud dev init` now includes more example pytests.

### Bug Fixes

- Partially fixed `dev parse` permission errors on WSL. To fully fix this issue for an Astro project, you must delete the project's existing `.astrocloud` directory and rerun `astrocloud dev init`.
- Fixed an issue where running `astrocloud dev parse/pytest` while a local Airflow environment was running would crash the Airflow environment. This issue was introduced in Astro CLI 1.3.1, which is no longer available for download.

## v1.3.0

Release date: March 3, 2022

### New Command to Parse DAGs for Errors

`astrocloud dev parse` is a new Astro CLI command that allows you to run a basic test against your Astro project to ensure that essential aspects of your code are properly formatted. This includes the DAG integrity test that is run with `astrocloud dev pytest`, which checks that your DAGs are able to to render in the Airflow UI.

This command was built to replace the need to constantly run `astrocloud dev restart` during troubleshooting to see if your DAGs render in the Airflow UI. Now, you can quickly run `astrocloud dev parse` and see import and syntax errors directly in your terminal without having to restart all Airflow services locally. For more complex testing, we still recommend using `astrocloud dev pytest`, which allows you to run other custom tests in your project.

For more information about `astrocloud dev parse`, see the [CLI Command Reference](cli-reference/astrocloud-dev-parse.md). For more guidance on testing DAGs locally, see [Test DAGs Locally](test-and-troubleshoot-locally.md#test-dags-locally).

### `astrocloud deploy` Parses DAGs by Default

To better protect your Deployments from unexpected errors, `astrocloud deploy` now automatically applies tests from `astrocloud dev parse` to your Astro project before completing the deploy process. If any of these tests fail, the CLI will not push your code to Astro.

For more information about `astrocloud deploy`, see [CLI Command Reference](cli-reference/astrocloud-deploy.md).

:::danger Breaking Change

For Deployments running Astro Runtime 4.1.0+, `astrocloud deploy` will no longer complete the code push to your Deployment if your DAGs contain basic errors. If any files in your Astro project contain these errors, then certain deploys might stop working after you upgrade the Astro CLI to v1.3.0.

To maintain the CLI's original behavior, use `astrocloud deploy --force`. This command forces a deploy even if errors are detected in your DAGs.

:::

### New Command to Update Deployment Configurations

You can now use `astrocloud deployment update` to update certain configurations for an existing Astro Deployment directly from the Astro CLI. The configurations that you can update are:

- Deployment name
- Deployment description
- Scheduler resources
- Scheduler replicas
- Worker resources

This is the same set of configurations that you can modify via the **Edit Configuration** view in the Cloud UI. For more information on modifying a Deployment, see [Configure a Deployment](configure-deployment.md). For more information on this command, see [CLI Command Reference](cli-reference/astrocloud-deployment-update.md).

## v1.2.0

Release date: February 25, 2022

### Deploy to Astro with Deployment API Keys for Simpler CI/CD

You can now use [Deployment API keys](api-keys.md) to run `astrocloud deploy` either from the CLI directly or via a CI/CD script. This update simplifies deploying code to Astro via CI/CD.

With an existing Deployment API key, you can set `ASTRONOMER_KEY_ID` and `ASTRONOMER_KEY_SECRET` as OS-level environment variables. From there, you can now configure a CI/CD pipeline that:

- Installs the Astro CLI.
- Runs `astrocloud deploy`.

When `astrocloud deploy` is run, the CLI will now automatically look for and use the Deployment API key credentials that were set as environment variables to authorize and complete a code push.

Previously, any script that automated code pushes to Astro had to include a series of `cURL` requests to the Cloud API and could not use Deployment API keys to run an Astro CLI command. If your existing CI/CD pipelines still utilize this method, we recommend replacing those commands with an Astro CLI-based workflow. For more information and guiding examples, see [CI/CD](ci-cd.md).

### New Command to Run DAG Unit Tests with pytest

You can now run custom unit tests for all DAGs in your Astro project with `astrocloud dev pytest`, a new Astro CLI command that uses [pytest](https://docs.pytest.org/en/7.0.x/index.html), a common testing framework for Python. As part of this change, new Astro projects created via `astrocloud dev init` now include a `tests` directory, which includes one example pytest built by Astronomer.

When you run this command, the Astro CLI creates a local Python environment that includes your DAG code, dependencies, and Astro Runtime Docker image. The CLI then runs any pytests in the `tests` directory and shows you the results of those tests in your terminal. You can add as many custom tests to this directory as you'd like.

For example, you can use this command to run tests that check for:

- Python and Airflow syntax errors.
- Import errors.
- Dependency conflicts.
- Unique DAG IDs.

These tests don't require a fully functional Airflow environment in order to execute, which makes this Astro CLI command the fastest and easiest way to test DAGs locally.

In addition to running tests locally, you can also run pytest as part of the Astro deploy process. To do so, specify the `--pytest` flag when running `astrocloud deploy`. This ensures that your code push to Astro automatically fails if any DAGs do not pass all pytests specified in the `tests` directory of your Astro project. For more information, see [Test DAGs Locally with pytest](test-and-troubleshoot-locally.md#test-dags-locally-with-pytest).

### New Command to View Deployment Scheduler Logs

If you prefer to troubleshoot DAGs and monitor your Deployments from the command line, you can now run `astrocloud deployment logs`, a new Astro CLI command that allows you to view the same Scheduler logs that appear in the **Logs** tab of the Cloud UI.

When you run this command, all Scheduler logs emitted by a Deployment over the last 24 hours appear in your terminal. Similarly to the Cloud UI, you can filter logs by log level using command flags. For more information about this command, see the [CLI Command Reference](cli-reference/astrocloud-deployment-logs.md).

### New Commands to Create and Delete Deployments on Astro

You can now use the Astro CLI to create and delete Deployments on Astro with two new commands:

- `astrocloud deployment create`
- `astrocloud deployment delete`

These commands are functionally identical to the [Deployment configuration](configure-deployment.md) and deletion process in the Cloud UI. For more information, see the [CLI Command Reference](cli-reference/astrocloud-deployment-create.md).

## v1.1.0

Release date: February 17, 2022

### New `astrocloud dev restart` Command to Test Local Changes

For users making quick and continuous changes to an Astronomer project locally, the Astro CLI now supports a new `astro dev restart` command. This command makes local testing significantly easier and is equivalent to running `astrocloud dev stop` followed by `astrocloud dev start`.

### Support for the Triggerer in Local Airflow Environments

The Astro CLI now supports the Apache Airflow [Triggerer component](https://airflow.apache.org/docs/apache-airflow/stable/concepts/deferring.html?) in a local environment. This means that you can test DAGs that use [deferrable operators](deferrable-operators.md) locally before pushing them to a Deployment on Astronomer. Additionally, Triggerer logs appear alongside Webserver and Scheduler logs when you run `astrocloud dev logs`.

The Triggerer will only be created in local environments running Astro Runtime 4.0.0+.

### Additional Improvements

- Postgres has been upgraded from 12.2 to [12.6](https://www.postgresql.org/docs/12/release-12-6.html) for local Airflow environments.

## v1.0.0

Release date: February 3, 2022

### Introducing the Astro CLI

The Astro CLI (`astrocloud`) is now generally available as the official command-line tool for Astro. It is a direct replacement of the previously released `astro` executable.

The Astro CLI sets the foundation for more robust functionality in the future and includes several significant improvements to both the local development experience as well as use cases specific to Astro. These changes are summarized in the following sections.

The Astro CLI can be installed via Homebrew. Commands take the form of:

```sh
astrocloud <command> # E.g. `astrocloud dev start`
```

We strongly recommend that all users install the Astro CLI and delete the `astro` executable from local directories as soon as possible. For guidelines, read [Install the Astro CLI](install-cli.md). As of February 2022, `astro` will no longer be maintained by our team. With that said, the release of the Astro CLI does not have any impact on your existing Deployments or DAGs.

### New Authentication Flow

The Astro CLI introduces an easy way to authenticate. Instead of requiring that users manually pass authentication tokens, the new CLI consists of a simple, browser-based login process.

Built with refresh tokens, the Astro CLI also does not require that users re-authenticate every 24 hours, as was the case with `astro`. As long as you remain authenticated via the Cloud UI, your session via the Astro CLI will remain valid. You can expect to be asked to re-authenticate only once every few months instead of on a daily basis.

### Improved Local Development

Astro CLI v1.0.0 includes several improvements to the local development experience:

- You can now run `astrocloud dev start` with [Docker Buildkit](https://docs.docker.com/develop/develop-images/build_enhancements/) enabled. This resolves a [common issue](https://forum.astronomer.io/t/buildkit-not-supported-by-daemon-error-command-docker-build-t-airflow-astro-bcb837-airflow-latest-failed-failed-to-execute-cmd-exit-status-1/857) where users with Docker Buildkit enabled could not run this command.
- After running `astrocloud dev start`, the CLI no shows you the status of the Webserver container as it spins up on your local machine. This makes it easier to know whether the Airflow UI is unavailable because the Airflow Webserver container is still spinning up.

### Additional Improvements

- `astrocloud deploy` now shows a list of your Deployments in the order by which they were created instead of at random.

## v1.0.4 (`astro`)

Release date: December 9, 2021

### Improved Example DAGs

The Astro CLI is built to enable developers to learn about, test, automate, and make the most of Apache Airflow both locally and on Astro. To that end, we've updated the CLI with two example DAGs that will be present for all users in the `/dags` folder that is automatically generated by `astro dev init`.

The file names are:
- `example-dag-basic.py`
- `example-dag-advanced.py`

The basic DAG showcases a simple ETL data pipeline and the advanced DAG showcases a series of more powerful Airflow features, including the TaskFlow API, jinja templating, branching and more. Both DAGs can be deleted at any time.

### Bug Fixes

Fixed a broken documentation link and outdated description in the `airflow_settings.yaml` file, which you can use to programmatically set Airflow Connections, Variables, and Pools locally.

## v1.0.3 (`astro`)

Release date: November 5, 2021

- Bug Fix: Fixed an issue where users saw errors related to S3 in Webserver logs when running locally (e.g. `Failed to verify remote log exists s3:///`).

## v1.0.2 (`astro`)

Release date: October 25, 2021

- Improved help text throughout the CLI

## v1.0.1 (`astro`)

Release date: October 15, 2021

- This release contains changes exclusively related to the Astro CLI developer experience.

## v1.0.0 (`astro`)

Release date: September 28, 2021

- Improvement: `astro dev init` now always pulls the latest version of Astro Runtime for new projects. This means that you no longer have to upgrade the CLI in order to take advantage of a new Runtime release. Note that you still need to manually [upgrade Runtime](upgrade-runtime.md) for existing projects.
- Improvement: Updated error messages throughout the CLI to be more clear and useful

## v0.2.9-beta (`astro`)

Release date: September 20, 2021

- Improvement: Bumped the default Astro Runtime version for new projects to [`3.0.2`](runtime-release-notes.md#astro-runtime-302)
- Improvement: You can now use `astro dev run` to run Airflow CLI commands
- Improvement: You can now use `astro dev logs` to show logs for the Airflow Scheduler and Webserver when developing locally

## v0.2.8-beta (`astro`)

Release date: August 31, 2021

- Improvement: Bumped the default Astro Runtime version for new projects to [`3.0.0`](runtime-release-notes.md#astro-runtime-300)
- Improvement: Updated help text throughout the CLI
- Improvement: Projects created with `astro dev init` now include a README file

## v0.2.7-beta (`astro`)

Release date: July 31, 2021

- Bug Fix: Fixed an issue where users could not push DAGs to Deployments on Astro via the CLI.

## v0.2.6-beta (`astro`)

Release date: July 30, 2021

- Improvement: You can now run `astro auth login` without specifying a domain (`astronomer.io` is always assumed).