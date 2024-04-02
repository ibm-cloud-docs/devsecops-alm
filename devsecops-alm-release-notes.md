---

copyright:
  years: 2024
lastupdated: "2024-03-22"

keywords: devsecops-alm, deployment guide, deployable architecture, release notes

subcollection: devsecops-alm

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes for DevSecOps Application Lifecycle Management
{: #devsecops-alm-relnotes}

Use these release notes to learn about the latest updates to the DevSecOps Application Lifecycle Management deployable architecture. The entries are grouped by date.
{: shortdesc}

To find the release notes for the DevSecOps compliance pipeline definitions that are used by this architecture, see [Release notes for DevSecOps](/docs/devsecops?topic=devsecops-release-notes).



## 22 March 2024
{: #devsecops-alm-march2024}
{: release-note}

Version 1.1.0 of DevSecOps Application Lifecycle Management released
:   Version 1.1.0 of the DevSecOps Application Lifecycle Management is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

   - CRN support added for Secrets Manager. Set `sm_instance_crn` with the CRN of the Secrets Manager instance. At minimum two CRN secrets, need to be provided. Set `pipeline_ibmcloud_api_key_secret_crn` with the CRN for the apikey that runs the pipelines and set `ci_signing_key_secret_crn` with the CRN for the signing key.

   - Support added for GoSec scans. Set `opt_in_gosec` to `1` to enable GoSec scans. See [DevsecOps Docs](https://cloud.ibm.com/docs/devsecops?topic=devsecops-devsecops-image-verify){: external}

   - Support for using a Git tag to select a version of the Compliance Pipelines definitions in the pipeline definitions section. The recommendation is to use the current settings, which automatically use the latest version of the Compliance Pipelines. To specifiy a tag use `pipeline_git_tag`.

   - Support for enabling artifact validation. See [DevsecOps Docs](https://cloud.ibm.com/docs/devsecops?topic=devsecops-cd-devsecops-ci-pipeline#devsecops-ci-pipeline-gosec-add){: external}
     The expected signing certificate secret name is `signing-certificate`. If this is already set in the secrets provider under a different secret name, it can be updated by setting `cd_code_signing_cert_secret_name` to match the existing secret entry name.


If you are upgrading from version `1.0.7` to `1.1.0` select the variant that corresponds to your current configuration. If `print-code-signing-certificate` is set in the CI pipeline properties, delete the entry before continuing. Likewise for `code-signing-cert` in the CD pipeline. These will cause an update failure if present. If a failure occurs, ensure those pipeline properties have been removed and then try again.
{: note}

## 11 December 2023
{: #devsecops-alm-december2023}
{: release-note}

Version 1.0.7 of DevSecOps Application Lifecycle Management released
:   Version 1.0.7 of the DevSecOps Application Lifecycle Management is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

   - You can now deploy a sample application with {{site.data.keyword.codeenginefull_notm}}.

   - Support added in the CC toolchain for auto remediation of vulnerabilities using CRA for supported application types.

   - DevSecOps Application Lifecycle Management now uses the `open-v10` version of the compliance pipelines. Upgrading from `1.0.6` to `1.0.7` in {{site.data.keyword.cloud_notm}} projects will also update the version to `open-v10`. You can explicitly change the version by using the `compliance_pipeline_branch` variable.

   - Support added for SonarQube.

   - A validation trigger has been added to the CD pipeline for validating Git promotions.

  If you are upgrading from version `1.0.6` to `1.0.7` select the `Deploy to Kubernetes` option of `1.0.7`
  {: note}

## 02 October 2023
{: #devsecops-alm-october2023}
{: release-note}

Version 1.0.6 of DevSecOps Application Lifecycle Management released
:   Version 1.0.6 of the DevSecOps Application Lifecycle Management is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

   - The {{site.data.keyword.IBM_notm}} addition of new Security and Compliance Center profile support.

   - The {{site.data.keyword.en_full_notm}} support for multiple secret groups when using Secrets Manager.

   - For more information, see [Mandatory and optional variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars). New SCC variables are prefixed with `scc_` and secret group variables are suffixed with `_secret_group`.

## 08 September 2023
{: #devsecops-alm-september2023}
{: release-note}

Version 1.0.5 of DevSecOps Application Lifecycle Management released
:   Version 1.0.5 of the DevSecOps Application Lifecycle Management is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

   - The {{site.data.keyword.IBM_notm}} Managed Compliance Pipelines definitions can be set across all pipelines by using `compliance_pipeline_branch` or on a pipeline basis by using `ci_compliance_pipeline_branch`, `ci_compliance_pipeline_pr_branch`, `cd_compliance_pipeline_branch`, or `cc_compliance_pipeline_branch`. The default value is `open-v9`.

   - The {{site.data.keyword.en_full_notm}} tool can be added to CI, CD, and CC toolchains by using `event_notifications_crn` with the CRN of the existing {{site.data.keyword.en_short}} service or to a specific toolchain by using `ci_event_notifications_crn`, `cd_event_notifications_crn`, or `cc_event_notifications_crn`.

   - Some variable names were updated for consistency. `evidence_repo_url` is now `evidence_repo_existing_url`. `issues_repo_url` is now `issues_repo_existing_url`. `inventory_repo_url` is now `inventory_repo_existingurl`. Old and new variable names work but code should be updated to use the new variable names.

   - Triggers can now be named, enabled, and disabled. For example, `ci_trigger_manual_name` and `ci_trigger_manual_enable`. For more information, see [Mandatory and optional variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars).

Version 1.0.5 addresses a potential issue whereby the default Sonarqube scan fails to run and instead attempts to connect to a custom server. The issue can be remedied in the 1.0.4 release by deleting the text parameter `sonarqube` from both the CI and CC pipeline properties. This was set to `{}` previously.
{: note}

## 26 May 2023
{: #devsecops-alm-may2023}
{: release-note}

Version 1.0.4 of DevSecOps Application Lifecycle Management released
:   The new version supports a simplified setup. It introduces a number of additional variables that act as group variables. For more information, see [Mandatory and optional variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars). For example, the `toolchain_region` variable now set the region for the CI, CD, CC toolchains, as well as the cluster region and container registry region.

## 20 April 2023
{: #devsecops-alm-apr2023}
{: release-note}

Introducing DevSecOps Application Lifecycle Management
:   The DevSecOps Application Lifecycle Management is [released](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-overview). You can use the deployable architecture to create a set of DevOps toolchains and pipelines. The [deployable architecture](#x10293733){: term} is based on the {{site.data.keyword.cloud_notm}} for Financial Services reference architecture.
