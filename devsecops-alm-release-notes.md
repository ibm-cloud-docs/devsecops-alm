---

copyright:
  years: 2024
lastupdated: "2024-12-09"

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

## 04 April 2025
{: #devsecops-alm-april2025}
{: release-note}

Deploying DevSecOps toolchains in a firewalled environment
:   DevSecOps Application Lifecycle Management supports deploying [DevSecOps toolchains in a firewalled environment](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-firewall-req).

Deploying DevSecOps Solution for Apps Stack
:   DevSecOps Application Lifecycle Management [preparing](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-stack-req) and [deploying](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-deploy-stack-req) the DevSecOps Solutions for Apps Stack.

## 24 March 2025
{: #devsecops-alm-march2025}
{: release-note}

Version 2.7.2 of DevSecOps Application Lifecycle Management released
:   Version 2.7.2 of the DevSecOps Application Lifecycle Management is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

This version extends upon the previously released air gapped support for the repository tool integrations by adding support for the sample appplication repository tool to be independently setup in an air gapped configuration.

As before the following variables are used to configure an air gapped setup.
`repo_blind_connection`,
`repo_git_id`,
`repo_git_provider`,
`repo_root_url`,
`repo_title`

Note: `repo_blind_connection` is now a bool as opposed to a string type previously.

Three types of configurations are supported:

1) Fully air gapped. All repositories are configured with a blind connection. 
To configure all the default repositories with air gap support, the following variables must be set. Set `compliance_pipeline_repo_use_group_settings` to `true`, `repo_apply_settings_to_compliance_repos` to `true` and `create_git_triggers` to `false`. See the variable descriptions for more details. 

2) Fully air gapped with the exception of the compliance-pipelines repository. Set `compliance_pipeline_repo_use_group_settings` to `false`, `repo_apply_settings_to_compliance_repos` to `true` and `create_git_triggers` to `false`.

3) Partially air gapped. Only the sample apps, config repos and the deployment repos are air gapped. Set `compliance_pipeline_repo_use_group_settings` to `false`, `repo_apply_settings_to_compliance_repos` to `false` and `create_git_triggers` to `false`.

Service API Key and API Key creation
:   The `ibmcloud-api-key` and the `cos-api-key` can now be configured in different ways.
For the `ibmcloud-api-key` setting `create_ibmcloud_api_key` to `true` by default creates a service api key with the relevant permissions to run the pipelines. As a side effect the repository tool integrations must be configured using a Git personal access token. See `repo_git_token_secret_name` for more details. Alternatively setting `force_create_standard_api_key` to `true` will instead create a standard apikey and access is scoped to the access group that the user has been assigned.

 Likewise setting `create_cos_api_key` to `true` and setting a name for the cos-api-key using `cos_api_key_secret_name` will create a service api key with the relevant permissions for the running pipeline to interact with the configured cos bucket. The `cos-api-key` additionally requires the `cos_instance_crn` and the `cos_bucket_name` to be set, to correctly apply the access policies to the COS service api key. 

 Lastly both the `ibmcloud-api-key` and the `cos-api-key` api keys can be specified using `pipeline_ibmcloud_api_key_secret_value` and `cos_api_key_secret_value` respectfully. See the variable descriptions for more details.

Access group support
:   An access group can be created by setting `create_access_group` to `true`. The name of the access group can be set using `toolchain_access_group_name`. Users can be assigned to this access group, granting the relevant permissions for toolchain operations. 

Note: `create_code_engine_access_policy` or `create_kubernetes_access_policy` can be set to `true` to cater to the polices for a Kubernetes or a Code Engine deployment type.

Secret references
:   By default secret references used by the pipelines and tool integrations use the legacy format and are identifiable by a purple color. Setting `use_legacy_ref` to `false` will use the new secret reference format identifiable by a teal color. Future releases will use the new format by default.

For more information, refer to [Variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars).

