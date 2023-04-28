---

copyright:
  years: 2023
lastupdated: "2023-04-28"

keywords: devsecops-alm, deployment guide, deployable architecture

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# DevSecOps Application Lifecycle Management mandatory and optional variables
{: #devsecops-alm-vars}

Use the required variables to create toolchains for the out of the box experience. If required, use the optional variables to make other adjustments to your toolchains. 
{: shortdesc}

## Required input variables
{: #devsecops-alm-min}

The required setup variables to create toolchains for the out of the box experience are included in the following table. Change the default values to reflect the details of your own account. 

You must specify all the CI, CD, and CC variables. However, if you do not want to create a toolchain you can turn off the flag that creates the toolchain by using the variables in Table 2.

The variables prefixed with `ci`, `cd` and `cc` apply to the CI, CD, and CC toolchains. Nonprefixed variables apply to all the toolchains. Also, modifying the default values for the prefixed variable inputs causes the prefixed variables inputs to take precedence over nonprefixed variable inputs. 
{: note}

| Parameter | Description |  Default | Values |
|------|-------------|------|--------|
|`ibmcloud_api_key`|API key used to create the toolchains. |  |
|`sm_location`|The location of the Secrets Manager instance.|`us-south`|`us-south` or `eu-gb`.  |
|`sm_name`|The name of an existing Secret Managers instance.|`sm-instance`|  |
|`sm_resource_group`|The resource group that contains the Secrets Manager instance. |`Default`|  |
|`sm_secret_group`|Group in Secrets Manager for organizing or grouping secrets.|`Default`|  |
|`toolchain_region`|{{site.data.keyword.cloud}} region where your toolchain is created.|`us-south`| `au-syd`, `eu-de`, `eu-fr2`, `eu-gb`, `jp-tok`, `jp-osa`, `ca-tor`, `br-sao`, `us-east`, or `us-south`. |
|`toolchain_resource_group`|The resource group within which the toolchain is created.|`Default`|  |
{: caption="Table 1. API key, secrets, and toolchain" caption-side="top"}
{: #alm-other-variables}
{: tab-title="API key, secrets, and toolchain"}
{: tab-group="mandatory"}
{: class="simple-tab-table"}

| Parameter | Description |  Default | Values |
|------|-------------|------|--------|
|`ci_cluster_name`|Name of the Kubernetes cluster where the application is deployed. (Can be the same cluster that is used for prod)|`mycluster-free`|  |
|`ci_cluster_namespace`|Name of the Kubernetes cluster namespace where the application is deployed.|`dev`|   |
|`ci_dev_region`|Region of the Kubernetes cluster where the application is deployed.|`ibm:yp:us-south`|  |
|`ci_dev_resource_group`|The cluster resource group. |`Default`|  |
|`ci_registry_namespace`|A unique namespace within the {{site.data.keyword.cloud}} Container Registry region where the application image is stored.|  |
|`ci_registry_region`|The {{site.data.keyword.cloud}} Region where the {{site.data.keyword.cloud}} Container Registry namespace is to be created.|`ibm:yp:us-south`|  |
|`ci_toolchain_name`|The name of the CI toolchain. |`DevSecOps CI Toolchain - Terraform`|  |
{: caption="Table 1. Continuous integration" caption-side="bottom"}
{: #alm-ci-variables}
{: tab-title="Continuous integration"}
{: tab-group="mandatory"}
{: class="simple-tab-table"}


| Parameter | Description |  Default | Values |
|------|-------------|------|--------|
|`cd_cluster_name`|Name of the Kubernetes cluster where the application is deployed. |`mycluster-free`|  |
|`cd_cluster_namespace`|Name of the Kubernetes cluster namespace where the application is deployed.|`prod`|  |
|`cd_cluster_region`|Region of the Kubernetes cluster where the application is deployed.|`ibm:yp:us-south`|  |
|`cd_toolchain_name`|The name of the CD toolchain. |`DevSecOps CD toolchain - Terraform`|  |
{: caption="Table 1. Continuous deployment" caption-side="bottom"}
{: #alm-cd-variables}
{: tab-title="Continuous deployment"}
{: tab-group="mandatory"}
{: class="simple-tab-table"}

| Parameter | Description |  Default | Values |
|------|-------------|------|--------|
|`cc_toolchain_name`|The name of the CC toolchain. |`DevSecOps CC Toolchain - Terraform`|  |
{: caption="Table 1. Continuous compliance" caption-side="bottom"}
{: #alm-cc-variables}
{: tab-title="Continuous compliance"}
{: tab-group="mandatory"}
{: class="simple-tab-table"}

## Optional input variables
{: #devsecops-alm-opt}

Use the optional setup variables in Tables 5 to 19 to extend the out of the box experience. For example, if you want to integrate Slack notifications to your toolchains. 

### Toolchain creation variables
{: #devsecops-alm-tccreate}

The following variables determine which toolchains are created. By default all three are set to `true`, which creates the DevSecOps CI, CD and CC toolchains. Any combination of the three toolchains can be created. 

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `create_ci_toolchain` | Determines whether the DevSecOps CI toolchain is created. If this toolchain is not created, then values must be set for the following variables: `evidence_repo_url`, `issues_repo_url`, and `inventory_repo_url`. | `bool` | `true` |
| `create_cd_toolchain` | Boolean flag that determines whether the DevSecOps CD toolchain is created. | `bool` | `true` |
| `create_cc_toolchain` | Boolean flag that determines whether the DevSecOps CC toolchain is created. | `bool` | `true` |
{: caption="Table 2. toolchain creation" caption-side="top"}

### Compliance repositories
{: #devsecops-alm-compl}

If the CI toolchain is not created, you must set the following variables.

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `evidence_repo_url`  | A template repository to clone compliance-evidence-locker for reference DevSecOps toolchain templates. | `string` | `""` |
| `issues_repo_url` | A template repository to clone `compliance-issues` for reference DevSecOps toolchain templates. | `string` | `""` |
| `inventory_repo_url` | A template repository to clone compliance-inventory for reference DevSecOps toolchain templates. | `string` | `""` |
{: caption="Table 3. Template repository" caption-side="top"}

### Bring your own code
{: #devsecops-alm-byoc}

The default experience for the DevSecOps toolchains is to use a sample app. This default can be updated to create an integration for your own application repository, which can be set to either clone an existing repository or use an existing repository. Set the following variables to clone or use an existing repository:

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `ci_app_repo_clone_from_branch` | Used when `app_repo_clone_from_url` is provided, the default branch that is used by the CI build, usually either `main` or `master`. | `string` | `""` |
| `ci_app_repo_clone_from_url`| Override the default sample app by providing your own sample app URL, which is cloned into the app repo. If the app repo exists, use `clone_if_not_exists` to leave the repo contents unchanged. | `string` | `""` |
| `ci_app_repo_clone_to_git_id` | By default absent, otherwise use custom server GUID, or other options for `git_id` field in the browser UI. | `string` | `""` |
| `ci_app_repo_clone_to_git_provider` | By default `hostedgit`, otherwise use `githubconsolidated` or `gitlab`. | `string` | `""` |
| `ci_app_repo_existing_branch` | Used when `app_repo_existing_url` is provided, the default branch that is used by the CI build, usually either `main` or `master`. | `string` | `""` |
| `ci_app_repo_existing_git_id`  | By default absent, otherwise use custom server GUID, or other options for `git_id` field in the browser UI. | `string` | `""` |
| `ci_app_repo_existing_git_provider`  | By default `hostedgit`, otherwise use `githubconsolidated` or `gitlab`. | `string` | `""` |
| `ci_app_repo_existing_url`  | Override to bring your own existing application repository URL, which is used directly instead of cloning the default sample. | `string` | `""` |
{: caption="Table 4. Clone or use an existing repository" caption-side="top"}

To use an existing application in github.ibm.com, use the following example settings:

| Name | Value |
|------|-------------|
|`ci_app_repo_existing_url` |`"https://github.ibm.com/{my-org}/{my-repo}.git"`|
|`ci_app_repo_existing_branch`|`"main"`|
|`ci_app_repo_existing_git_provider`|`"githubconsolidated"`|
|`ci_app_repo_existing_git_id`|`"integrated"`|
{: caption="Table 5. Examples to use an existing application" caption-side="top"}

### Secrets
{: #devsecops-alm-secrets}

To run all the toolchains, you need the following secrets at a minimum:

- An API key to run the toolchains.
- A GPG key to sign images in the CI toolchain.

By default, the DevSecOps toolchains expect the secrets to be in Secrets Manager. The API key must be stored with an `ibmcloud-api-key` entry and the signing key that is stored under `signing_key`.

To create an API key for the DevSecOps toolchains, see [{{site.data.keyword.cloud}} API key](https://cloud.ibm.com/iam/apikeys){: external} and review the [IAM Access permissions](/docs/devsecops?topic=devsecops-iam-permissions). The IAM access permissions are relevant only if you are running toolchains in some one else's account. Do not confuse the API key for running the toolchains with the API key called `ibmcloud_api_key`, which is one of the required inputs for setting up the toolchains.

`ibmcloud-api-key` and `signing_key` are the default names of the expected secrets in the secrets vault. These default names can be changed. See the following table for more details:

| Name | Description |  Value |
|------|-------------|------|
|`ci_ibmcloud_api_key_secret_name`|Name of the {{site.data.keyword.cloud}} API key secret in the secret provider.|`"ibmcloud-api-key"`|
|`cd_ibmcloud_api_key_secret_name`|Name of the {{site.data.keyword.cloud}} API key secret in the secret provider.|`"ibmcloud-api-key"`|
|`cc_ibmcloud_api_key_secret_name`|Name of the {{site.data.keyword.cloud}} API key secret in the secret provider.|`"ibmcloud-api-key"`|
|`ci_signing_key_secret_name`|Name of the signing key secret in the secret provider.|`"signing_key"`|
{: caption="Table 6. Secret names" caption-side="top"}

For more secret names, search for "secret_name" in the variable list.

### {{site.data.keyword.keymanagementservicelong}}
{: #devsecops-alm-keyprotect}

Secrets Manager is the default secrets provider for the DevSecOps toolchains. However, you can use {{site.data.keyword.keymanagementserviceshort}} instead. Set the following variables for {{site.data.keyword.keymanagementserviceshort}}. These variables apply to the {{site.data.keyword.keymanagementserviceshort}} integration in all the DevSecOps toolchains that are created.

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `kp_name` | Name of the {{site.data.keyword.keymanagementserviceshort}} instance where the secrets are stored. | `string` | `"kp-compliance-secrets"` |
| `kp_location`  | {{site.data.keyword.cloud}} location or region that contains the {{site.data.keyword.keymanagementserviceshort}} instance. | `string` | `"us-south"` |
| `kp_resource_group` | The resource group that contains the {{site.data.keyword.keymanagementserviceshort}} instance for your secrets. | `string` | `"Default"` |
{: caption="Table 7. {{site.data.keyword.keymanagementserviceshort}}" caption-side="top"}

Alternatively, you can set the {{site.data.keyword.keymanagementserviceshort}} integration settings in individual toolchains by using the prefixed form of the variables:

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `ci_kp_name`  | Name of the {{site.data.keyword.keymanagementserviceshort}} instance where the secrets are stored. | `string` | `""` |
| `ci_kp_location` | {{site.data.keyword.cloud}} location or region that contains the {{site.data.keyword.keymanagementserviceshort}} instance. | `string` | `""` |
| `ci_kp_resource_group`  | The resource group that contains the {{site.data.keyword.keymanagementserviceshort}} instance. | `string` | `""` |
| `cd_kp_name`  | Name of the {{site.data.keyword.keymanagementserviceshort}} instance where the secrets are stored. | `string` | `""` |
| `cd_kp_location` | {{site.data.keyword.cloud}} location or region that contains the {{site.data.keyword.keymanagementserviceshort}} instance. | `string` | `""` |
| `cd_kp_resource_group`  | The resource group that contains the {{site.data.keyword.keymanagementserviceshort}} instance for your secrets. | `string` | `""` |
| `cc_kp_name` | Name of the {{site.data.keyword.keymanagementserviceshort}} instance where the secrets are stored. | `string` | `""` |
| `cc_kp_location` | {{site.data.keyword.cloud}} location or region that contains the {{site.data.keyword.keymanagementserviceshort}} instance. | `string` | `""` |
| `cc_kp_resource_group` | The resource group that contains the {{site.data.keyword.keymanagementserviceshort}} instance for your secrets. | `string` | `""` |
{: caption="Table 8. Prefixed forms" caption-side="top"}

### Switching between secrets providers
{: #devsecops-alm-switchsecret}

Secrets Manager is the default provider for the toolchains. Use the following variables to switch between {{site.data.keyword.keymanagementserviceshort}} and Secrets Manager for each toolchain.

Secrets providers can be switched across all the toolchains.

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `enable_key_protect` | Enable {{site.data.keyword.keymanagementserviceshort}} integrations. | `bool` | `false` |
| `enable_secrets_manager` | Enable the Secrets Manager integrations. | `bool` | `true` |
{: caption="Table 9. Secrets providers" caption-side="top"}

Alternatively, you can switch secrets providers seperately for the different toolchains. The toolchain-specific variable defaults for the secrets providers, as outlined in the Table 10, are set to `false` by default. The variables are grouped under Key Protect and Secrets Manager. All the variables in a group take precendence if any variable in that group is changed from the default value.

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `ci_enable_key_protect` | Enables {{site.data.keyword.keymanagementserviceshort}} integration. | `bool` | `false` |
| `cd_enable_key_protect` | Uses the {{site.data.keyword.keymanagementserviceshort}} integration. | `bool` | `false` |
| `cc_enable_key_protect` | Enables {{site.data.keyword.keymanagementserviceshort}} integration. | `bool` | `false` |
| `ci_enable_secrets_manager`  | Enables Secrets Manager integration. | `bool` | `false` |
| `cd_enable_secrets_manager` | Uses the Secrets Manager integration. | `bool` | `false` |
| `cc_enable_secrets_manager` | Enables Secrets Manager integration. | `bool` | `false` |
{: caption="Table 10. Switch secrets provider" caption-side="top"}

Set these variables to `true` to use a {{site.data.keyword.keymanagementserviceshort}} integration instead of Secrets Manager. Also, set the Secrets Manager values to `false` in this case so that an unnecessary integration is not created in the toolchain.

## Optional CI, CD, and CC variables
{: #devsecops-alm-optional}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `deployment_repo_url` | The repository to clone deployment for DevSecOps toolchain template. | `string` | `""` |
| `ibmcloud_api` | {{site.data.keyword.cloud}} API Endpoint. | `string` | `"https://cloud.ibm.com"` |
{: caption="Table 11. Optional API key, secrets, and toolchain" caption-side="bottom"}
{: #alm-opt-other-variables}
{: tab-title="API key, secrets, and toolchain"}
{: tab-group="optional"}
{: class="simple-tab-table"}

| Name | Description | Type | Default | 
|------|-------------|------|---------|
| `ci_app_group`  | Specify Git user or group for your application. | `string` | `""` |
| `ci_app_name`  | Name of the application image and inventory entry. | `string` | `"hello-compliance-app"` |
| `ci_app_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `ci_app_repo_git_token_secret_name`  | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `ci_app_version`  | The version of the app to deploy. | `string` | `"v1"` |
| `ci_authorization_policy_creation` | Disable toolchain service to Secrets Manager Service authorization policy creation. | `string` | `""` |
| `ci_cluster_name` | Name of the Kubernetes cluster where the application is deployed. The cluster can be the same cluster that is used for prod. | `string` | `"mycluster-free"` |
| `ci_cluster_namespace` | Name of the Kubernetes cluster namespace where the application is deployed. | `string` | `"dev"` |
| `ci_code_engine_build_strategy` | The build strategy for the Code Engine entity. Default strategy is `dockerfile`. Set as `buildpacks` for `buildpacks` build. | `string` | `""` |
| `ci_code_engine_entity_type` | Type of Code Engine entity to create or update as part of deployment. Default type is `application`. Set as `job` for `job` type. | `string` | `""` |
| `ci_code_engine_project` | The name of the Code Engine project to use (or create). | `string` | `"DevSecOps_CE"` |
| `ci_code_engine_region`  | The region to create or look up for the Code Engine project. | `string` | `"ibm:yp:us-south"` |
| `ci_code_engine_resource_group`  | The resource group of the Code Engine project. | `string` | `"Default"` |
| `ci_code_engine_source`| The path to the location of code to build in the repository. | `string` | `""` |
| `ci_compliance_base_image` | Pipeline baseimage to run most of the built-in pipeline code. | `string` | `""` |
| `ci_compliance_pipeline_group`  | Specify user or group for compliance pipline repo. | `string` | `""` |
| `ci_compliance_pipeline_repo_auth_type`| Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `ci_compliance_pipeline_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `ci_cos_api_key_secret_name`  | Name of the Cloud Object Storage API key secret in the secret provider. | `string` | `"cos-api-key"` |
| `ci_cos_bucket_name` | Cloud Object Storage bucket name. | `string` | `""` |
| `ci_cos_endpoint`  | Cloud Object Storage endpoint name. | `string` | `""` |
| `ci_cra_generate_cyclonedx_format` | If set to 1, CRA also generates the BOM in cyclonedx format (defaults to 1). | `string` | `"1"` |
| `ci_custom_image_tag`  | The custom tag for the image in a comma-separated list. | `string` | `""` |
| `ci_deployment_target` | The deployment target, cluster, or code-engine. | `string` | `"cluster"` |
| `ci_dev_region`  | Region of the Kubernetes cluster where the application is deployed. | `string` | `"ibm:yp:us-south"` |
| `ci_dev_resource_group` | The cluster resource group. | `string` | `"Default"` |
| `ci_doi_environment`  | The DevOps Insights target environment. | `string` | `""` |
| `ci_doi_toolchain_id`  | DevOps Insights toolchain ID to link to. | `string` | `""` |
| `ci_doi_toolchain_id_pipeline_property`  | The DevOps Insights instance toolchain ID. | `string` | `""` |
| `ci_enable_pipeline_dockerconfigjson` | Adds the `pipeline-dockerconfigjson` property to the pipeline properties.| `bool` | `false` |
| `ci_enable_slack` | Set to `true` to create the integration. | `bool` | `false` |
| `ci_evidence_group`  | Specify Git user or group for evidence repository. | `string` | `""` |
| `ci_evidence_repo_auth_type`  | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `ci_evidence_repo_git_token_secret_name`  | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `ci_inventory_group`  | Specify Git user or group for inventory repository. | `string` | `""` |
| `ci_inventory_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `ci_inventory_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `ci_issues_group` | Specify Git user or group for issues repository. | `string` | `""` |
| `ci_issues_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `ci_issues_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `ci_link_to_doi_toolchain`  | Enable a link to a DevOps Insights instance in another toolchain. | `bool` | `false` |
| `ci_opt_in_dynamic_api_scan`  | Enable the OWASP Zap API scan. `1` enable or `0` disable. | `string` | `"1"` |
| `ci_opt_in_dynamic_scan`| To enable the OWASP Zap scan. `1` enable or `0` disable. | `string` | `"1"` |
| `ci_opt_in_dynamic_ui_scan` | To enable the OWASP Zap UI scan. `1` enable or `0` disable. | `string` | `"1"` |
| `ci_opt_in_sonar` | Opt in for SonarQube. | `string` | `"1"` |
| `ci_opt_out_v1_evidence` | Opt out of v1 evidence.| `string` | `"1"` |
| `ci_pipeline_config_group` | Specify user or group for pipeline config repo. | `string` | `""` |
| `ci_pipeline_config_path` | The name and path of the `pipeline-config.yaml` file within the pipeline-config repo. | `string` | `".pipeline-config.yaml"` |
| `ci_pipeline_config_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `ci_pipeline_config_repo_branch` | Specify the branch that contains the custom `pipeline-config.yaml` file. | `string` | `""` |
| `ci_pipeline_config_repo_clone_from_url` | Specify a repository that contains a custom `pipeline-config.yaml` file. | `string` | `""` |
| `ci_pipeline_config_repo_existing_url`  | Specify a repository that contains a custom `pipeline-config.yaml` file. | `string` | `""` |
| `ci_pipeline_config_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `ci_pipeline_debug` | `0` by default. Set to `1` to enable debug logging. | `string` | `"0"` |
| `ci_pipeline_dockerconfigjson_secret_name` | Name of the pipeline docker config JSON secret in the secret provider.| `string` | `"pipeline_dockerconfigjson_secret_name"` |
| `ci_pipeline_ibmcloud_api_key_secret_name` | Name of the Cloud API key secret in the secret provider. | `string` | `"ibmcloud-api-key"` |
| `ci_registry_namespace` | A unique namespace within the {{site.data.keyword.cloud}} Container Registry region where the application image is stored. | `string` | `""` |
| `ci_registry_region` | The {{site.data.keyword.cloud}} Region where the {{site.data.keyword.cloud}} Container Registry namespace is to be created. | `string` | `"ibm:yp:us-south"` |
| `ci_repositories_prefix`  | Prefix name for the cloned compliance repos. | `string` | `"compliance"` |
| `ci_signing_key_secret_name` | Name of the signing key secret in the secret provider. | `string` | `"signing_key"` |
| `ci_slack_channel_name` | The Slack channel that notifications are posted to. | `string` | `"my-channel"` |
| `ci_slack_notifications` | The switch that turns the Slack integration on or off. | `string` | `"0"` |
| `ci_slack_pipeline_fail`  | Generate pipeline failed notifications. | `bool` | `true` |
| `ci_slack_pipeline_start` | Generate pipeline start notifications. | `bool` | `true` |
| `ci_slack_pipeline_success` | Generate pipeline succeeded notifications. | `bool` | `true` |
| `ci_slack_team_name` | The Slack team name, which is the word or phrase before .slack.com in the team URL. | `string` | `"my-team"` |
| `ci_slack_toolchain_bind`  | Generate tool added to toolchain notifications. | `bool` | `true` |
| `ci_slack_toolchain_unbind`  | Generate tool removed from toolchain notifications. | `bool` | `true` |
| `ci_slack_webhook_secret_name`  | Name of the webhook secret in the secret provider. | `string` | `"slack-webhook"` |
| `ci_sm_location` | {{site.data.keyword.cloud}} location or region that contains the Secrets Manager instance. | `string` | `""` |
| `ci_sm_name` | Name of the Secrets Manager instance where the secrets are stored. | `string` | `""` |
| `ci_sm_resource_group` | The resource group that contains the Secrets Manager instance. | `string` | `""` |
| `ci_sm_secret_group`  | Group in Secrets Manager for organizing or grouping secrets. | `string` | `""` |
| `ci_sonarqube_config` | Runs a SonarQube scan in an isolated Docker-in-Docker container (default configuration) or in an existing Kubernetes cluster (custom configuration). Options: default or custom. Default is default. | `string` | `"default"` |
| `ci_toolchain_description`  | Description for the CI toolchain. | `string` | `"Toolchain created with terraform template for DevSecOps CI Best Practices."` |
| `ci_toolchain_name`  | The name of the CI toolchain. | `string` | `"DevSecOps CI Toolchain - Terraform"` |
| `ci_toolchain_region` | The region that contains the CI toolchain. | `string` | `""` |
| `ci_toolchain_resource_group` | The resource group within which the toolchain is created. | `string` | `""` |
{: caption="Table 11. Continuous integration" caption-side="bottom"}
{: #alm-opt-ci-variables}
{: tab-title="Continuous integration"}
{: tab-group="optional"}
{: class="simple-tab-table"}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `cd_app_version`  | The version of the app to deploy. | `string` | `"v1"` |
| `cd_authorization_policy_creation`  | Disable toolchain service to Secrets Manager Service authorization policy creation. | `string` | `""` |
| `cd_change_management_group` | Specify group for change management repository. | `string` | `""` |
| `cd_change_management_repo`  | This repository holds the change management requests created for the deployments. | `string` | `""` |
| `cd_change_management_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `cd_change_management_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `cd_change_repo_clone_from_url` | Override the default management repo, which is cloned into the app repo. If the app repo exists, use `clone_if_not_exists` to leave the repo contents unchanged. | `string` | `""` |
| `cd_change_request_id` | The ID of an open change request. If this variable is set to `notAvailable`, a change request is automatically created by the continuous deployment pipeline. | `string` | `"notAvailable"` |
| `cd_cluster_name`| Name of the Kubernetes cluster where the application is deployed. | `string` | `"mycluster-free"` |
| `cd_cluster_namespace` | Name of the Kubernetes cluster namespace where the application is deployed. | `string` | `"prod"` |
| `cd_cluster_region` | Region of the Kubernetes cluster where the application is deployed. | `string` | `"ibm:yp:us-south"` |
| `cd_code_signing_cert_secret_name` | Name of the code signing certificate secret in the secret provider. | `string` | `"code-signing-cert"` |
| `cd_compliance_base_image` | Pipeline baseimage to run most of the built-in pipeline code. | `string` | `""` |
| `cd_compliance_pipeline_group` | Specify user or group for compliance pipline repo. | `string` | `""` |
| `cd_compliance_pipeline_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `cd_compliance_pipeline_repo_git_token_secret_name`  | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `cd_cos_api_key_secret_name` | Name of the Cloud Object Storage API key secret in the secret provider. | `string` | `"cos-api-key"` |
| `cd_cos_bucket_name` | Cloud Object Storage bucket name. | `string` | `""` |
| `cd_cos_endpoint`  | Cloud Object Storage endpoint name. | `string` | `""` |
| `cd_customer_impact` | Custom impact of the change request. | `string` | `"no_impact"` |
| `cd_deployment_group` | Specify group for deployment. | `string` | `""` |
| `cd_deployment_repo_auth_type`  | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `cd_deployment_repo_clone_from_branch` | Used when `deployment_repo_clone_from_url` is provided, the default branch that is used by the CD build, usually either `main` or `master`. | `string` | `""` |
| `cd_deployment_repo_clone_from_url` | Override the default sample app by providing your own sample deployment URL, which is cloned into the app repo. If the app repo exists, use `clone_if_not_exists` to leave the repo contents unchanged. | `string` | `""` |
| `cd_deployment_repo_clone_to_git_id` | By default absent, otherwise use custom server GUID, or other options for `git_id` field in the browser UI. | `string` | `""` |
| `cd_deployment_repo_clone_to_git_provider` | By default `hostedgit`, otherwise use `githubconsolidated` or `gitlab`. | `string` | `""` |
| `cd_deployment_repo_existing_branch`  | Used when `deployment_repo_existing_url` is provided, the default branch that is by the CD build, usually either `main` or `master`. | `string` | `""` |
| `cd_deployment_repo_existing_git_id`  | By default absent, otherwise use custom server GUID, or other options for `git_id` field in the browser UI. | `string` | `""` |
| `cd_deployment_repo_existing_git_provider` | By default `hostedgit`, otherwise use `githubconsolidated` or `gitlab`. | `string` | `"hostedgit"` |
| `cd_deployment_repo_existing_url` | Override to bring your own existing deployment repository URL, which is used directly instead of cloning the default deployment sample. | `string` | `""` |
| `cd_deployment_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `cd_doi_environment` | DevOps Insights environment for DevSecOps CD deployment. | `string` | `""` |
| `cd_doi_toolchain_id` | DevOps Insights toolchain ID to link to. | `string` | `""` |
| `cd_emergency_label`  | Identifies the pull request as an emergency. | `string` | `"EMERGENCY"` |
| `cd_enable_signing_validation` | Adds the `code-signing-certificate` Property to the pipeline properties. | `bool` | `false` |
| `cd_enable_slack` | Set to true to create the integration. | `bool` | `false` |
| `cd_evidence_group` | Specify Git user or group for evidence repository. | `string` | `""` |
| `cd_evidence_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `cd_evidence_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `cd_inventory_group` | Specify Git user or group for inventory repository. | `string` | `""` |
| `cd_inventory_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `cd_inventory_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `cd_issues_group` | Specify Git user or group for issues repository. | `string` | `""` |
| `cd_issues_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `cd_issues_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `cd_link_to_doi_toolchain` | Enable a link to a DevOps Insights instance in another toolchain, true or false. | `bool` | `true` |
| `cd_merge_cra_sbom` | Merge the SBOM | `string` | `"1"` |
| `cd_opt_out_v1_evidence` | Opt out of evidence v1. | `string` | `"1"` |
| `cd_pipeline_config_group` | Specify user or group for pipeline config repo. | `string` | `""` |
| `cd_pipeline_config_path` | The name and path of the pipeline-config.yaml file within the pipeline-config repo. | `string` | `".pipeline-config.yaml"` |
| `cd_pipeline_config_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `cd_pipeline_config_repo_branch` | Specify the branch that contains the custom pipeline-config.yaml file. | `string` | `""` |
| `cd_pipeline_config_repo_clone_from_url` | Specify a repository that contains a custom pipeline-config.yaml file. | `string` | `""` |
| `cd_pipeline_config_repo_existing_url` | Specify a repository that contains a custom pipeline-config.yaml file. | `string` | `""` |
| `cd_pipeline_config_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `cd_pipeline_debug`  | `0` by default. Set to `1` to enable debug logging. | `string` | `"0"` |
| `cd_pipeline_ibmcloud_api_key_secret_name` | Name of the Cloud API key secret in the secret provider. | `string` | `"ibmcloud-api-key"` |
| `cd_repositories_prefix`  | Prefix name for the cloned compliance repos. | `string` | `"compliance"` |
| `cd_satellite_cluster_group` | The Satellite cluster group | `string` | `""` |
| `cd_scc_enable_scc` | Enable the SCC integration. | `bool` | `true` |
| `cd_scc_integration_name`  | The name of the SCC integration name. | `string` | `"Security and Compliance"` |
| `cd_slack_channel_name`  | The Slack channel that notifications are posted to. | `string` | `"my-channel"` |
| `cd_slack_notifications` | The switch to turn the Slack integration on or off. | `string` | `"0"` |
| `cd_slack_pipeline_fail` | Generate pipeline failed notifications. | `bool` | `true` |
| `cd_slack_pipeline_start` | Generate pipeline start notifications. | `bool` | `true` |
| `cd_slack_pipeline_success` | Generate pipeline succeeded notifications. | `bool` | `true` |
| `cd_slack_team_name`  | The Slack team name, which is the word or phrase before .slack.com in the team URL. | `string` | `"my-team"` |
| `cd_slack_toolchain_bind`  | Generate tool added to toolchain notifications. | `bool` | `true` |
| `cd_slack_toolchain_unbind` | Generate tool removed from toolchain notifications. | `bool` | `true` |
| `cd_slack_webhook_secret_name` | Name of the webhook secret in the secret provider. | `string` | `"slack-webhook"` |
| `cd_sm_location` | {{site.data.keyword.cloud}} location or region that contains the Secrets Manager instance. | `string` | `""` |
| `cd_sm_name`  | Name of the Secrets Manager instance where the secrets are stored. | `string` | `""` |
| `cd_sm_resource_group` | The resource group that contains the Secrets Manager instance for your secrets. | `string` | `""` |
| `cd_sm_secret_group`  | Group in Secrets Manager for organizing or grouping secrets. | `string` | `""` |
| `cd_source_environment` | The source environment that the app is promoted from. | `string` | `"master"` |
| `cd_target_environment` | The target environment that the app is deployed to. | `string` | `"prod"` |
| `cd_target_environment_detail` | Details of the environment that is being updated. | `string` | `""` |
| `cd_target_environment_purpose` | Purpose of the environment that is being updated. | `string` | `"production"` |
| `cd_toolchain_description` | Description for the CD toolchain. | `string` | `"Toolchain created with terraform template for DevSecOps CD Best Practices."` |
| `cd_toolchain_name`  | The name of the CD toolchain. | `string` | `"DevSecOps CD Toolchain - Terraform"` |
| `cd_toolchain_region`  | The region that contains the CI toolchain. | `string` | `""` |
| `cd_toolchain_resource_group`  | Resource group within which toolchain is created. | `string` | `""` |
{: caption="Table 11. Continuous deployment" caption-side="bottom"}
{: #alm-opt-cd-variables}
{: tab-title="Continuous deployment"}
{: tab-group="optional"}
{: class="simple-tab-table"}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `cc_app_group` | Specify user or group for app repo. | `string` | `""` |
| `cc_app_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `oauth` |
| `cc_app_repo_branch` | The default branch of the app repo. | `string` | `"master"` |
| `cc_app_repo_git_id` | By default absent, otherwise use the custom server GUID, or other options for `git_id` field in the browser UI.  | `string` | `""` |
| `cc_app_repo_git_provider` | The type of the Git provider. | `string` | `"hostedgit"` |
| `cc_app_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `cc_app_repo_url` | The Git URL for the application repository. | `string` | `""` |
| `cc_authorization_policy_creation` | Disable the toolchain service to Secrets Manager service authorization policy creation. | `string` | `""` |
| `cc_compliance_base_image`  | Pipeline baseimage to run most of the built-in pipeline code. | `string` | `""` |
| `cc_compliance_pipeline_group` | Specify user or group for compliance pipline repo. | `string` | `""` |
| `cc_compliance_pipeline_repo_auth_type` | Select the authentication method that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `cc_compliance_pipeline_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `cc_cos_api_key_secret_name` | Name of the Cloud Object Storage API key secret in the secret provider. | `string` | `"cos-api-key"` |
| `cc_cos_bucket_name` | Cloud Object Storage bucket name. | `string` | `""` |
| `cc_cos_endpoint` | Cloud Object Storage endpoint name. | `string` | `""` |
| `cc_doi_environment`  | DevOps Insights environment for DevSecOps CD deployment. | `string` | `""` |
| `cc_doi_toolchain_id`  | DevOps Insights toolchain ID to link to. | `string` | `""` |
| `cc_enable_pipeline_dockerconfigjson` | Adds the pipeline-dockerconfigjson property to the pipeline properties.| `bool` | `false` |
| `cc_enable_slack` | Create the Slack integration. | `bool` | `false` |
| `cc_environment_tag` | Tag name that represents the target environment in the inventory. Example: `prod_latest`. | `string` | `"prod_latest"` |
| `cc_evidence_group`  | Specify Git user or group for evidence repository. | `string` | `""` |
| `cc_evidence_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `cc_evidence_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `cc_inventory_group`  | Specify Git user or group for inventory repository. | `string` | `""` |
| `cc_inventory_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `cc_inventory_repo_git_token_secret_name`  | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `cc_issues_group` | Specify Git user or group for issues repository. | `string` | `""` |
| `cc_issues_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `cc_issues_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `cc_link_to_doi_toolchain` | Enable a link to a DevOps Insights instance in another toolchain, true or false. | `bool` | `true` |
| `cc_opt_in_auto_close`  | Enable auto-closing of issues that come from vulnerabilities when the vulnerability is no longer detected by the CC pipeline run. | `string` | `"1"` |
| `cc_opt_in_dynamic_api_scan` | Enable the OWASP Zap API scan. `1` enable or `0` disable. | `string` | `""` |
| `cc_opt_in_dynamic_scan`  | Enable the OWASP Zap scan. `1` enable or `0` disable. | `string` | `""` |
| `cc_opt_in_dynamic_ui_scan` | Enable the OWASP Zap UI scan. `1` enable or `0` disable. | `string` | `""` |
| `cc_pipeline_config_group` | Specify user or group for pipeline config repo. | `string` | `""` |
| `cc_pipeline_config_path` | The name and path of the `pipeline-config.yaml` file within the pipeline-config repo. | `string` | `".pipeline-config.yaml"` |
| `cc_pipeline_config_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `"oauth"` |
| `cc_pipeline_config_repo_branch` | Specify the branch that contains the custom `pipeline-config.yaml` file. | `string` | `""` |
| `cc_pipeline_config_repo_clone_from_url` | Specify a repository that contains a custom `pipeline-config.yaml` file. | `string` | `""` |
| `cc_pipeline_config_repo_existing_url` | Specify a repository that contains a custom `pipeline-config.yaml` file. | `string` | `""` |
| `cc_pipeline_config_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `"git-token"` |
| `cc_pipeline_debug` | `0` by default. Set to `1` to enable debug logging. | `string` | `"0"` |
| `cc_pipeline_dockerconfigjson_secret_name` | Name of the pipeline docker config JSON secret in the secret provider.| `string` | `"pipeline_dockerconfigjson_secret_name"` |
| `cc_pipeline_ibmcloud_api_key_secret_name` | Name of the Cloud API key secret in the secret provider. | `string` | `"ibmcloud-api-key"` |
| `cc_repositories_prefix` | The prefix for the compliance repositories. | `string` | `"compliance"` |
| `cc_scc_enable_scc` | Enable the SCC integration. | `bool` | `true` |
| `cc_scc_integration_name` | The SCC integration name. | `string` | `"Security and Compliance"` |
| `cc_slack_channel_name` | The Slack channel that notifications are posted to. | `string` | `"my-channel"` |
| `cc_slack_notifications` | Turns the Slack integration on or off. | `string` | `"0"` |
| `cc_slack_pipeline_fail` | Generate pipeline failed notifications. | `bool` | `true` |
| `cc_slack_pipeline_start` | Generate pipeline start notifications. | `bool` | `true` |
| `cc_slack_pipeline_success` | Generate pipeline succeeded notifications. | `bool` | `true` |
| `cc_slack_team_name` | The Slack team name, which is the word or phrase before `.slack.com` in the team URL. | `string` | `"my-team"` |
| `cc_slack_toolchain_bind` | Generate tool added to toolchain notifications. | `bool` | `true` |
| `cc_slack_toolchain_unbind`  | Generate tool removed from toolchain notifications. | `bool` | `true` |
| `cc_slack_webhook_secret_name`  | Name of the webhook secret in the secret provider. | `string` | `"slack-webhook"` |
| `cc_sm_location`  | {{site.data.keyword.cloud}} location or region that contains the Secrets Manager instance. | `string` | `""` |
| `cc_sm_name` | Name of the Secrets Manager instance where the secrets are stored. | `string` | `""` |
| `cc_sm_resource_group`  | The resource group that contains the Secrets Manager instance for your secrets. | `string` | `""` |
| `cc_sm_secret_group`  | Group in Secrets Manager for organizing or grouping secrets. | `string` | `""` |
| `cc_sonarqube_config` | Runs a SonarQube scan in an isolated Docker-in-Docker container (default configuration) or in an existing Kubernetes cluster (custom configuration). Options: default or custom. Default is default. | `string` | `"default"` |
| `cc_toolchain_description` | Description of the CC toolchain. | `string` | `"Toolchain created with terraform template for DevSecOps CC Best Practices."` |
| `cc_toolchain_name` | The name of the CC toolchain. | `string` | `"DevSecOps CC Toolchain - Terraform"` |
| `cc_toolchain_region` | The region that contains the CI toolchain. | `string` | `""` |
| `cc_toolchain_resource_group`  | Resource group within which the toolchain is created. | `string` | `""` |
{: caption="Table 11. Continuous compliance" caption-side="bottom"}
{: #alm-opt-cc-variables}
{: tab-title="Continuous compliance"}
{: tab-group="optional"}
{: class="simple-tab-table"}

## Output variables
{: #devsecops-alm-output}

| Name | Description |
|------|-------------|
| `app_repo_url` | The App repo URL. |
| `compliance_cc_toolchain_id` | The ID of the compliance CC toolchain. |
| `compliance_cd_toolchain_id` | The ID of the compliance CD toolchain. |
| `compliance_ci_toolchain_id` | The ID of the compliance CI toolchain. |
| `evidence_repo_url` | The Evidence Repo URL. |
| `inventory_repo_url` | The Inventory Repo URL. |
| `issues_repo_url` | The Issues Repo URL. |
{: caption="Table 12. Outputs" caption-side="top"}
