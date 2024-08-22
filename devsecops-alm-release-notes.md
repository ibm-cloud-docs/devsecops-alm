---

copyright:
  years: 2024
lastupdated: "2024-06-24"

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

## 21 June 2024
{: #devsecops-alm-june2024}
{: release-note}

Version 1.8.0 of DevSecOps Application Lifecycle Management released
:   Version 1.8.0 of the DevSecOps Application Lifecycle Management is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

This version surfaces additional functionality from the DevSecOps pipelines including options for modifying the behavior of CRA scans and Event Notifications.
`ci_cra_bom_generate`,
`ci_cra_deploy_analysis`,
`ci_cra_vulnerability_scan`,
`pr_cra_bom_generate`,
`pr_cra_deploy_analysis`,
`pr_cra_vulnerability_scan`,
`ci_enable_pipeline_notifications`,
`ci_event_notifications`,
`ci_pipeline_properties`,
`ci_repository_properties`,
`cd_enable_pipeline_notifications`,
`cd_event_notifications`,
`cd_pipeline_properties`,
`cc_cra_bom_generate`,
`cc_cra_deploy_analysis`,
`cc_cra_vulnerability_scan`,
`cc_enable_pipeline_notifications`,
`cc_event_notifications`,
`cc_pipeline_properties`

For more information, see [Variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars)

This version introduces a new customization feature. The variables `ci_pipeline_properties`, `cd_pipeline_properties` and `cc_pipeline_properties` allow the user to add custom properties to the CI, CD and CC pipelines using the relevant prefixed variables.

The following example uses the `ci_pipeline_properties` which targets the the CI toolchain. Note the `pipeline_id` lines in the JSON which has `ci` and `pr` values and denotes the CI and PR pipelines. When using the `cd_pipeline_properties` and `cc_pipeline_properties` variables, using `cd` and `cc` as the values will target the respective pipelines to add the new variables. The example highlights the addition of custom text, secret and enum pipeline properties. Note that for the secure type property, it is enough to specify only the name of the secret in the default Secrets Manager instance. Alterantively the full secret reference can be provided, for example `{vault::sm-compliance-secrets.custom-secret-group.secret-name}`

```
[
  {
    "pipeline_id": "ci",
    "properties": [
      {
        "name": "custom-param1",
        "type": "text",
        "value": "example1"
      },
      {
        "name": "custom-param2",
        "type": "secure",
        "value": "grit-token"
      },
      {
        "name": "custom-param3",
        "type": "single_select",
        "value": "0",
        "enum": "[0,1,2]"
      }
    ]
  },
  {
    "pipeline_id": "pr",
    "properties": [
      {
        "name": "custom-param1",
        "type": "text",
        "value": "example1"
      }
    ]
  }
]

```

The CI toolchain additionally has a variable called `ci_repository_properties` that allows the user to add new or existing repositories to the CI toolchain. The default behavior is that listed repositories will treat the repositories as existing rather then attempt to clone those repositories.

The following example highlights how to add additional repositories to the CI toolchain. Note that the top level entries in the JSON are inherited by the child elements and can be overridden by entries in the child elements with the same key name. Two repositories are listed in the example. The first repository, is the most basic case. It contains a `repository_url` and `default_branch`. The `default_branch` overrides the `master` value with `main`. The default `mode` is `link`. Since the repository is on GitHub, it requires Git token. This is specified in the top level of the JSON. This value needs to be in the Secrets Provider. Specifying the repository as such will result in triggers automatically being created for the repository, namely a Git Trigger, a manual trigger and a PR trigger. 

The second listed repository is in `clone` mode and will attempt to clone the repository into GRIT. If a `name` is not provided for the repository, it will use the same name as the repository being cloned. 

This second example also shows the creation of custom triggers. It should be noted that add custom triggers will prevent the creation of the automatic triggers.

```
[
  {
    "pipeline_id": "ci",
    "repository_owner": "owner_name",
    "default_branch": "master",
    "repositories": [
      {
        "repository_url": "https://github.com/open-toolchain/secure-kube-toolchain",
        "default_branch": "main",
         "git_token_secret_ref": "github-token",
      },
      {
        "repository_url": "https://github.com/open-toolchain/hello-containers",
        "mode": "clone",
        "name": "clone-of-hello-containers",
        "triggers": [
          {
            "type": "manual",
            "name": "Manual Trigger1",
            "properties": [
              {
                "type": "text",
                "name": "param1",
                "value": "example1"
              }
            ]
          }
        ]
      }
    ]
  }
]

```

## 15 May 2024
{: #devsecops-alm-may2024}
{: release-note}

Version 1.2.1 of DevSecOps Application Lifecycle Management released
:   Version 1.2.1 of the DevSecOps Application Lifecycle Management is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

   - Additional options to configure the name of the Code Engine Projects in the Code Engine Variant, `code_engine_project`, `code_engine_project_prefix`, `ci_code_engine_project_prefix` and `cd_code_engine_project_prefix`. 

For more information, see [Variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars)

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
