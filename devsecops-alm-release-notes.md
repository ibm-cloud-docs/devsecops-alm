---

copyright:
  years: 2023
lastupdated: "2023-10-05"

keywords: devsecops-alm, deployment guide, deployable architecture, release notes

subcollection: devsecops-alm

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes for DevSecOps Application Lifecycle Management
{: #devsecops-alm-relnotes}

Use these release notes to learn about the latest updates to the DevSecOps Application Lifecycle Management deployable architecture. The entries are grouped by date.
{: shortdesc}


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

Note:
Version 1.0.5 addresses a potential issue whereby the default Sonarqube scan fails to run and instead attempts to connect to a custom server. The issue can be remedied in the 1.0.4 release by deleting the text parameter `sonarqube` from both the CI and CC pipeline properties. This was set to `{}` previously.

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
