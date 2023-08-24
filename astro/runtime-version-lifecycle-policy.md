---
title: "Astro Runtime maintenance and security"
sidebar_label: "Astro Runtime maintenance and security"
id: runtime-version-lifecycle-policy
---

<head>
  <meta name="description" content="Learn how Astronomer releases and maintains Astro Runtime. Astro Runtime is a Docker image built by Astronomer that provides a differentiated Apache Airflow experience and execution framework." />
  <meta name="og:description" content="Learn how Astronomer releases and maintains Astro Runtime. Astro Runtime is a Docker image built by Astronomer that provides a differentiated Apache Airflow experience and execution framework." />
</head>

Astro Runtime is required by all Astronomer products and is maintained by Astronomer. It improves your Airflow environment's functionality, reliability, efficiency, and performance.

Astronomer maintenance and lifecycle policies are part of the Astro Runtime distribution and define the period that specific versions are supported and how frequently updates are provided.

## Versioning

Astro Runtime versions are released regularly and use [semantic versioning](https://semver.org/). Astronomer ships major, minor, and patch releases of Astro Runtime in the format of `major.minor.patch`.

- **Major** versions are released for significant feature additions. This includes new major or minor versions of Apache Airflow, as well as API or DAG specification changes that are not backward compatible.
- **Minor** versions are released for functional changes. This includes API or DAG specification changes that are backward compatible, which might include new minor versions of `astronomer-providers` and `openlineage-airflow`.
- **Patch** versions are released for bug and security fixes that resolve unwanted behavior. This includes new patch versions of Apache Airflow, `astronomer-providers`, and `openlineage-airflow`.

### Astro Runtime, Airflow and Python version mapping

To use a different version of Airflow, refer to the following table to choose the correct Astro Runtime version. Refer to the [Astro Runtime release notes](runtime-release-notes.md) for all available versions.

| Astro Runtime | Python version | Airflow version |
| :-----------: | :------------: | :-------------: |
|       4       |      3.9       |       2.2       |
|       5       |      3.9       |       2.3       |
|       6       |      3.9       |       2.4       |
|       7       |      3.9       |       2.5       |
|       8       |      3.10      |       2.6       |
|       9       |      3.11      |       2.7       |

### Provider package versioning

If an Astro Runtime release includes changes to an installed version of a provider package that is maintained by Astronomer (`astronomer-providers` or `astro-sdk`), the version change is documented in the [Astro Runtime release notes](runtime-release-notes.md).

To determine the version of any provider package installed in your current Astro Runtime image, run:

```bash
astro dev bash --scheduler | pip freeze | grep <provider>
```

## Release channels

To meet the unique needs of different operating environments, Astro Runtime versions are associated with the following release channels:

- **Stable:** Includes the latest Astronomer and Apache Airflow features, available on release
- **Long-term Support (LTS):** Includes additional testing, stability, and maintenance for a core set of features

Each major Astro Runtime version is associated with an Astro Runtime stable release channel. The LTS release channel is a subset of the stable release channel and includes additional stability, reliability, and support.

For users that want to keep up with the latest Astronomer and Airflow features on an incremental basis, we recommend upgrading to new versions of Astro Runtime as soon as they are made generally available. This should be regardless of release channel. New versions of Runtime are issued regularly and include timely support for the latest major, minor, and patch versions of Airflow.

For customers looking for less frequent upgrades and functional changes, we recommend following the LTS release channel exclusively.

## Maintenance policy

The maintenance period for an Astro Runtime version depends on its release channel:

| Release Channel | Maintenance Duration                                                            |
| --------------- | ------------------------------------------------------------------------------- |
| Stable          | 6 months or 3 months after the next major Astro Runtime release, whichever is longer |
| LTS             | 18 months or 6 months after the next LTS Astro Runtime release, whichever is longer  |

For each major Runtime version, only the latest `minor.patch` version is supported at any given time. If you report an issue with an Astro Runtime version that is not latest, the Astronomer Support team will always ask that you upgrade as a first step to resolution. For example, any user who reports an issue with Astro Runtime 4.0.2 will be asked to upgrade to the latest 4.x.y version as soon as it's generally available.

Within the maintenance window of each Astro Runtime version, the following is true:

- A set of Docker images corresponding to that version are available for download on [Quay.io](https://quay.io/repository/astronomer/astro-runtime?tab=tags) and PyPi.
- Astronomer will regularly publish bug or security fixes identified as high priority.
- Support for paying customers running a maintained version of Astro Runtime is provided by [Astronomer Support](https://cloud.astronomer.io/support).
- A user can create a new Deployment with the Cloud UI, API, or Astro CLI with any supported `major.minor` version pair of Runtime. For new Deployments, the Cloud UI assumes the latest patch.

When the maintenance window for a given version of Runtime ends, the following is true:

- Astronomer is not obligated to answer questions regarding a Deployment that is running an unsupported version.
- New Deployments cannot be created on Astro with that version of Runtime. Versions that are no longer maintained will not render as an option in the Deployment creation process from the Cloud UI, API, or Astro CLI.
- The Deployment view of the Cloud UI will show a warning that encourages the user to upgrade if the Deployment is running that version.
- The latest version of the Astro CLI will show a warning if a user pushes an Astro Runtime image to Astronomer that corresponds to that version.

Astronomer will not interrupt service for Deployments running Astro Runtime versions that are no longer in maintenance. Unsupported versions of Astro Runtime are available for local development and testing with the Astro CLI.

#### End of maintenance date

Maintenance is discontinued the last day of the month for a given version. For example, if the maintenance window for a version of Astro Runtime is January - June of a given year, that version will be maintained by Astronomer until the last day of June.

## Lifecycle schedule

<!--- Version-specific -->

The following table contains the exact lifecycle for each published version of Astro Runtime. These timelines are based on the LTS and Stable release channel maintenance policies.

| Runtime Version   | Apache Airflow version | Release Date | End of Maintenance Date | LTS Support |
|:-----------------------------------------------:|:----------------------:| ------------------ | ----------------------- |-------------|
| [4](runtime-release-notes.md#astro-runtime-420) | 2.2                    | March 10, 2022     | September 2023          | ✓           |
| [5](runtime-release-notes.md#astro-runtime-500) | 2.3                    | April 30, 2022     | October 2023            | ✓           |
| [6](runtime-release-notes.md#astro-runtime-600) | 2.4                    | September 19, 2022 | March 2024              | ✓           |
| [7](runtime-release-notes.md#astro-runtime-700) | 2.5                    | December 3, 2022   | July 2023               |             |
| [8](runtime-release-notes.md#astro-runtime-800) | 2.6                    | April 30, 2023     | October 2023            |             |
| [9](runtime-release-notes.md#astro-runtime-900) | 2.7                    | August 18, 2023    | January 2025            | ✓           |

If you have any questions or concerns, contact [Astronomer support](https://cloud.astronomer.io/support).

## Security

Astronomer continuously checks for available security fixes for software used in Astro Runtime. This process includes scanning language dependencies, container images, and open source threat intelligence sources. When a security fix is available, Astronomer evaluates potential risks for organizations using Astro Runtime and determines deployment priority. Low priority fixes are deployed following the regular maintenance policy.

If a vulnerability is not yet addressed in a third-party dependency and no official fix is available, Astronomer attempts to address the vulnerability or its impact with environmental mitigations. Whenever possible, Astronomer collaborates with the upstream project to support a timely delivery of the official fix. This process also covers images publicly available on [Quay.io](https://quay.io/repository/astronomer/astro-runtime?tab=tags) and provides context for their vulnerability scanning results.

If you identify a vulnerability that results in relevant risk for your organization, contact [Astronomer security](mailto:security@astronomer.io).

### Backport policy for bug and security fixes

- **Functional bugs:** When Astronomer identifies a significant functional bug in Astro Runtime, a fix is backported to all Long Term Support (LTS) versions and the latest stable version. To avoid the impact of previously identified bugs, Astronomer recommends that you consistently upgrade Astro Runtime to the latest stable version.

- **Security vulnerabilities:** When Astronomer identifies a significant security vulnerability in Astro Runtime, a fix is backported and made available as a patch version for all stable and LTS versions in maintenance. A significant security issue is defined as an issue with significant impact and exploitability.

Occasionally, Astronomer might deviate from the defined response policy and backport a bug or security fix to releases other than the latest stable and LTS versions. To request a fix for a specific bug, contact your customer success manager.


## System distribution

The following table lists the operating systems and architectures supported by each Astro Runtime version. If you're using a Mac computer with an M1 chip, Astronomer recommends using Astro Runtime 6.0.4 or later.

| Astro Runtime | Operating System (OS)  | Architecture    |
| ------------- | ---------------------- | --------------- |
| 4             | Debian 11.3 (bullseye) | AMD64           |
| 5             | Debian 11.3 (bullseye) | AMD64           |
| 6             | Debian 11.3 (bullseye) | AMD64 and ARM64 |
| 7             | Debian 11.3 (bullseye) | AMD64 and ARM64 |
| 8             | Debian 11.3 (bullseye) | AMD64 and ARM64 |

Astro Runtime 6.0.4 and later images are multi-arch and support AMD64 and ARM64 processor architectures for local development. Docker automatically uses the correct processor architecture based on the computer you are using. To use an architecture different from your current machine, use the flag `--platform` in your `Dockerfile`:

```bash
FROM --platform=linux/amd64 quay.io/astronomer/astro-runtime:8.6.0
```

## See also

- [Astro Runtime overview](runtime-overview.md)
- [Astro Runtime release notes](runtime-release-notes.md)
- [Choose Runtime version](develop-project.md#choosing-an-astro-runtimeairflow-version)
