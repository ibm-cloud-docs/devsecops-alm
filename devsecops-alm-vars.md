---

copyright:
  years: 2024
lastupdated: "2024-06-24"

keywords: devsecops-alm, deployment guide, deployable architecture

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# DevSecOps Application Lifecycle Management mandatory and optional variables
{: #devsecops-alm-vars}

Use the required variables to create toolchains for the out of the box experience. Then use the optional variables to make other adjustments to your toolchains.
{: shortdesc}


## Required input variables
{: #devsecops-alm-min}


| Name | Description | Type | Default |
|------|-------------|------|---------|
| `toolchain_name`  | Common element of the toolchain name. The toolchain names are appended with `CI Toolchain` or `CD Toolchain` or `CC Toolchain` followed by a timestamp. Can explicitly be set using `ci_toolchain_name`, `cd_toolchain_name`, and `cc_toolchain_name`. | `string` | `DevSecOps` |
| `toolchain_region` | The region identifier that is used, by default, for all resource creation and service instance lookup. This can be overridden on a per resource or service basis. See `ci_toolchain_region`,`cd_toolchain_region`,`cc_toolchain_region`, `ci_cluster_region`, `cd_cluster_region`, `ci_registry_region`.| `string` | `us-south` |
| `toolchain_resource_group` | The resource group that is used, by default, for all resource creation and service instance lookups. This can be overridden on a per resource or service basis. See `ci_toolchain_resource_group`,`cd_toolchain_resource_group`,`cc_toolchain_resource_group`, `ci_cluster_resource_group`. | `string` | `Default` |
|`registry_namespace`|A unique namespace within the IBM Cloud Container Registry region where the application image is stored.|`string`|`""`|
|`cluster_name`|Name of the Kubernetes cluster where the application is deployed. This sets the same cluster for both CI and CD toolchains. See `ci_cluster_name` and `cd_cluster_name` to set different clusters. By default, the cluster namespace for CI is set to `dev` and for CD is to `prod`. You can change these settings by using `ci_cluster_namespace` and `cd_cluster_namespace`.|`string`|`mycluster-free`|
|`sm_location`|The region location of the Secrets Manager instance. This applies to the CI, CD, and CC Secret Manager integrations. See `ci_sm_location`, `cd_sm_location`, and `cc_sm_location` to set separately.|`string`|`us-south`|
|`sm_name`|The name of the Secret Manager instance. This applies to the CI, CD, and CC Secret Manager integrations. See `ci_sm_name`, `cd_sm_name`, and `cc_sm_name` to set separately.|`string`|`sm-instance`|
|`sm_resource_group`|The resource group containing the Secrets Manager instance. This applies to the CI, CD, and CC Secret Manager integrations. See `ci_sm_resource_group`, `cd_sm_resource_group`, and `cc_sm_resource_group` to set separately.|`string`|`Default`|
|`sm_secret_group`|Group in Secrets Manager for organizing/grouping secrets. This applies to the CI, CD and CC Secret Manager integrations. See `ci_sm_secret_group`, `cd_sm_secret_group`, and `cc_sm_secret_group` to set separately.|`string`|`Default`|
|`ibmcloud_api_key`|API key that creates the toolchains. |  |  |
{: caption="Template repository" caption-side="top"}

The variables that are prefixed with `ci`, `cd`, and `cc` apply to the CI, CD, and CC toolchains. Nonprefixed variables apply to all the toolchains. Also, modifying the default values for the prefixed variable inputs causes the prefixed variables inputs to take precedence over nonprefixed variable inputs.
{: note}

## Optional input variables
{: #devsecops-alm-opt}

### Group level variables
{: #devsecops-alm-grp}

 | Name | Description | Type | Default |
|------|-------------|------|---------|
| `add_pipeline_definitions` | Set to `true` to add pipeline definitions. | `string` | `"true"` |
| `cos_endpoint` | Set the Cloud Object Storage endpoint for accessing your COS bucket. This setting sets the same endpoint for COS in the CI, CD, and CC toolchains. See `ci_cos_endpoint`, `cd_cos_endpoint`, and `cc_cos_endpoint` to set the endpoints separately.| `string` | `""` |
| `enable_slack` | Set to `true` to create the integration. This requires a valid `slack_channel_name`, `slack_team_name`, and a valid `webhook` (see `slack_webhook_secret_name`). This setting applies for CI, CD, and CC toolchains. To enable Slack separately, see `ci_enable_slack`, `cd_enable_slack`, and `cc_enable_slack`.| `bool` | `false` |
|`slack_channel_name`|The Slack channel that notifications are posted to. This applies to the CI, CD, and CC toolchains. To set separately see `ci_slack_channel_name`, `cd_slack_channel_name`, and `cc_slack_channel_name`|`string`|`my-channel`|
|`slack_team_name`|The Slack team name, which is the word or phrase before `.slack.com` in the team URL. This applies to the CI, CD, and CC toolchains. To set separately, see `ci_slack_team_name`, `cd_slack_team_name`, and `cc_slack_team_name`.|`string`|`my-team`|
|`slack_webhook_secret_name`|The name of the webhook secret for Slack in the secret provider. This applies to the CI, CD, and CC toolchains. To set separately, see `ci_slack_webhook_secret_name`, `cd_slack_webhook_secret_name`, and `cc_slack_webhook_secret_name`.|`string`|`slack-webhook`|
|`slack_notifications`|This is enabled automatically when a Slack integration is created. The switch overrides the Slack notifications. Set `1` for on and `0` for off. This applies to the CI, CD, and CC toolchains. To set separately, see `ci_slack_notifications`, `cd_slack_notifications`, and `cc_slack_notifications`.|`string`|`""`|
|`authorization_policy_creation`|Disable Toolchain Service to Secrets Manager Service authorization policy creation. To disable set the value to `disabled`. This applies to the CI, CD, and CC toolchains. To set separately, see `ci_authorization_policy_creation`, `cd_authorization_policy_creation`, and `cc_authorization_policy_creation`.|`string`|`""`|
|`repo_group`|Specify Git user or group for your application. This must be set if the repository authentication type is `pat` (personal access token).|`string`|`""`|
|`repo_secret_group`|Secret group in Secrets Manager that contains the secret for the repo. This variable sets the same secret group for all the repositories. It can be overridden on a per secret group basis. Only applies when using Secrets Manager.|`string`|`""`|
|`repo_git_token_secret_name`|Name of the Git token secret in the secret provider. Specifying a secret name for the Git Token automatically sets the authentication type to `pat`.|`string`|`""`|
|`repo_git_token_secret_value`|The personal access token that will be added to the `repo_git_token_secret_name` secret in the secrets provider.|`string`|`""`|
|`compliance_base_image`|Pipeline baseimage to run most of the built-in pipeline code. Applies to the CI, CD and CC toolchain pipelines.|`string`|`""` |
| `compliance_pipeline_branch` | The Compliance Pipeline branch. | `string` | `"open-v9"` |
| `compliance_pipeline_existing_repo_url` | The URL of an existing compliance pipelines repository. | `string` | `""` |
| `compliance_pipeline_repo_blind_connection` | Setting this value to `true` means the server is not addressable on the public internet. IBM Cloud will not be able to validate the connection details you provide. Certain functionality that requires API access to the git server will be disabled. Delivery pipeline will only work using a private worker that has network access to the git server. | `string` | `""` |
| `compliance_pipeline_repo_git_id` | Set this value to `github` for github.com, or to the ID of a custom GitHub Enterprise server. | `string` | `""` |
| `compliance_pipeline_repo_git_provider` | Git provider for compliance pipeline repo. If not set will default to `hostedgit`. | `string` | `""` |
| `compliance_pipeline_repo_name` | Sets the name for the compliance pipelines repository if cloned. The expected behaviour is to link to an existing compliance-pipelines repository. | `string` | `""` |
| `compliance_pipeline_repo_root_url` | (Optional) The Root URL of the server. e.g. https://git.example.com. | `string` | `""` |
| `compliance_pipeline_repo_title` | (Optional) The title of the server. e.g. My Git Enterprise Server. | `string` | `""` |
| `compliance_pipeline_repo_use_group_settings` | Set to `true` to apply group level repository settings to the compliance pipeline repository. See `repo_git_provider` as an example. | `bool` | `false` |
| `compliance_pipeline_source_repo_url` | The URL of a compliance pipelines repository to clone. | `string` | `""` |
| `cos_api_key_secret_value` | A user provided api key with COS access permissions that can be pushed to Secrets Manager. See `cos_api_key_secret_name` and `create_cos_api_key`.| `string` | `""` | no |
| `cos_bucket_name` | Set the name of your COS bucket. This applies the same COS bucket name for the CI, CD, and CC toolchains.| `string` | `""` | no |
| `cos_instance_crn` | The CRN of the Cloud Object Storage instance containing the required bucket. This value is required to generate the correct access policies if creating IAM service credentials.| `string` | `""` | no |
| `create_access_group` | Set to `true` to create an access group for the operations of the DevSecOps toolchains.| `bool` | `false` | no |
| `create_code_engine_access_policy` | Add a Code Engine access policy to the generated IAM access key. See `create_ibmcloud_api_key`.| `bool` | `true` | no |
| `create_git_token` | Set to `true` to create and add the specified personal access token secret to the Secrets Provider. Use `repo_git_token_secret_value` for setting the value.| `bool` | `false` |
| `create_git_triggers` | Set to `true` to create the default Git triggers associated with the compliance repos and sample app. | `string` | `"true"` |
| `create_kubernetes_access_policy` | Add a Kubernetes access policy to the generated IAM access key. See `create_ibmcloud_api_key`.| `bool` | `true` | no |
| `create_privateworker_secret` | Set to `true` to add a specified private worker service api key to the Secrets Provider. | `bool` | `false` |
| `create_signing_key` | Set to `true` to create and add a `signing-key` and the `signing-certificate` to the Secrets Provider.| `bool` | `false` |
| `create_triggers` | Set to `true` to create the default triggers associated with the compliance repos and sample app. | `string` | `"true"` |
| `enable_privateworker` | Set to `true` to enable private workers for the CI, CD, CC and PR pipelines. A valid service api key must be set in Secrets Manager. The name of this secret can be specified using `privateworker_credentials_secret_name`. | `string` | `"false"` |
| `rotate_signing_key` | Set to `true` to rotate the signing key and signing certificate. It is important to make a back up for the current code signing certificate as pending CD deployments might require image validation against the previous signing key.| `bool` | `false` |
| `event_notifications_crn` | Set the {{site.data.keyword.en_short}} CRN to create an {{site.data.keyword.en_short}} integration. This paramater applies to the CI, CD and CC toolchains.| `string` | `""` |
| `force_create_standard_api_key` | Set to `true` to force create a standard api key. By default the generated api key will be a service api key. It is a requirement to use a Git Token when using the service api key for running the pipelines. See `repo_git_token_secret_name` for more details. The alternative is to set `force_create_standard_api_key` to `true` to create a standard api key.| `bool` | `false` | no |
| `gosec_private_repository_host` | Your private repository base URL. | `string` | `""` |
| `gosec_repo_ssh_key_secret_group` | Secret group prefix for the gosec private repository ssh key secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `gosec_repo_ssh_key_secret_name` | Name of the SSH key token for the private repository in the secret provider. | `string` | `"git-ssh-key"` |
| `ibmcloud_api` | TThe environment URL. When left unset this will default to `https://cloud.ibm.com`| `string` | `""` | no |
| `opt_in_gosec` | Enables gosec scans | `string` | `""` |
| `pipeline_git_tag` | The Git tag within the pipeline definitions repository for the Compliance Pipelines. | `string` | `""` |
|`sm_instance_crn` | The CRN of the Secrets Manager instance applies to CI, CD, and CC toolchains unless set individually. | `string` | `""` | no |
|`cos_api_key_secret_crn` | The CRN for the Cloud Object Storage apikey. | `string` | `""` | no |
|`event_notifications_crn` | Set the Event Notifications CRN to create an Events Notification integration. This parameter applies to the CI, CD, and CC toolchains. | `string` | `""` | no |
|`gosec_private_repository_ssh_key_secret_crn`| The CRN for the GoSec repository secret. | `string` | `""` | no |
|`pipeline_doi_api_key_secret_crn`| The CRN for the pipeline DOI apikey. | `string` | `""` | no |
|`pipeline_ibmcloud_api_key_secret_crn`| The CRN for the IBMCloud apikey. | `string` | `""` | no |
| `pipeline_ibmcloud_api_key_secret_value` | A user provided api key for running the toolchain pipelines that can be pushed to Secrets Manager. See `pipeline_ibmcloud_api_key_secret_name` and `create_ibmcloud_api_key`.| `string` | `""` | no |
| `pipeline_config_repo_git_id` | Set this value to `github` for github.com, or to the GUID of a custom GitHub Enterprise server. | `string` | `""` |
| `pipeline_config_repo_git_provider` | Git provider for pipeline repo config | `string` | `""` |
| `privateworker_credentials_secret_crn` | The CRN for the Private Worker secret secret. | `string` | `""` |
| `privateworker_credentials_secret_group` | Secret group prefix for the Private Worker secret. Defaults to using `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `privateworker_credentials_secret_name` | Name of the privateworker secret in the secret provider. | `string` | `""` |
| `privateworker_name` | The name of the private worker tool integration. | `string` | `"private-worker-tool-01"` |
| `privateworker_secret_value` | The private worker service api key that will be added to the `privateworker_credentials_secret_name` secret in the secrets provider. | `string` | `""` |
| `repo_apply_settings_to_compliance_repos` |Set to `true` to apply the same settings to all the default compliance repositories. Set to `false` to apply these settings to only the sample application, pipeline config and the deployment repositories.| `bool` | `true` | no |
|`repo_blind_connection` | Setting this value to `true` means the server is not addressable on the public internet. IBM Cloud will not be able to validate the connection details you provide. Certain functionality that requires API access to the git server will be disabled. Delivery pipeline will only work using a private worker that has network access to the git server. | `bool` | `false` |
|`repo_git_id` | The Git ID for the compliance repositories.| `string` | `""` | no |
|`repo_git_provider` | The Git provider type.| `string` | `""` | no |
|`repo_git_token_secret_crn` | The CRN for the repositories Git Token. | `string` | `""` | no |
| `repo_root_url` | (Optional) The Root URL of the server. e.g. https://git.example.com. | `string` | `""` |
|`repo_title` | (Optional) The title of the server. e.g. My Git Enterprise Server. | `string` | `""` |
|`scc_instance_crn` | The Security and Compliance Center service instance CRN (Cloud Resource Name). This parameter is only relevant when the `scc_use_profile_attachment` parameter is enabled. The value must match the regular expression. | `string` | `""` | no |
|`scc_scc_api_key_secret_crn` | The CRN for the SCC apikey. | `string` | `""` | no |
|`slack_webhook_secret_crn` | The CRN for the Slack webhook secret. | `string` | `""` | no |
|`sonarqube_secret_crn` | The CRN for the SonarQube secret. | `string` | `""` | no |
| `scc_attachment_id` | An attachment ID. An attachment is configured under a profile to define how a scan runs. To find the attachment ID, in the browser, in the attachments list, click the attachment link, and a panel appears that you can use to copy the attachment ID. This parameter is only relevant when the `scc_use_profile_attachment` parameter is enabled. | `string` | `""` |
| `scc_instance_crn` | The Security and Compliance Center service instance CRN (Cloud Resource Name). This parameter is only relevant when the `scc_use_profile_attachment` parameter is enabled. The value must match the regular expression.| `string` | `""` |
| `scc_profile_name` | The name of a Security and Compliance Center profile. Use the `IBM Cloud for Financial Services` profile, which contains the DevSecOps Toolchain rules, or use a user-authored customized profile that is configured to contain those rules. This parameter is only relevant when the `scc_use_profile_attachment` parameter is enabled. | `string` | `""` |
| `scc_profile_version` | The version of a Security and Compliance Center profile, in SemVer format, like `0.0.0`. This parameter is only relevant when the `scc_use_profile_attachment` parameter is enabled.| `string` | `""` |
| `scc_scc_api_key_secret_name` | The Security and Compliance Center api-key secret in the secret provider. | `string` | `"scc-api-key"` |
| `scc_scc_api_key_secret_group` | Secret group prefix for the Security and Compliance tool secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `scc_use_profile_attachment` | Set to `enabled` to enable use profile with attachment so that the scripts in the pipeline can interact with the Security and Compliance Center service. When enabled, other parameters become relevant; `scc_scc_scc_api_key_secret_name`, `scc_instance_crn`, `scc_profile_name`, `scc_profile_version`, `scc_attachment_id`.| `string` | `"disabled"` |
| `toolchain_access_group_name` | The name of the DevSecOps access group. See `create_access_group`.| `string` | `"devsecops-toolchain"` | no |
| `use_legacy_ref` | Set to `true` to use the legacy secret reference format for Secrets Manager secrets.| `bool` | `true` | no |
{: caption="Group level variables" caption-side="top"}

### Toolchain creation variables
{: #devsecops-alm-tccreate}

The following variables determine which toolchains are created. By default all three are set to `true`, which creates the DevSecOps CI, CD and CC toolchains. Any combination of the three toolchains can be created.

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `create_ci_toolchain` | Determines whether the DevSecOps CI toolchain is created. If this toolchain is not created, then values must be set for the following variables: `evidence_repo_existing_url`, `issues_repo_existing_url`, and `inventory_repo_existing_url`. | `bool` | `true` |
| `create_cd_toolchain` | Boolean flag that determines whether the DevSecOps CD toolchain is created. | `bool` | `true` |
| `create_cc_toolchain` | Boolean flag that determines whether the DevSecOps CC toolchain is created. | `bool` | `true` |
{: caption="toolchain creation" caption-side="top"}

### Compliance repositories
{: #devsecops-alm-compl}

If the CI toolchain is not created, you must set the following variables.

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `evidence_repo_existing_url`  | A template repository to clone compliance-evidence-locker for reference DevSecOps toolchain templates. | `string` | `""` |
| `issues_repo_existing_url` | A template repository to clone `compliance-issues` for reference DevSecOps toolchain templates. | `string` | `""` |
| `inventory_repo_existing_url` | A template repository to clone compliance-inventory for reference DevSecOps toolchain templates. | `string` | `""` |
{: caption="Template repository" caption-side="top"}

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
{: caption="Clone or use an existing repository" caption-side="top"}

To use an existing application in github.ibm.com, use the following example settings:

| Name | Value |
|------|-------------|
|`ci_app_repo_existing_url` |`"https://github.ibm.com/{my-org}/{my-repo}.git"`|
|`ci_app_repo_existing_branch`|`"main"`|
|`ci_app_repo_existing_git_provider`|`"githubconsolidated"`|
|`ci_app_repo_existing_git_id`|`"integrated"`|
{: caption="Examples to use an existing application" caption-side="top"}

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
{: caption="Secret names" caption-side="top"}

For more secret names, search for "secret_name" in the variable list.

### {{site.data.keyword.keymanagementservicelong}}
{: #devsecops-alm-keyprotect}

Secrets Manager is the default secrets provider for the DevSecOps toolchains. However, you can use {{site.data.keyword.keymanagementserviceshort}} instead. Set the following variables for {{site.data.keyword.keymanagementserviceshort}}. These variables apply to the {{site.data.keyword.keymanagementserviceshort}} integration in all the DevSecOps toolchains that are created.

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `kp_name` | Name of the {{site.data.keyword.keymanagementserviceshort}} instance where the secrets are stored. | `string` | `"kp-compliance-secrets"` |
| `kp_location`  | {{site.data.keyword.cloud}} location or region that contains the {{site.data.keyword.keymanagementserviceshort}} instance. | `string` | `"us-south"` |
| `kp_resource_group` | The resource group that contains the {{site.data.keyword.keymanagementserviceshort}} instance for your secrets. | `string` | `"Default"` |
{: caption="{{site.data.keyword.keymanagementserviceshort}}" caption-side="top"}

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
{: caption="Prefixed forms" caption-side="top"}

### Switching between secrets providers
{: #devsecops-alm-switchsecret}

Secrets Manager is the default provider for the toolchains. Use the following variables to switch between {{site.data.keyword.keymanagementserviceshort}} and Secrets Manager for each toolchain.

Secrets providers can be switched across all the toolchains.

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `enable_key_protect` | Enable {{site.data.keyword.keymanagementserviceshort}} integrations. | `bool` | `false` |
| `enable_secrets_manager` | Enable the Secrets Manager integrations. | `bool` | `true` |
{: caption="Secrets providers" caption-side="top"}

Alternatively, you can switch secrets providers seperately for the different toolchains. The toolchain-specific variable defaults for the secrets providers, as outlined in the Table 10, are set to `false` by default. The variables are grouped under Key Protect and Secrets Manager. All the variables in a group take precendence if any variable in that group is changed from the default value.

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `ci_enable_key_protect` | Enables {{site.data.keyword.keymanagementserviceshort}} integration. | `bool` | `false` |
| `cd_enable_key_protect` | Uses the {{site.data.keyword.keymanagementserviceshort}} integration. | `bool` | `false` |
| `cc_enable_key_protect` | Enables {{site.data.keyword.keymanagementserviceshort}} integration. | `bool` | `false` |
| `ci_enable_secrets_manager`  | Enables Secrets Manager integration. | `bool` | `false` |
| `cd_enable_secrets_manager` | Uses the Secrets Manager integration. | `bool` | `false` |
| `cc_enable_secrets_manager` | Enables Secrets Manager integration. | `bool` | `false` |
{: caption="Switch secrets provider" caption-side="top"}

Set these variables to `true` to use a {{site.data.keyword.keymanagementserviceshort}} integration instead of Secrets Manager. Also, set the Secrets Manager values to `false` in this case so that an unnecessary integration is not created in the toolchain.

## Optional CI, CD, and CC variables
{: #devsecops-alm-optional}

If you are deploying with Code engine, see [Optional Code Engine CI and CD variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars#devsecops-alm-codeengine-optional)
{: note}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `deployment_repo_url` | The repository to clone deployment for DevSecOps toolchain template. | `string` | `""` |
| `ibmcloud_api` | {{site.data.keyword.cloud}} API Endpoint. | `string` | `"https://cloud.ibm.com"` |
{: caption="API key, secrets, and toolchain" caption-side="bottom"}
{: #api-parameters}
{: tab-title="API key, secrets, and toolchain"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `ci_app_group`  | Specify Git user or group for your application. | `string` | `""` |
| `ci_app_name`  | Name of the application image and inventory entry. | `string` | `"hello-compliance-app"` |
| `ci_app_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `ci_app_repo_git_token_secret_crn`| The CRN for the app repository Git Token. | `string` | `""` | no |
| `ci_app_repo_git_token_secret_name`  | Name of the Git token secret in the secret provider. | `string` | `""` |
| `ci_app_version`  | The version of the app to deploy. | `string` | `"v1"` |
| `ci_authorization_policy_creation` | Disable toolchain service to Secrets Manager Service authorization policy creation. | `string` | `""` |
| `ci_cluster_name` | Name of the Kubernetes cluster where the application is deployed. The cluster can be the same cluster that is used for prod. | `string` | `"mycluster-free"` |
| `ci_cluster_namespace` | Name of the Kubernetes cluster namespace where the application is deployed. | `string` | `"dev"` |
| `ci_compliance_base_image` | Pipeline baseimage to run most of the built-in pipeline code. | `string` | `""` |
| `ci_compliance_pipeline_branch` | The CI Pipeline Compliance Pipeline branch.| `string` | `""` |
| `ci_compliance_pipeline_pr_branch` | The PR Pipeline Compliance Pipeline branch.| `string` | `""` |
| `ci_compliance_pipeline_group`  | Specify user or group for compliance pipline repo. | `string` | `""` |
| `ci_compliance_pipeline_repo_auth_type`| Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `ci_compliance_pipeline_repo_git_token_secret_crn` | The CRN for the Compliance Pipeline repository Git Token. | `string` | `""` | no |
| `ci_compliance_pipeline_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `ci_cos_api_key_secret_crn` | The CRN for the Cloud Object Storage apikey. | `string` | `""` | no |
| `ci_cos_api_key_secret_name`  | Name of the Cloud Object Storage API key secret in the secret provider. | `string` | `""` |
| `ci_cos_endpoint`  | Cloud Object Storage endpoint name. | `string` | `""` |
| `ci_cra_bom_generate` | Set this flag to `1` to generate cra bom in CI pipeline.| `string` | `"1"` |
| `ci_cra_deploy_analysis` | Set this flag to `1` for cra deployment analysis to be done.| `string` | `"1"` |
| `ci_cra_generate_cyclonedx_format` | If set to 1, CRA also generates the BOM in cyclonedx format (defaults to 1). | `string` | `"1"` |
| `ci_cra_vulnerability_scan` | Set this flag to `1` and `ci-cra-bom-generate` to `1` for cra vulnerability scan in CI pipeline. If this value is set to 1 and `ci-cra-bom-generate` is set to `0`, the scan will be marked as `failure`| `string` | `"1"` |
| `pr_cra_bom_generate` | Set this flag to `1` to generate cra bom in CC pipeline.| `string` | `"1"` |
| `pr_cra_deploy_analysis` | Set this flag to `1` for cra deployment analysis to be done.| `string` | `"1"` |
| `pr_cra_vulnerability_scan` | Set this flag to `1` and `pr-cra-bom-generate` to `1` for cra vulnerability scan in CI pipeline. If this value is set to 1 and `pr-cra-bom-generate` is set to `0`, the scan will be marked as `failure`| `string` | `"1"` |
| `ci_custom_image_tag`  | The custom tag for the image in a comma-separated list. | `string` | `""` |
| `ci_deployment_target` | The deployment target, cluster, or code-engine. | `string` | `"cluster"` |
| `ci_dev_region`  | Region of the Kubernetes cluster where the application is deployed. | `string` | `"ibm:yp:us-south"` |
| `ci_dev_resource_group` | The cluster resource group. | `string` | `"Default"` |
| `ci_doi_environment`  | The DevOps Insights target environment. | `string` | `""` |
| `ci_doi_toolchain_id`  | DevOps Insights toolchain ID to link to. | `string` | `""` |
| `ci_doi_toolchain_id_pipeline_property`  | The DevOps Insights instance toolchain ID. | `string` | `""` |
| `ci_enable_pipeline_dockerconfigjson` | Adds the `pipeline-dockerconfigjson` property to the pipeline properties.| `bool` | `false` |
| `ci_enable_slack` | Set to `true` to create the integration. | `bool` | `false` |
| `ci_enable_pipeline_notifications` | When enabled, pipeline run events will be sent to the Event Notifications and Slack integrations in the enclosing toolchain. | `bool` | `false` |
| `ci_event_notifications` | To enable event notification, set event_notifications to 1 | `string` | `"0"` |
| `ci_evidence_group`  | Specify Git user or group for evidence repository. | `string` | `""` |
| `ci_evidence_repo_auth_type`  | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `ci_evidence_repo_git_token_secret_crn` | The CRN for the Evidence repository Git Token. | `string` | `""` | no |
| `ci_evidence_repo_git_token_secret_name`  | Name of the Git token secret in the secret provider. | `string` | `""` |
| `ci_gosec_private_repository_host` | Your private repository base URL. | `string` | `""` |
| `ci_gosec_private_repository_ssh_key_secret_crn` | The CRN for the GoSec repository secret. | `string` | `""` | no |
| `ci_gosec_repo_ssh_key_secret_group` | Secret group prefix for the gosec private repository ssh key secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `ci_gosec_repo_ssh_key_secret_name` | Name of the SSH key token for the private repository in the secret provider. | `string` | `"git-ssh-key"` |
| `ci_inventory_group`  | Specify Git user or group for inventory repository. | `string` | `""` |
| `ci_inventory_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `ci_inventory_repo_git_token_secret_crn` | The CRN for the Inventory repository Git Token. | `string` | `""` | no |
| `ci_inventory_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `ci_issues_group` | Specify Git user or group for issues repository. | `string` | `""` |
| `ci_issues_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `ci_issues_repo_git_token_secret_crn` | The CRN for the Issues repository Git Token. | `string` | `""` | no |
| `ci_issues_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `ci_link_to_doi_toolchain`  | Enable a link to a DevOps Insights instance in another toolchain. | `bool` | `false` |
| `ci_opt_in_dynamic_api_scan`  | Enable the OWASP Zap API scan. `1` enable or `0` disable. | `string` | `"1"` |
| `ci_opt_in_dynamic_scan`| To enable the OWASP Zap scan. `1` enable or `0` disable. | `string` | `"1"` |
| `ci_opt_in_dynamic_ui_scan` | To enable the OWASP Zap UI scan. `1` enable or `0` disable. | `string` | `"1"` |
| `ci_opt_in_gosec` | Enables gosec scans | `string` | `""` |
| `ci_opt_in_sonar` | Opt in for SonarQube. | `string` | `"1"` |
| `ci_pipeline_config_group` | Specify user or group for pipeline config repo. | `string` | `""` |
| `ci_pipeline_config_path` | The name and path of the `pipeline-config.yaml` file within the pipeline-config repo. | `string` | `".pipeline-config.yaml"` |
| `ci_pipeline_config_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `ci_pipeline_config_repo_branch` | Specify the branch that contains the custom `pipeline-config.yaml` file. | `string` | `""` |
| `ci_pipeline_config_repo_clone_from_url` | Specify a repository that contains a custom `pipeline-config.yaml` file. | `string` | `""` |
| `ci_pipeline_config_repo_existing_url`  | Specify a repository that contains a custom `pipeline-config.yaml` file. | `string` | `""` |
| `ci_pipeline_config_repo_git_token_secret_crn` | The CRN for the Pipeline Config repository Git Token. | `string` | `""` | no |
| `ci_pipeline_config_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `ci_pipeline_debug` | `0` by default. Set to `1` to enable debug logging. | `string` | `"0"` |
| `ci_pipeline_dockerconfigjson_secret_crn` | The CRN for Dockerconfig json secret. | `string` | `""` | no |
| `ci_pipeline_dockerconfigjson_secret_name` | Name of the pipeline docker config JSON secret in the secret provider.| `string` | `"pipeline_dockerconfigjson_secret_name"` |
| `ci_pipeline_git_tag` | The GIT tag within the pipeline definitions repository for the Compliance CI Pipeline. | `string` | `""` |
| `pr_pipeline_git_tag` | The GIT tag within the pipeline definitions repository for the Compliance PR Pipeline. | `string` | `""` |
| `ci_pipeline_git_token_secret_crn` | The CRN for the Git Token pipeline property. | `string` | `""` | no |
| `ci_pipeline_ibmcloud_api_key_secret_crn`| The CRN for the IBMCloud apikey. | `string` | `""` | no |
| `ci_pipeline_ibmcloud_api_key_secret_name` | Name of the Cloud API key secret in the secret provider. | `string` | `"ibmcloud-api-key"` |
| `ci_print_code_signing_certificate` | Set to `1` to enable printing of the public signing certificate in the logs. | `string` | `"1"` | no |
| `ci_registry_namespace` | A unique namespace within the {{site.data.keyword.cloud}} Container Registry region where the application image is stored. | `string` | `""` |
| `ci_registry_region` | The {{site.data.keyword.cloud}} Region where the {{site.data.keyword.cloud}} Container Registry namespace is to be created. | `string` | `"ibm:yp:us-south"` |
| `ci_repositories_prefix`  | Prefix name for the cloned compliance repos. For the repositories_prefix value only a-z, A-Z and 0-9 and the special characters `-_` are allowed. In addition the string must not end with a special character or have two consecutive special characters.| `string` | `"compliance"` |
| `ci_repository_properties` | Stringified JSON containing the repositories and triggers that get created in the CI toolchain pipelines. | `string` | `""` |
| `ci_signing_key_secret_crn` | The CRN for Signing Key secret. | `string` | `""` | no |
| `ci_signing_key_secret_name` | Name of the signing key secret in the secret provider. | `string` | `"signing_key"` |
| `ci_slack_channel_name` | The Slack channel that notifications are posted to. | `string` | `""` |
| `ci_slack_notifications` | The switch that turns the Slack integration on or off. | `string` | `"0"` |
| `ci_slack_pipeline_fail`  | Generate pipeline failed notifications. | `bool` | `true` |
| `ci_slack_pipeline_start` | Generate pipeline start notifications. | `bool` | `true` |
| `ci_slack_pipeline_success` | Generate pipeline succeeded notifications. | `bool` | `true` |
| `ci_slack_team_name` | The Slack team name, which is the word or phrase before .slack.com in the team URL. | `string` | `""` |
| `ci_slack_toolchain_bind`  | Generate tool added to toolchain notifications. | `bool` | `true` |
| `ci_slack_toolchain_unbind`  | Generate tool removed from toolchain notifications. | `bool` | `true` |
| `ci_slack_webhook_secret_name`  | Name of the webhook secret in the secret provider. | `string` | `""` |
| `ci_sm_instance_crn`| The CRN of the Secrets Manager instance for the CI toolchain. | `string` | `""` | no |
| `ci_sm_location` | {{site.data.keyword.cloud}} location or region that contains the Secrets Manager instance. | `string` | `""` |
| `ci_sm_name` | Name of the Secrets Manager instance where the secrets are stored. | `string` | `""` |
| `ci_sm_resource_group` | The resource group that contains the Secrets Manager instance. | `string` | `""` |
| `ci_sm_secret_group`  | Group in Secrets Manager for organizing or grouping secrets. | `string` | `""` |
| `ci_sonarqube_config` | Runs a SonarQube scan in an isolated Docker-in-Docker container (default configuration) or in an existing Kubernetes cluster (custom configuration). Options: default or custom. Default is default. | `string` | `"default"` |
| `ci_sonarqube_secret_crn` | The CRN for the SonarQube secret. | `string` | `""` | no |
| `ci_sonarqube_secret_name`| The name of the SonarQube secret. | `string` | `"sonarqube-secret"` | no |
| `ci_toolchain_description`  | Description for the CI toolchain. | `string` | `"Toolchain created with terraform template for DevSecOps CI Best Practices."` |
| `ci_toolchain_name`  | The name of the CI toolchain. | `string` | `"DevSecOps CI Toolchain - Terraform"` |
| `ci_toolchain_region` | The region that contains the CI toolchain. | `string` | `""` |
| `ci_toolchain_resource_group` | The resource group within which the toolchain is created. | `string` | `""` |
| `ci_trigger_git_name` | The name of the CI pipeline GIT trigger. | `string` | `"Git CI Trigger"` |
| `ci_trigger_git_enable` | Set to `true` to enable the CI pipeline Git trigger.| `bool` | `true` |
| `ci_trigger_timed_name` | The name of the CI pipeline Timed trigger. | `string` | `"Git CI Timed Trigger"` |
| `ci_trigger_timed_enable` | Set to `true` to enable the CI pipeline Timed trigger. | `bool` | `false` |
| `ci_trigger_timed_cron_schedule` | Only needed for timer triggers. Cron expression that indicates when this trigger will activate. Maximum frequency is every 5 minutes. The string is based on UNIX crontab syntax: minute, hour, day of month, month, day of week. Example: 0 *_/2 * * * - every 2 hours.| `string` | `"0 4 * * *"` |
| `ci_trigger_manual_name` | The name of the CI pipeline Manual trigger.| `string` | `"Manual Trigger"` |
| `ci_trigger_manual_enable` | Set to `true` to enable the CI pipeline Manual trigger. | `bool` | `true` |
| `ci_trigger_pr_git_name` | The name of the PR pipeline GIT trigger. | `string` | `"Git PR Trigger"` |
| `ci_trigger_pr_git_enable` | Set to `true` to enable the PR pipeline Git trigger.| `bool` | `true` |
| `ci_trigger_manual_pruner_name` | The name of the manual Pruner trigger.| `string` | `"Evidence Pruner Manual Trigger"` |
| `ci_trigger_manual_pruner_enable` | Set to `true` to enable the manual Pruner trigger. | `bool` | `true` |
| `ci_trigger_timed_pruner_name` | The name of the timed Pruner trigger. | `string` | `"Evidence Pruner Timed Trigger"` |
| `ci_trigger_timed_pruner_enable` | Set to `true` to enable the timed Pruner trigger. | `bool` | `false` |
| `ci_pipeline_ibmcloud_api_key_secret_group` | Secret group prefix for the pipeline ibmcloud API key secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `ci_signing_key_secret_group` | Secret group prefix for the signing key secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `ci_cos_api_key_secret_group` | Secret group prefix for the Cloud Object Storage API key secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `ci_slack_webhook_secret_crn` | The CRN for the Slack webhook secret. | `string` | `""` | no |
| `ci_slack_webhook_secret_group` | Secret group prefix for the Slack webhook secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `ci_pipeline_dockerconfigjson_secret_group` | Secret group prefix for the pipeline DockerConfigJson secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `ci_pipeline_git_token_secret_group` | Secret group prefix for the pipeline Git token secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `ci_app_repo_secret_group` | Secret group prefix for the App repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `ci_issues_repo_secret_group` | Secret group prefix for the Issues repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `ci_inventory_repo_secret_group` | Secret group prefix for the Inventory repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `ci_evidence_repo_secret_group` | Secret group prefix for the Evidence repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `ci_compliance_pipeline_repo_secret_group` | Secret group prefix for the Compliance Pipeline repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `ci_pipeline_config_repo_secret_group` | Secret group prefix for the Pipeline Config repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `ci_pipeline_git_token_secret_name` | Name of the pipeline Git token secret in the secret provider. | `string` | `"pipeline-git-token"` |
{: caption="Continuous integration" caption-side="bottom"}
{: #ci-parameters}
{: tab-title="Continuous integration"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `cd_app_version`  | The version of the app to deploy. | `string` | `"v1"` |
| `cd_authorization_policy_creation`  | Disable toolchain service to Secrets Manager Service authorization policy creation. | `string` | `""` |
| `cd_change_management_group` | Specify group for change management repository. | `string` | `""` |
| `cd_change_management_repo`  | This repository holds the change management requests created for the deployments. | `string` | `""` |
| `cd_change_management_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `cd_change_management_repo_git_provider` | Git provider for the change management repo. If not set will default to `hostedgit`. | `string` | `""` |
| `cd_change_management_repo_git_token_secret_crn`| The CRN for the Change Management repository Git Token. | `string` | `""` | no |
| `cd_change_management_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `cd_change_repo_clone_from_url` | Override the default management repo, which is cloned into the app repo. If the app repo exists, use `clone_if_not_exists` to leave the repo contents unchanged. | `string` | `""` |
| `cd_change_request_id` | The ID of an open change request. If this variable is set to `notAvailable`, a change request is automatically created by the continuous deployment pipeline. | `string` | `"notAvailable"` |
| `cd_cluster_name`| Name of the Kubernetes cluster where the application is deployed. | `string` | `"mycluster-free"` |
| `cd_cluster_namespace` | Name of the Kubernetes cluster namespace where the application is deployed. | `string` | `"prod"` |
| `cd_cluster_region` | Region of the Kubernetes cluster where the application is deployed. | `string` | `"ibm:yp:us-south"` |
| `cd_code_signing_cert_secret_crn` | The CRN for the public signing key cert in the secrets provider. | `string` | `""` |
| `cd_code_signing_cert_secret_group` | Secret group prefix for the pipeline Public signing key cert secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `cd_code_signing_cert_secret_name` | Name of the Cloud API key secret in the secret provider. | `string` | `"signing-certificate"` |
| `cd_compliance_base_image` | Pipeline baseimage to run most of the built-in pipeline code. | `string` | `""` |
| `cd_compliance_pipeline_branch` | The CD Pipeline Compliance Pipeline branch.| `string` | `""` |
| `cd_compliance_pipeline_group` | Specify user or group for compliance pipline repo. | `string` | `""` |
| `cd_compliance_pipeline_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `cd_compliance_pipeline_repo_git_token_secret_name`  | Name of the Git token secret in the secret provider. | `string` | `""` |
| `cd_cos_api_key_secret_crn` | The CRN for the Cloud Object Storage apikey. | `string` | `""` | no |
| `cd_cos_api_key_secret_name` | Name of the Cloud Object Storage API key secret in the secret provider. | `string` | `""` |
| `cd_cos_endpoint`  | Cloud Object Storage endpoint name. | `string` | `""` |
| `cd_customer_impact` | Custom impact of the change request. | `string` | `"no_impact"` |
| `cd_deployment_group` | Specify group for deployment. | `string` | `""` |
| `cd_deployment_repo_auth_type`  | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `cd_deployment_repo_clone_from_branch` | Used when `deployment_repo_clone_from_url` is provided, the default branch that is used by the CD build, usually either `main` or `master`. | `string` | `""` |
| `cd_deployment_repo_clone_from_url` | Override the default sample app by providing your own sample deployment URL, which is cloned into the app repo. If the app repo exists, use `clone_if_not_exists` to leave the repo contents unchanged. | `string` | `""` |
| `cd_deployment_repo_clone_to_git_id` | By default absent, otherwise use custom server GUID, or other options for `git_id` field in the browser UI. | `string` | `""` |
| `cd_deployment_repo_clone_to_git_provider` | By default `hostedgit`, otherwise use `githubconsolidated` or `gitlab`. | `string` | `""` |
| `cd_deployment_repo_existing_branch`  | Used when `deployment_repo_existing_url` is provided, the default branch that is by the CD build, usually either `main` or `master`. | `string` | `""` |
| `cd_deployment_repo_existing_git_id`  | By default absent, otherwise use custom server GUID, or other options for `git_id` field in the browser UI. | `string` | `""` |
| `cd_deployment_repo_existing_git_provider` | By default `hostedgit`, otherwise use `githubconsolidated` or `gitlab`. | `string` | `"hostedgit"` |
| `cd_deployment_repo_existing_url` | Override to bring your own existing deployment repository URL, which is used directly instead of cloning the default deployment sample. | `string` | `""` |
| `cd_deployment_repo_git_token_secret_crn` | The CRN for the Deployment repository Git Token. | `string` | `""` | no |
| `cd_deployment_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `cd_doi_environment` | DevOps Insights environment for DevSecOps CD deployment. | `string` | `""` |
| `cd_doi_toolchain_id` | DevOps Insights toolchain ID to link to. | `string` | `""` |
| `cd_emergency_label`  | Identifies the pull request as an emergency. | `string` | `"EMERGENCY"` |
| `cd_enable_slack` | Set to true to create the integration. | `bool` | `false` |
| `cd_enable_pipeline_notifications` | When enabled, pipeline run events will be sent to the Event Notifications and Slack integrations in the enclosing toolchain. | `bool` | `false` |
| `cd_event_notifications` | To enable event notification, set event_notifications to 1 | `string` | `"0"` |
| `cd_evidence_group` | Specify Git user or group for evidence repository. | `string` | `""` |
| `cd_evidence_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `cd_evidence_repo_git_token_secret_crn`| The CRN for the Evidence repository Git Token. | `string` | `""` | no |
| `cd_evidence_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `cd_inventory_group` | Specify Git user or group for inventory repository. | `string` | `""` |
| `cd_inventory_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `cd_inventory_repo_git_token_secret_crn` | The CRN for the Inventory repository Git Token. | `string` | `""` | no |
| `cd_inventory_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `cd_issues_group` | Specify Git user or group for issues repository. | `string` | `""` |
| `cd_issues_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `cd_issues_repo_git_token_secret_crn` | The CRN for the Issues repository Git Token. | `string` | `""` | no |
| `cd_issues_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `cd_link_to_doi_toolchain` | Enable a link to a DevOps Insights instance in another toolchain, true or false. | `bool` | `true` |
| `cd_merge_cra_sbom` | Merge the SBOM | `string` | `"1"` |
| `cd_pipeline_config_group` | Specify user or group for pipeline config repo. | `string` | `""` |
| `cd_pipeline_config_path` | The name and path of the pipeline-config.yaml file within the pipeline-config repo. | `string` | `".pipeline-config.yaml"` |
| `cd_pipeline_config_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `cd_pipeline_config_repo_branch` | Specify the branch that contains the custom pipeline-config.yaml file. | `string` | `""` |
| `cd_pipeline_config_repo_clone_from_url` | Specify a repository that contains a custom pipeline-config.yaml file. | `string` | `""` |
| `cd_pipeline_config_repo_existing_url` | Specify a repository that contains a custom pipeline-config.yaml file. | `string` | `""` |
| `cd_pipeline_config_repo_git_token_secret_crn` | The CRN for the Config repository Git Token. | `string` | `""` | no |
| `cd_pipeline_config_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `cd_pipeline_debug`  | `0` by default. Set to `1` to enable debug logging. | `string` | `"0"` |
| `cd_pipeline_git_token_secret_crn`| The CRN for the Git Token secret in the pipeline properties. | `string` | `""` | no |
| `cd_pipeline_ibmcloud_api_key_secret_crn` | The CRN for the pipeline apikey. | `string` | `""` | no |
| `cd_pipeline_ibmcloud_api_key_secret_name` | Name of the Cloud API key secret in the secret provider. | `string` | `"ibmcloud-api-key"` |
| `cd_pipeline_properties` | Stringified JSON containing the properties for the CD toolchain pipelines. | `string` | `""` |
| `cd_repositories_prefix`  | Prefix name for the cloned compliance repos. | `string` | `"compliance"` |
| `cd_satellite_cluster_group` | The Satellite cluster group | `string` | `""` |
| `cd_scc_enable_scc` | Enable the SCC integration. | `bool` | `true` |
| `cd_scc_integration_name`  | The name of the SCC integration name. | `string` | `"Security and Compliance"` |
| `cd_slack_channel_name`  | The Slack channel that notifications are posted to. | `string` | `""` |
| `cd_slack_notifications` | The switch to turn the Slack integration on or off. | `string` | `"0"` |
| `cd_slack_pipeline_fail` | Generate pipeline failed notifications. | `bool` | `true` |
| `cd_slack_pipeline_start` | Generate pipeline start notifications. | `bool` | `true` |
| `cd_slack_pipeline_success` | Generate pipeline succeeded notifications. | `bool` | `true` |
| `cd_slack_team_name`  | The Slack team name, which is the word or phrase before .slack.com in the team URL. | `string` | `""` |
| `cd_slack_toolchain_bind`  | Generate tool added to toolchain notifications. | `bool` | `true` |
| `cd_slack_toolchain_unbind` | Generate tool removed from toolchain notifications. | `bool` | `true` |
| `cd_slack_webhook_secret_name` | Name of the webhook secret in the secret provider. | `string` | `""` |
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
| `cd_toolchain_region`  | The region that contains the CD toolchain. | `string` | `""` |
| `cd_toolchain_resource_group`  | Resource group within which toolchain is created. | `string` | `""` |
| `cd_trigger_git_name` | The name of the CD pipeline GIT trigger. | `string` | `"Git CD Trigger"` |
| `cd_trigger_git_enable` | Set to `true` to enable the CD pipeline Git trigger.| `bool` | `true` |
| `cd_trigger_timed_name` | The name of the CD pipeline Timed trigger. | `string` | `"Git CD Timed Trigger"` |
| `cd_trigger_timed_enable` | Set to `true` to enable the CI pipeline Timed trigger. | `bool` | `false` |
| `cd_trigger_timed_cron_schedule` | Only needed for timer triggers. Cron expression that indicates when this trigger will activate. Maximum frequency is every 5 minutes. The string is based on UNIX crontab syntax: minute, hour, day of month, month, day of week. Example: 0 *_/2 * * * - every 2 hours.| `string` | `"0 4 * * *"` |
| `cd_trigger_manual_name` | The name of the CD pipeline Manual trigger.| `string` | `"Manual Trigger"` |
| `cd_trigger_manual_enable` | Set to `true` to enable the CD pipeline Manual trigger. | `bool` | `true` |
| `cd_trigger_manual_promotion_name` | The name of the CD pipeline Manual Promotion trigger.| `string` | `"Manual Promotion Trigger"` |
| `cd_trigger_manual_promotion_enable` | Set to `true` to enable the CD pipeline Manual Promotion trigger.| `bool` | `true` |
| `cd_trigger_manual_pruner_name` | The name of the manual Pruner trigger.| `string` | `"Evidence Pruner Manual Trigger"` |
| `cd_trigger_manual_pruner_enable` | Set to `true` to enable the manual Pruner trigger. | `bool` | `true` |
| `cd_trigger_timed_pruner_name` | The name of the timed Pruner trigger. | `string` | `"Evidence Pruner Timed Trigger"` |
| `cd_trigger_timed_pruner_enable` | Set to `true` to enable the timed Pruner trigger. | `bool` | `false` |
| `cd_scc_enable_scc` | Adds the SCC tool integration to the toolchain. | `bool` | `true` |
| `cd_slack_webhook_secret_crn` | The CRN for the Slack webhook secret. | `string` | `""` | no |
| `cd_slack_webhook_secret_group` | Secret group prefix for the Slack webhook secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cd_sm_instance_crn`| The CRN of the Secrets Manager instance. | `string` | `""` | no |
| `cd_change_management_repo_secret_group` | Secret group prefix for the Change Management repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cd_deployment_repo_secret_group` | Secret group prefix for the Deployment repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cd_issues_repo_secret_group` | Secret group prefix for the Issues repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cd_inventory_repo_secret_group` | Secret group prefix for the Inventory repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cd_evidence_repo_secret_group` | Secret group prefix for the Evidence repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cd_compliance_pipeline_repo_secret_group` |Secret group prefix for the Compliance Pipeline repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cd_pipeline_config_repo_secret_group` | Secret group prefix for the Pipeline Config repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `cd_cos_api_key_secret_group` | Secret group prefix for the Cloud Object Storage API key secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cd_pipeline_ibmcloud_api_key_secret_group` | Secret group prefix for the pipeline ibmcloud API key secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cd_pipeline_git_tag` | The GIT tag within the pipeline definitions repository for the Compliance CD Pipeline. | `string` | `""` |
| `cd_pipeline_git_token_secret_group` | Secret group prefix for the pipeline Git token secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cd_code_signing_cert` | The base64 encoded GPG public key. | `string` | `""` |
| `cd_pipeline_git_token_secret_name` | Name of the pipeline Git token secret in the secret provider. | `string` | `"pipeline-git-token"` |
| `change_management_existing_url` | The URL for an existing Change Management repository. | `string` | `""` |
| `change_management_repo_git_id` | Set this value to `github` for github.com, or to the ID of a custom GitHub Enterprise server. | `string` | `""` |
{: caption="Continuous deployment" caption-side="bottom"}
{: #cd-parameters}
{: tab-title="Continuous deployment"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `cc_app_group` | Specify user or group for app repo. | `string` | `""` |
| `cc_app_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `oauth` |
| `cc_app_repo_branch` | The default branch of the app repo. | `string` | `"master"` |
| `cc_app_repo_git_id` | By default absent, otherwise use the custom server GUID, or other options for `git_id` field in the browser UI.  | `string` | `""` |
| `cc_app_repo_git_provider` | The type of the Git provider. | `string` | `"hostedgit"` |
| `cc_app_repo_git_token_secret_crn` | The CRN for the app repository Git Token. | `string` | `""` | no |
| `cc_app_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `cc_app_repo_url` | The Git URL for the application repository. | `string` | `""` |
| `cc_artifactory_token_secret_crn` | The CRN for the Artifactory secret. | `string` | `""` | no |
| `cc_authorization_policy_creation` | Disable the toolchain service to Secrets Manager service authorization policy creation. | `string` | `""` |
| `cc_compliance_base_image`  | Pipeline baseimage to run most of the built-in pipeline code. | `string` | `""` |
| `cc_compliance_pipeline_branch` | The CC Pipeline Compliance Pipeline branch.| `string` | `""` |
| `cc_compliance_pipeline_group` | Specify user or group for compliance pipline repo. | `string` | `""` |
| `cc_compliance_pipeline_repo_auth_type` | Select the authentication method that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `cc_compliance_pipeline_repo_git_token_secret_crn` | The CRN for the Compliance Pipeline repository Git Token. | `string` | `""` | no |
| `cc_compliance_pipeline_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `cc_cos_api_key_secret_crn`| The CRN for the Cloud Object Storage apikey. | `string` | `""` | no |
| `cc_cos_api_key_secret_name` | Name of the Cloud Object Storage API key secret in the secret provider. | `string` | `""` |
| `cc_cos_endpoint` | Cloud Object Storage endpoint name. | `string` | `""` |
| `cc_cra_bom_generate` | Set this flag to `1` to generate cra bom in CC pipeline.| `string` | `"1"` |
| `cc_cra_deploy_analysis` | Set this flag to `1` for cra deployment analysis to be done.| `string` | `"1"` |
| `cc_cra_vulnerability_scan` | Set this flag to `1` and `cc-cra-bom-generate` to `1` for cra vulnerability scan in CI pipeline. If this value is set to 1 and `cc-cra-bom-generate` is set to `0`, the scan will be marked as `failure`| `string` | `"1"` |
| `cc_doi_environment`  | DevOps Insights environment for DevSecOps CD deployment. | `string` | `""` |
| `cc_doi_toolchain_id`  | DevOps Insights toolchain ID to link to. | `string` | `""` |
| `cc_enable_pipeline_dockerconfigjson` | Adds the pipeline-dockerconfigjson property to the pipeline properties.| `bool` | `false` |
| `cc_enable_slack` | Create the Slack integration. | `bool` | `false` |
| `cc_environment_tag` | Tag name that represents the target environment in the inventory. Example: `prod_latest`. | `string` | `"prod_latest"` |
| `cc_enable_pipeline_notifications` | When enabled, pipeline run events will be sent to the Event Notifications and Slack integrations in the enclosing toolchain. | `bool` | `false` |
| `cc_event_notifications` | To enable event notification, set event_notifications to 1 | `string` | `"0"` |
| `cc_evidence_group`  | Specify Git user or group for evidence repository. | `string` | `""` |
| `cc_evidence_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `cc_evidence_repo_git_token_secret_crn`| The CRN for the Evidence repository Git Token. | `string` | `""` | no |
| `cc_evidence_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `cc_inventory_group`  | Specify Git user or group for inventory repository. | `string` | `""` |
| `cc_inventory_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `cc_inventory_repo_git_token_secret_crn` | The CRN for the Inventory repository Git Token. | `string` | `""` | no |
| `cc_inventory_repo_git_token_secret_name`  | Name of the Git token secret in the secret provider. | `string` | `""` |
| `cc_issues_group` | Specify Git user or group for issues repository. | `string` | `""` |
| `cc_issues_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `cc_issues_repo_git_token_secret_crn` | The CRN for the Issues repository Git Token. | `string` | `""` | no |
| `cc_issues_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `cc_gosec_private_repository_host` | Your private repository base URL. | `string` | `""` |
| `cc_gosec_private_repository_ssh_key_secret_crn` | The CRN for the Deployment repository Git Token. | `string` | `""` | no |
| `cc_gosec_repo_ssh_key_secret_group` | Secret group prefix for the gosec private repository ssh key secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `cc_gosec_repo_ssh_key_secret_name` | Name of the SSH key token for the private repository in the secret provider. | `string` | `"git-ssh-key"` |
| `cc_link_to_doi_toolchain` | Enable a link to a DevOps Insights instance in another toolchain, true or false. | `bool` | `true` |
| `cc_opt_in_auto_close`  | Enable auto-closing of issues that come from vulnerabilities when the vulnerability is no longer detected by the CC pipeline run. | `string` | `"1"` |
| `cc_opt_in_dynamic_api_scan` | Enable the OWASP Zap API scan. `1` enable or `0` disable. | `string` | `""` |
| `cc_opt_in_dynamic_scan`  | Enable the OWASP Zap scan. `1` enable or `0` disable. | `string` | `""` |
| `cc_opt_in_dynamic_ui_scan` | Enable the OWASP Zap UI scan. `1` enable or `0` disable. | `string` | `""` |
| `cc_opt_in_gosec` | Enables gosec scans | `string` | `""` |
| `cc_pipeline_config_group` | Specify user or group for pipeline config repo. | `string` | `""` |
| `cc_pipeline_config_path` | The name and path of the `pipeline-config.yaml` file within the pipeline-config repo. | `string` | `".pipeline-config.yaml"` |
| `cc_pipeline_config_repo_auth_type` | Select the method of authentication that is used to access the Git provider. `oauth` or `pat`. | `string` | `""` |
| `cc_pipeline_config_repo_branch` | Specify the branch that contains the custom `pipeline-config.yaml` file. | `string` | `""` |
| `cc_pipeline_config_repo_clone_from_url` | Specify a repository that contains a custom `pipeline-config.yaml` file. | `string` | `""` |
| `cc_pipeline_config_repo_existing_url` | Specify a repository that contains a custom `pipeline-config.yaml` file. | `string` | `""` |
| `cc_pipeline_config_repo_git_token_secret_crn` | The CRN for the Pipeline Config repository Git Token. | `string` | `""` | no |
| `cc_pipeline_config_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `cc_pipeline_debug` | `0` by default. Set to `1` to enable debug logging. | `string` | `"0"` |
| `cc_pipeline_dockerconfigjson_secret_crn` | The CRN for the Dockerconfig json secret. | `string` | `""` | no |
| `cc_pipeline_dockerconfigjson_secret_name` | Name of the pipeline docker config JSON secret in the secret provider.| `string` | `"pipeline_dockerconfigjson_secret_name"` |
| `cc_pipeline_doi_api_key_secret_crn` | The CRN for the pipeline DOI apikey. | `string` | `""` | no |
| `cc_pipeline_git_tag` | The GIT tag within the pipeline definitions repository for the Compliance CC Pipeline. | `string` | `""` |
| `cc_pipeline_git_token_secret_crn` | The CRN for pipeline Git token property. | `string` | `""` | no |
| `cc_pipeline_ibmcloud_api_key_secret_crn` | The CRN for the IBMCloud apikey. | `string` | `""` | no |
| `cc_pipeline_ibmcloud_api_key_secret_name` | Name of the Cloud API key secret in the secret provider. | `string` | `"ibmcloud-api-key"` |
| `cc_pipeline_properties` | Stringified JSON containing the properties for the CC toolchain pipelines. | `string` | `""` |
| `cc_repositories_prefix` | The prefix for the compliance repositories. | `string` | `"compliance"` |
| `cc_scc_enable_scc` | Enable the SCC integration. | `bool` | `true` |
| `cc_scc_integration_name` | The SCC integration name. | `string` | `"Security and Compliance"` |
| `cc_slack_channel_name` | The Slack channel that notifications are posted to. | `string` | `""` |
| `cc_slack_notifications` | Turns the Slack integration on or off. | `string` | `"0"` |
| `cc_slack_pipeline_fail` | Generate pipeline failed notifications. | `bool` | `true` |
| `cc_slack_pipeline_start` | Generate pipeline start notifications. | `bool` | `true` |
| `cc_slack_pipeline_success` | Generate pipeline succeeded notifications. | `bool` | `true` |
| `cc_slack_team_name` | The Slack team name, which is the word or phrase before `.slack.com` in the team URL. | `string` | `""` |
| `cc_slack_toolchain_bind` | Generate tool added to toolchain notifications. | `bool` | `true` |
| `cc_slack_toolchain_unbind`  | Generate tool removed from toolchain notifications. | `bool` | `true` |
| `cc_slack_webhook_secret_crn` | The CRN for Slack Webhook secret. | `string` | `""` | no |
| `cc_slack_webhook_secret_name`  | Name of the webhook secret in the secret provider. | `string` | `""` |
| `cc_sm_instance_crn`| The CRN of the Secrets Manager instance. | `string` | `""` | no |
| `cc_sm_location`  | {{site.data.keyword.cloud}} location or region that contains the Secrets Manager instance. | `string` | `""` |
| `cc_sm_name` | Name of the Secrets Manager instance where the secrets are stored. | `string` | `""` |
| `cc_sm_resource_group`  | The resource group that contains the Secrets Manager instance for your secrets. | `string` | `""` |
| `cc_sm_secret_group`  | Group in Secrets Manager for organizing or grouping secrets. | `string` | `""` |
| `cc_sonarqube_config` | Runs a SonarQube scan in an isolated Docker-in-Docker container (default configuration) or in an existing Kubernetes cluster (custom configuration). Options: default or custom. Default is default. | `string` | `"default"` |
| `cc_sonarqube_secret_crn` | The CRN for the SonarQube secret. | `string` | `""` | no |
| `cc_sonarqube_secret_name` | The name of the SonarQube secret. | `string` | `""` | no |
| `cc_toolchain_description` | Description of the CC toolchain. | `string` | `"Toolchain created with terraform template for DevSecOps CC Best Practices."` |
| `cc_toolchain_name` | The name of the CC toolchain. | `string` | `"DevSecOps CC Toolchain - Terraform"` |
| `cc_toolchain_region` | The region that contains the CI toolchain. | `string` | `""` |
| `cc_toolchain_resource_group`  | Resource group within which the toolchain is created. | `string` | `""` |
| `cc_trigger_timed_name` | The name of the CC pipeline Timed trigger. | `string` | `"CC Timed Trigger"` |
| `cc_trigger_timed_enable` | Set to `true` to enable the CC pipeline Timed trigger. | `bool` | `false` |
| `cc_trigger_timed_cron_schedule` | Only needed for timer triggers. Cron expression that indicates when this trigger will activate. Maximum frequency is every 5 minutes. The string is based on UNIX crontab syntax: minute, hour, day of month, month, day of week. Example: 0 *_/2 * * * - every 2 hours.| `string` | `"0 4 * * *"` |
| `cc_trigger_manual_name` | The name of the CC pipeline Manual trigger.| `string` | `"CC Manual Trigger"` |
| `cc_trigger_manual_enable` | Set to `true` to enable the CI pipeline Manual trigger. | `bool` | `true` |
| `cc_trigger_manual_pruner_name` | The name of the manual Pruner trigger.| `string` | `"Evidence Pruner Manual Trigger"` |
| `cc_trigger_manual_pruner_enable` | Set to `true` to enable the manual Pruner trigger. | `bool` | `true` |
| `cc_trigger_timed_pruner_name` | The name of the timed Pruner trigger. | `string` | `"Evidence Pruner Timed Trigger"` |
| `cc_trigger_timed_pruner_enable` | Set to `true` to enable the timed Pruner trigger. | `bool` | `false` |
| `cc_scc_enable_scc` | Adds the SCC tool integration to the toolchain. | `bool` | `true` |
| `cc_pipeline_ibmcloud_api_key_secret_group` | Secret group prefix for the pipeline ibmcloud API key secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cc_cos_api_key_secret_group` | Secret group prefix for the Cloud Object Storage API key secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cc_slack_webhook_secret_group` | Secret group prefix for the Slack webhook secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cc_pipeline_dockerconfigjson_secret_group` | Secret group prefix for the pipeline DockerConfigJson secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cc_app_repo_secret_group` |Secret group prefix for the App repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cc_issues_repo_secret_group` | Secret group prefix for the Issues repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cc_inventory_repo_secret_group` |Secret group prefix for the Inventory repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cc_evidence_repo_secret_group` | Secret group prefix for the Evidence repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cc_compliance_pipeline_repo_secret_group` | Secret group prefix for the Compliance Pipeline repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
| `cc_pipeline_config_repo_secret_group` | Secret group prefix for the Pipeline Config repo secret. Defaults to `sm_secret_group` if not set. Only used with `Secrets Manager`.| `string` | `""` |
{: caption="Continuous compliance" caption-side="bottom"}
{: #cc-parameters}
{: tab-title="Continuous compliance"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

## Optional Code Engine CI and CD variables
{: #devsecops-alm-codeengine-optional}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `code_engine_project` | The name of the Code Engine project to use. Created if it does not exist. Applies to both the CI and CD toolchains. To set individually use `ci_code_engine_project` and `cd_code_engine_project`. | `string` | `""` |
| `code_engine_project_prefix` | A string that will be prefixed to`ci_code_engine_project` and `cd_code_engine_project`. | `string` | `""` |
| `ci_code_engine_app_concurrency` | The maximum number of requests that can be processed concurrently per instance. | `string` | `"100"` |
| `ci_code_engine_app_deployment_timeout` | The maximum timeout for the application deployment. | `string` | `"300"` |
| `ci_code_engine_app_max_scale` | The maximum number of instances that can be used for this application. If you set this value to 0, the application scales as needed. The application scaling is limited only by the instances per the resource quota for the project of your application. | `string` | `"1"` |
| `ci_code_engine_app_min_scale` | The minimum number of instances that can be used for this application. This option is useful to ensure that no instances are running when not needed. | `string` | `"0"` |
| `ci_code_engine_app_port` | The port where the application listens. The format is `[NAME:]PORT`, where `[NAME:]` is optional. If `[NAME:]` is specified, valid values are `h2c`, or `http1`. When `[NAME:]` is not specified or is `http1`, the port uses `HTTP/1.1`. When `[NAME:]` is `h2c`, the port uses unencrypted `HTTP/2`. | `string` | `"8080"` |
| `ci_code_engine_app_visibility` | The visibility for the application. Valid values are public, private and project. Setting a visibility of public means that your app can receive requests from the public internet or from components within the Code Engine project. Setting a visibility of private means that your app is not accessible from the public internet and network access is only possible from other IBM Cloud using Virtual Private Endpoints (VPE) or Code Engine components that are running in the same project. Visibility can only be private if the project supports application private visibility. Setting a visibility of project means that your app is not accessible from the public internet and network access is only possible from other Code Engine components that are running in the same project. | `string` | `"public"` |
| `ci_code_engine_binding_resource_group` | The name of a resource group to use for authentication for the service bindings of the Code Engine project. A service ID is created with Operator and Manager roles for all services in this resource group. Use '*' to specify all resource groups in this account. | `string` | `""` |
| `ci_code_engine_build_size` | The size to use for the build, which determines the amount of resources used. Valid values include `small`, `medium`, `large`, `xlarge`. | `string` | `"large"` |
| `ci_code_engine_build_strategy` | The build strategy for the Code Engine component. It can be `dockerfile` or `buildpacks`. | `string` | `"dockerfile"` |
| `ci_code_engine_build_timeout` | The amount of time, in seconds, that can pass before the build run must succeed or fail. | `string` | `"1200"` |
| `ci_code_engine_build_use_native_docker` | Property to opt-in for using native docker build capabilities as opposed to use Code Engine build to containerize the source. Note this setting only takes effect if the build-strategy is set to `dockerfile`. Valid values are `true` and `false`. | `string` | `"false"` |
| `ci_code_engine_context_dir` | The directory in the repository that contains the buildpacks file or the Dockerfile. | `string` | `"."` |
| `ci_code_engine_cpu` | The amount of CPU set for the instance of the application or job. | `string` | `"0.25"` |
| `ci_code_engine_deployment_type` | type of Code Engine component to create/update as part of deployment. It can be either `application` or `job`. | `string` | `"application"` |
| `ci_code_engine_dockerfile` | The path to the `Dockerfile`. Specify this option only if the name is other than `Dockerfile` | `string` | `"Dockerfile"` |
| `ci_code_engine_env_from_configmaps` | Semi-colon separated list of configmaps to set environment variables. | `string` | `""` |
| `ci_code_engine_env_from_secrets` | Semi-colon separated list of secrets to set environment variables. | `string` | `""` |
| `ci_code_engine_ephemeral_storage` | The amount of ephemeral storage to set for the instance of the application or for the runs of the job. Use M for megabytes or G for gigabytes. | `string` | `"0.4G"` |
| `ci_code_engine_image_name` | Name of the image that is built. | `string` | `"code-engine-compliance-app"` |
| `ci_code_engine_job_instances` | Specifies the number of instances that are used for runs of the job. When you use this option, the system converts to array indices. For example, if you specify instances of 5, the system converts to array-indices of 0 - 4. This option can only be specified if the --array-indices option is not specified. The default value is 1. | `string` | `"1"` |
| `ci_code_engine_job_maxexecutiontime` | The maximum execution time in seconds for runs of the job. | `string` | `"7200"` |
| `ci_code_engine_job_retrylimit` | The number of times to rerun an instance of the job before the job is marked as failed. | `string` | `"3"` |
| `ci_code_engine_memory` | The amount of memory set for the instance of the application or job. Use M for megabytes or G for gigabytes. | `string` | `"0.5G"` |
| `ci_code_engine_project` | The name of the Code Engine project to use for the CI pipeline build. The project is created if it does not already exist. | `string` | `"Sample_CI_Project"` |
| `ci_code_engine_project_prefix` | A string that will be prefixed to `ci_code_engine_project`. This takes precedence over values set in `code_engine_project_prefix`. | `string` | `""` |
| `ci_code_engine_region` | The region to create/lookup for the Code Engine project. | `string` | `""` |
| `ci_code_engine_registry_domain` | The container registry URL domain that is used to build and tag the image. Useful when using private-endpoint container registry. | `string` | `""` |
| `ci_code_engine_remove_refs` | Remove references to unspecified configuration resources (configmap/secret) references (pulled from env-from-configmaps, env-from-secrets along with auto-managed by CD). | `string` | `"false"` |
| `ci_code_engine_resource_group` | The resource group of the Code Engine project. | `string` | `""` |
| `ci_code_engine_service_bindings` | JSON array including service name(s) as a simple JSON string. | `string` | `""` |
| `ci_code_engine_source` | The path to the location of code to build in the repository. Defaults to the root of source code repository. | `string` | `""` |
| `ci_code_engine_wait_timeout` | The maximum timeout for the CLI operation to wait. | `string` | `"1300"` |
{: caption="Optional variables for Code Engine deployment" caption-side="bottom"}
{: #ci-parameters-codeengine}
{: tab-title="Continuous integration"}
{: tab-group="codeengine-variables"}
{: class="simple-tab-table"}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `cd_code_engine_app_concurrency` | The maximum number of requests that can be processed concurrently per instance. | `string` | `"100"` |
| `cd_code_engine_app_deployment_timeout` | The maximum timeout for the application deployment. | `string` | `"300"` |
| `cd_code_engine_app_max_scale` | The maximum number of instances that can be used for this application. If you set this value to 0, the application scales as needed. The application scaling is limited only by the instances per the resource quota for the project of your application. | `string` | `"1"` |
| `cd_code_engine_app_min_scale` | The minimum number of instances that can be used for this application. This option is useful to ensure that no instances are running when not needed. | `string` | `"0"` |
| `cd_code_engine_app_port` | The port where the application listens. The format is `[NAME:]PORT`, where `[NAME:]` is optional. If `[NAME:]` is specified, valid values are `h2c`, or `http1`. When `[NAME:]` is not specified or is `http1`, the port uses `HTTP/1.1`. When `[NAME:]` is `h2c`, the port uses unencrypted `HTTP/2`. | `string` | `"8080"` |
| `cd_code_engine_app_visibility` | The visibility for the application. Valid values are public, private and project. Setting a visibility of public means that your app can receive requests from the public internet or from components within the Code Engine project. Setting a visibility of private means that your app is not accessible from the public internet and network access is only possible from other IBM Cloud using Virtual Private Endpoints (VPE) or Code Engine components that are running in the same project. Visibility can only be private if the project supports application private visibility. Setting a visibility of project means that your app is not accessible from the public internet and network access is only possible from other Code Engine components that are running in the same project. | `string` | `"public"` |
| `cd_code_engine_binding_resource_group` | The name of a resource group to use for authentication for the service bindings of the Code Engine project. A service ID is created with Operator and Manager roles for all services in this resource group. Use '*' to specify all resource groups in this account. | `string` | `""` |
| `cd_code_engine_cpu` | The amount of CPU set for the instance of the application or job. | `string` | `"0.25"` |
| `cd_code_engine_deployment_type` | type of Code Engine component to create/update as part of deployment. It can be either `application` or `job`. | `string` | `"application"` |
| `cd_code_engine_env_from_configmaps` | Semi-colon separated list of configmaps to set environment variables. | `string` | `""` |
| `cd_code_engine_env_from_secrets` | Semi-colon separated list of secrets to set environment variables. | `string` | `""` |
| `cd_code_engine_ephemeral_storage` | The amount of ephemeral storage to set for the instance of the application or for the runs of the job. Use M for megabytes or G for gigabytes. | `string` | `"0.4G"` |
| `cd_code_engine_job_instances` | Specifies the number of instances that are used for runs of the job. When you use this option, the system converts to array indices. For example, if you specify instances of 5, the system converts to array-indices of 0 - 4. This option can only be specified if the --array-indices option is not specified. The default value is 1. | `string` | `"1"` |
| `cd_code_engine_job_maxexecutiontime` | The maximum execution time in seconds for runs of the job. | `string` | `"7200"` |
| `cd_code_engine_job_retrylimit` | The number of times to rerun an instance of the job before the job is marked as failed. | `string` | `"3"` |
| `cd_code_engine_memory` | The amount of memory set for the instance of the application or job. Use M for megabytes or G for gigabytes. | `string` | `"0.5G"` |
| `cd_code_engine_project` | The name of the Code Engine project to use for the CD pipeline promoted code. The project is created if it does not already exist. | `string` | `"Sample_CD_Project"` |
| `cd_code_engine_project_prefix` | A string that will be prefixed to `cd_code_engine_project`. This takes precedence over values set in `code_engine_project_prefix`. | `string` | `""` |
| `cd_code_engine_region` | The region to create/lookup for the Code Engine project. | `string` | `""` |
| `cd_code_engine_remove_refs` | Remove references to unspecified configuration resources (configmap/secret) references (pulled from env-from-configmaps, env-from-secrets along with auto-managed by CD). | `string` | `"false"` |
| `cd_code_engine_resource_group` | The resource group of the Code Engine project. | `string` | `""` |
| `cd_code_engine_service_bindings` | JSON array including service name(s) as a simple JSON string. | `string` | `""` |
| `cd_code_signing_cert` | The base64 encoded GPG public key. | `string` | `""` |
| `cd_code_engine_service_bindings` | JSON array including service name(s) as a simple JSON string. | `string` | `""` |
{: caption="Optional variables for Code Engine deployment" caption-side="bottom"}
{: #cd-parameters-codeengine}
{: tab-title="Continuous deployment"}
{: tab-group="codeengine-variables"}
{: class="simple-tab-table"}

## Output variables
{: #devsecops-alm-output}

| Name | Description |
|------|-------------|
| `app_repo_url` | The App repo URL. |
| `compliance_cc_toolchain_id` | The ID of the compliance CC toolchain. |
| `compliance_cd_toolchain_id` | The ID of the compliance CD toolchain. |
| `compliance_ci_toolchain_id` | The ID of the compliance CI toolchain. |
| `compliance_cc_toolchain_url` | The Compliance CC Toolchain URL. |
| `compliance_cd_toolchain_url` | The Compliance CD Toolchain URL. |
| `compliance_ci_toolchain_url` | The Compliance CI Toolchain URL. |
| `evidence_repo_url` | The Evidence Repo URL. |
| `inventory_repo_url` | The Inventory Repo URL. |
| `issues_repo_url` | The Issues Repo URL. |
| `secrets_manager_instance_id` | The Secrets Manage Instance ID |
| `key_protect_instance_id` | The Key Protect Instance ID |
{: caption="Outputs" caption-side="top"}