## 9 December 2024
{: #devsecops-alm-dec2024}
{: release-note}

Version 2.5.0 of DevSecOps Application Lifecycle Management released
:   Version 2.5.0 of the DevSecOps Application Lifecycle Management is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

Upgrading to 2.5.0

Upgrading from 2.4.3
:   When upgrading from version 2.4.3, set the `compliance_pipeline_repo_name` variable to `compliance-pipelines` to prevent the forced replacement of the compliance-pipelines tool integration.

Do not set this variable if upgrading from `2.0.3` to `2.5.0`.
{: important}

Upgrading from 2.0.3
:   During the upgrade process, an unused Terraform resource of type `random_string` is destroyed.

Fixed issues
:   The slack-notifications pipeline property is now correctly calculated when enabling the Slack tool integration.

New features
:   Support for a private worker tool integration is now available. The following variables are used to configure the private worker tool integration:
   * `privateworker_name`
   * `privateworker_credentials_secret_name`
   * `privateworker_secret_value`
   * `create_privateworker_secret`
   * `enable_privateworker`

Additional information
:  The individual CI, CD, and CC modules used by the ALM introduce a new variable that specifies the name of the compliance-pipelines repository. While this variable is not used in the default configuration, it may cause a forced replacement of the repository integration. Note that this does not affect the repository itself, only the tool integration.

Private Worker Configuration

:  To add a private worker to your set of toolchains:

   1. Set the `enable_privateworker` variable to `true` to add the worker tool integration to your toolchains.
   2. Set the `create_privateworker_secret` variable to `true` to create a secret in your Secrets Manager instance.
   3. Provide the service API key for your private worker in the `privateworker_secret_value` variable.
   4. Specify the secret name in the `privateworker_credentials_secret_name` variable where the service API key will be stored.

   An existing private worker services API key is required.
   {: note}


For more information, refer to [Variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars).

## 6 November 2024
{: #devsecops-alm-nov2024}
{: release-note}

Version 2.4.3 of DevSecOps Application Lifecycle Management released
:   Version 2.4.3 of the DevSecOps Application Lifecycle Management is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

The individual CI, CD and CC modules leveraged by the ALM introduce a new variable specifiying the name of the `compliance-pipelines` repository. The variable is not used in the default configuration but it causes a forced replacement of the repository integration. There is no deletion to the repository itself only the tool integration.
{: #note}

An unused Terraform resource of type `random_string` had been previously created using the default settings. You will see this being destroyed in the upgrade.
{: #note}

Configuration of the compliance repositories using a blind connection to support air gapped environments.

The following group level variables apply the settings against the default compliance repositories:
`repo_blind_connection`,
`repo_git_id`,
`repo_git_provider`,
`repo_root_url`,
`repo_title`

Set `repo_blind_connection` to `true` to enable a blind connection, set `repo_git_id` to the Git ID for the compliance repositories and `repo_git_provider` to the Git provider type for example `GitHub` or `GitLab`. `repo_root_url` should be set to the root URL of the server for example `https://git.example.com.` and `repo_title` with a title for the server. The repositories can also be individually configured with repository specific versions of the above variables.

Support to provision secrets in the Configured Secrets Manager instance, specifically a git token, and the signing keys. The variables that are required are as follows:

`create_git_token`
`repo_git_token_secret_name`
`repo_git_token_secret_value`
`create_signing_key`
`rotate_signing_key`
`ci_signing_key_secret_name`
`cd_code_signing_cert_secret_name`

Setting `create_git_token` to `true`, `repo_git_token_secret_name` to `my-git-token`  and specifying a Git personal access token in `repo_git_token_secret_value` will result in a secret that is called `my-git-token` with the value that is specified in `repo_git_token_secret_value` being created in Secrets Manager.
It is important to note that setting `repo_git_token_secret_value` changes the behavior of the authentication that is used by the repository tools. The repositories will now expect to use an access token and a username/owner for your repositories. This owner name needs to be set in `repo_group`. This behavior can be overridden using the auth type variable that is associated with each specific repository and reverted to `oauth` access. These variables end with `_repo_auth_type` for example `issues_repo_auth_type`

Setting `create_signing_key` to `true` generates a signing key and the corresponding signing validation certificate. The names for these secrets are specified in `ci_signing_key_secret_name` and `cd_code_signing_cert_secret_name`.

The generated signing secrets can be rotated by setting `rotate_signing_key` to true. This key will rotate both the signing key and signing validation certifcate. It is important to make a copy of the validation certificate in the event that there are pending deployments that were signed with the previous signing key.

This release also provides support for using GitLab as a source for the repositories and a simplified means of changing the Git Provider.
See `repo_git_provider`. Set this variable to `GitLab` to configure all the compliance repository tools created by the DA to use GitLab.

To use GitLab as a provider, an access token for GitLab and the user/owner name must be provided. The name of the access token can be set in `repo_git_token_secret_name`  which applies to all the compliance repositories and the repository owner name can be set using `repo_group`.

These can be individually configured using the variables ending with `_repo_git_token_secret_name` and `_group`. For example `issues_repo_git_token_secret_name` and `issues_group`


For more information, see [Variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars).

## 26 September 2024
{: #devsecops-alm-sept2024}
{: release-note}

Version 2.0.3 of the DevSecOps Application Lifecycle Management released
:   Version 2.0.3 of the DevSecOps Application Lifecycle Management is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.


**Simplified pipeline configuration**: All default and custom pipeline properties are now set using a single JSON variable for each pipeline type (CI, CD, and CC).

Changes and Improvements

- Streamlined pipeline configuration and management
- Improved usability with JSON variable names matching pipeline property names
- Reduced complexity with all pipeline properties in one place


This update is a breaking change. Update with caution. To ensure a smooth transition, follow these steps:

1. Record current pipeline properties and values for CI, CD, and CC toolchains.
1. Update JSON variables using the following templates:
    - For the Kubernetes flavor of the toolchains, use [CI JSON](https://github.com/terraform-ibm-modules/terraform-ibm-devsecops-alm/blob/main/solutions/kubernetes/ci-properties.json), [CD JSON](https://github.com/terraform-ibm-modules/terraform-ibm-devsecops-alm/blob/main/solutions/kubernetes/cd-properties.json) and the [CC JSON](https://github.com/terraform-ibm-modules/terraform-ibm-devsecops-alm/blob/main/solutions/kubernetes/cc-properties.json).
    - Alternatively for the Code Engine flavor, use [CI JSON](https://github.com/terraform-ibm-modules/terraform-ibm-devsecops-alm/blob/main/solutions/code-engine/ci-properties.json), [CD JSON](https://github.com/terraform-ibm-modules/terraform-ibm-devsecops-alm/blob/main/solutions/code-engine/cd-properties.json) and the [CC JSON](https://github.com/terraform-ibm-modules/terraform-ibm-devsecops-alm/blob/main/solutions/code-engine/cc-properties.json).
1. Add any previously specified custom properties to the updated JSON variables.
{: #note}

The most common variable type is the text property which takes the form:
```JSON
  {
    "name": "opt-in-dynamic-scan",
    "type": "text",
    "value": "1"
  }
```

Secrets are set as follows:

```JSON

  {
    "name": "git-token",
    "type": "secure",
    "value": "my-github-token"
  }

```

You can set the value to a secure type in one of the following three ways:

**Secret name**: Use the name of the secret as it appears in the secrets provider. The full reference to the secret is automatically calculated based on the Secrets Manager and Secret group specified during DA creation.
**CRN**: Set a Cloud Resource Name (CRN) to a secret. This requires a Secrets Manager integration configured in CRN mode.
**Full secret reference**: Set the full secret reference, for example {vault::sm-compliance-secrets.Default.my-github-token}. This approach allows you to specify different secret groups.

There are several existing variables that now operate in tandem with the JSON preoperties. These are as follows:
`app_repo_branch`,
`cluster_name`,
`cluster_namespace`,
`cluster_region`,
`code_engine_project`,
`code_engine_region`,
`code_engine_resource_group`,
`code_signing_cert_secret_name`,
`cos_api_key_secret_name`,
`cos_bucket_name`,
`cos_endpoint`,
`dev_region`,
`dev_resource_group`,
`environment_tag`,
`pipeline_doi_api_key_secret_name `,
`doi_toolchain_id`,
`doi_toolchain_id_pipeline_property`,
`pipeline_ibmcloud_api_key_secret_name`,
`region`,
`registry_namespace`,
`registry_region`,
`signing_key_secret_name`,
`pipeline_config_repo_branch`

The purpose of these variables is to allow an alternate way to specify the value of a pipeline property. They are helper variables. Take for example the variables that end with `region`. These will automatically be set with the region matching the `toolchain_region` variable. The `cluster_region` can be explicitly set or it will inherit the value set in `toolchain_region` .As such the equivalent pipeline property in the JSON can be left empty but it still needs to be specifed for the pipeline property to be created. Setting the value in the JSON has the highest precedence and when set it will override any values set in the variables above.

```JSON
  {
    "name": "cluster-region",
    "type": "text",
    "value": ""
  }
```
Another special case is the `doi_toolchain_id`. Unless you know beforehand that you would like the DevOpInsights integration to point to a specific toolchain that already exists, leave this entry empty and the DA will automatically provide the ID for you.

There is the possibility that the upgrade will fail and this is likely due to adding pipeline properties to the pipelines manually via the UI rather than through Projects. The likely error in this case that is seen in the logs will be `duplicate property error`. To resolve this issue, you will have to remove the problematic property in the UI and then re-run the Projects deploy step of the DA. If the property is not easily identifiable then delete all the pipeline properties apart from the repository tool integration properties.

If the property does not appear in the JSON then it will not appear in the pipeline properties after the deployment. As long as the property has an empty value set, then you can use the above listed variables if required.
{: #note}

Version 2.0.3 has two other significant changes:

*  There is a significant reduction in the number of variables available for setup. Roughly a 60% reduction. This is from a combination of using the pipeline property JSONs and adding more group level variables and hiding their related variables. For example `cos_bucket_name` sets the cos bucket name across all three toolchains and the related variables of `ci_cos_bucket_name`, `cd_cos_bucket_name` and `cc_cos_bucket_name` are now absent.

*  Locked properties: This is a new feature designed to limit the control for users that only have permission to run the pipelines. When the property is locked, it will not be available to edit when running a pipeline. Several properties are now locked by default. To unlock these properties, identify the required property in the properties JSON and add the following line:

```JSON
{
  "name": "doi-ibmcloud-api-key",
  "type": "secure",
  "value": "ibmcloud-api-key",
  "locked": "false"
}
```

Minor changes

*  The original default name for the `ci_signing_key_secret_name` has been updated to `signing-key`.
*  CRA Auto-Remediation is now on by default.
*  The set up for bringing your own sample app has been simplified and for cases where the sample app resides in Git or the IBM hosted Git Repos and Issue tracking, it will automatically calculate the provider setting for you. They can still be explicitly set. The require variables are now only `app_repo_existing_url`, `app_repo_branch` and `app_repo_git_token_secret_name` if a Git token is required to access the repository.

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
{: #note}

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
