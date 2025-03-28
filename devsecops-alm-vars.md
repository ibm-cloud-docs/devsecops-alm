---

copyright:
  years: 2025
lastupdated: "2025-03-24"

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
| `toolchain_name` | This variable specifies the root name for the CI, CD and CC toolchain names. A fixed suffix will automatically be appended. Setting `DevSecOps` will generate toolchains with the names `DevSecOps-CI-Toolchain`,  `DevSecOps-CD-Toolchain` and `DevSecOps-CC-Toolchain`. The full name of each toolchain can be set independently using `ci_toolchain_name`, `cd_toolchain_name`, and `cc_toolchain_name`. | `string` | `"DevSecOps"` |
| `toolchain_region` | The region identifier that will be used, by default, for all resource creation and service instance lookup. This can be overridden on a per resource/service basis. | `string` | `"us-south"` |
| `toolchain_resource_group` | The resource group that will be used, by default, for all resource creation and service instance lookups. This can be overridden on a per resource/service basis. | `string` | `"Default"` |
| `registry_namespace` | A unique namespace within the IBM Cloud Container Registry region where the application image is stored. | `string` | `""` |
| `sm_location` | The region hosting the Secrets Manager instance. This applies to the CI, CD and CC Secret Manager integrations. | `string` | `"us-south"` |
| `sm_name` | The name of an existing Secret Managers instance. This applies to the CI, CD and CC Secret Manager integrations. | `string` | `"sm-instance"` |
| `sm_resource_group` | The name of the existing resource group containing the Secrets Manager instance for your secrets.. This applies to the CI, CD and CC Secret Manager integrations. | `string` | `"Default"` |
| `sm_secret_group` | The Secrets Manager secret group containing the secrets for the DevSecOps pipelines. This applies to the CI, CD and CC Secret Manager integrations. | `string` | `"Default"` |
| `ci_pipeline_properties` | This JSON represents the pipeline properties belonging to the both the CI and PR pipelines in the CI toolchain. Each element in the JSON represents a seperate pipeline property. Three attributes are required to create a property. These are the `name` field (how the name appears in the pipeline properties), the `type` (text, secure and enum) and then the `value`. Do not put secrets directly into JSON for the `secure` type, instead the value for a `secret` type should be a CRN to a secret in the configured secrets provider or a secret reference to a secret in the configured secrets provider. | `string` | `""` |
| `cd_pipeline_properties` | This JSON represents the pipeline properties belonging to the CD pipeline in the CD toolchain. Each element in the JSON represents a seperate pipeline property. Three attributes are required to create a property. These are the `name` field (how the name appears in the pipeline properties), the `type` (text, secure and enum) and then the `value`. Do not put secrets directly into JSON for the `secure` type, instead the value for a `secret` type should be a CRN to a secret in the configured secrets provider or a secret reference to a secret in the configured secrets provider. | `string` | `""` |
| `cc_pipeline_properties` | This JSON represents the pipeline properties belonging to the CC pipeline in the CC toolchain. Each element in the JSON represents a seperate pipeline property. Three attributes are required to create a property. These are the `name` field (how the name appears in the pipeline properties), the `type` (text, secure and enum) and then the `value`. Do not put secrets directly into JSON for the `secure` type, instead the value for a `secret` type should be a CRN to a secret in the configured secrets provider or a secret reference to a secret in the configured secrets provider. | `string` | `""` |
| `cluster_name` | (required in Kubernetes variant only) Name of the Kubernetes cluster where the application is deployed. This sets the same cluster name for both CI and CD toolchains. See `ci_cluster_name` and `cd_cluster_name` to set different cluster names. By default , the cluster namespace for CI will be set to `dev` and CD to `prod`. These can be changed using `ci_cluster_namespace` and `cd_cluster_namespace`. | `string` | `"mycluster-free"` |
| `ci_code_engine_project` | (required in Kubernetes variant only) The name of the Code Engine project to use. The project is created if it does not already exist. | `string` | `"DevSecOps_CE"` |
| `cd_code_engine_project` | (required in Kubernetes variant only) The name of the Code Engine project to use for the CD pipeline promoted code. The project is created if it does not already exist. | `string` | `"Sample_CD_Project"` |
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
| `add_code_engine_prefix` | Set to `true` to use `prefix` to add a prefix to the code engine project names. | `bool` | `true` |
| `add_container_name_suffix` | Set to `true` to add a random suffix to the specified ICR name. | `bool` | `false` |
| `add_pipeline_definitions` | Set to `true` to add pipeline definitions. | `string` | `"true"` |
| `app_group` | Specify the Git user or group for the application repository. | `string` | `""` |
| `app_repo_auth_type` | Select the method of authentication that is used to access the Git repository. Valid values are 'oauth' or 'pat'. Defaults to `oauth` when unset. `pat` is a git `personal access token`. | `string` | `""` |
| `app_repo_branch` | This is the repository branch used by the default sample application. Alternatively if `app_repo_existing_url` is provided, then the branch must reflect the default branch for that repository. Typically these branches are `main` or `master`. | `string` | `"main"` |
| `app_repo_clone_from_url` | Override the default sample app by providing your own sample app URL, which is cloned into the app repository. Note, uses `clone_if_not_exists` mode, so if the app repository already exists the repository contents are unchanged. | `string` | `""` |
| `app_repo_clone_to_git_id` | Set this value to `github` for github.com, or to the GUID of a custom GitHub Enterprise server. | `string` | `""` |
| `app_repo_clone_to_git_provider` | By default this gets set as 'hostedgit', else set to 'githubconsolidated' for GitHub repositories. | `string` | `""` |
| `app_repo_existing_git_id` | Set this value to `github` for github.com, or to the GUID of a custom GitHub Enterprise server. | `string` | `""` |
| `app_repo_existing_git_provider` | By default this gets set as 'hostedgit', else set to 'githubconsolidated' for GitHub repositories. | `string` | `""` |
| `app_repo_existing_url` | Bring your own existing application repository by providing the URL. This will create an integration for your application repository instead of cloning the default sample. Repositories existing in a different org will require the use of Git token. See `app_repo_git_token_secret_name` under optional variables. | `string` | `"__NOTSET__"` |
| `app_repo_git_token_secret_crn` | The CRN of the Git token used for accessing the sample application repository. | `string` | `""` |
| `app_repo_git_token_secret_name` | Name of the Git token secret in the secret provider used for accessing the sample (or bring your own) application repository. | `string` | `""` |
| `app_repo_secret_group` | Secret group for the App repository secret. Defaults to the value set in `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `authorization_policy_creation` | Disable Toolchain Service to Secrets Manager/Key Protect/Notifications Service authorization policy creation. To disable set the value to `disabled`. This applies to the CI, CD, and CC toolchains. To set independently, see `ci_authorization_policy_creation`, `cd_authorization_policy_creation`, and `cc_authorization_policy_creation`. | `string` | `""` |
| `autostart` | Set to `true` to auto run the CI pipeline in the CI toolchain after creation. | `bool` | `false` |
| `cluster_name` | Name of the Kubernetes cluster where the application is deployed. This sets the same cluster name for both CI and CD toolchains. See `ci_cluster_name` and `cd_cluster_name` to set different cluster names. By default , the cluster namespace for CI will be set to `dev` and CD to `prod`. These can be changed using `ci_cluster_namespace` and `cd_cluster_namespace`. | `string` | `"mycluster-free"` |
| `code_engine_project` | The name of the Code Engine project to use. Created if it does not exist. Applies to both the CI and CD toolchains. To set individually use `ci_code_engine_project` and `cd_code_engine_project`. | `string` | `""` |
| `compliance_pipeline_branch` | The Compliance Pipeline definitions branch. See `ci_compliance_pipeline_branch`, `cd_compliance_pipeline_branch` and `cc_compliance_pipeline_branch` to set independently. | `string` | `"open-v10"` |
| `compliance_pipeline_existing_repo_url` | The URL of an existing compliance pipelines repository. | `string` | `""` |
| `compliance_pipeline_group` | Specify user or group for compliance pipline repository. | `string` | `""` |
| `compliance_pipeline_repo_auth_type` | Select the method of authentication that is used to access the Git repository. Valid values are 'oauth' or 'pat'. Defaults to `oauth` when unset. `pat` is a git `personal access token`. | `string` | `""` |
| `compliance_pipeline_repo_blind_connection` | Setting this value to `true` means the server is not addressable on the public internet. IBM Cloud will not be able to validate the connection details you provide. Certain functionality that requires API access to the git server will be disabled. Delivery pipeline will only work using a private worker that has network access to the git server. | `bool` | `false` |
| `compliance_pipeline_repo_git_id` | Set this value to `github` for github.com, or to the ID of a custom GitHub Enterprise server. | `string` | `""` |
| `compliance_pipeline_repo_git_provider` | Git provider for pipeline repo | `string` | `""` |
| `compliance_pipeline_repo_git_token_secret_crn` | The CRN of the Git token used for accessing the sample application repository. | `string` | `""` |
| `compliance_pipeline_repo_git_token_secret_name` | Name of the Git token secret in the secret provider used for accessing the compliance pipelines repository. | `string` | `""` |
| `compliance_pipeline_repo_name` | Sets the name for the compliance pipelines repository if cloned. The expected behaviour is to link to an existing compliance-pipelines repository. | `string` | `""` |
| `compliance_pipeline_repo_root_url` | (Optional) The Root URL of the server. e.g. https://git.example.com. | `string` | `""` |
| `compliance_pipeline_repo_secret_group` | Secret group for the Compliance Pipeline repository secret. Defaults to the value set in `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `compliance_pipeline_repo_title` | (Optional) The title of the server. e.g. My Git Enterprise Server. | `string` | `""` |
| `compliance_pipeline_repo_use_group_settings` | Set to `true` to apply group level repository settings to the compliance pipeline repository. See `repo_git_provider` as an example. | `bool` | `true` |
| `continuous_delivery_service_name` | The name of the CD instance. | `string` | `"cd-devsecops"` |
| `cos_api_key_secret_crn` | The CRN of the Cloud Object Storage apikey. Applies to the CI, CD and CC toolchains. Can beset independently using `ci_cos_api_key_secret_crn`,`cd_cos_api_key_secret_crn`,`cc_cos_api_key_secret_crn`. | `string` | `""` |
| `cos_api_key_secret_group` | Secret group for the COS api key secret. Defaults to the value set in `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `cos_api_key_secret_name` | Name of the Cloud Object Storage API key secret in the secret provider for accessing the evidence COS bucket. In addition `cos_endpoint` and `cos_bucket_name` must be set. This setting sets the same API key for the COS settings in the CI, CD, and CC toolchains. | `string` | `""` |
| `cos_api_key_secret_value` | A user provided api key with COS access permissions that can be pushed to Secrets Manager. See `cos_api_key_secret_name` and `create_cos_api_key`. | `string` | `""` |
| `cos_bucket_name` | Set the name of your COS bucket. This applies the same COS bucket name for the CI, CD, and CC toolchains. See `ci_cos_bucket_name`, `cd_cos_bucket_name`, and `cc_cos_bucket_name` to set separately. | `string` | `""` |
| `cos_endpoint` | The endpoint for the Cloud Object Stroage instance containing the evidence bucket. This setting sets the same endpoint for COS in the CI, CD, and CC toolchains. See `ci_cos_endpoint`, `cd_cos_endpoint`, and `cc_cos_endpoint` to set the endpoints independently. | `string` | `""` |
| cos_instance_crn | The CRN of the Cloud Object Storage instance containing the required bucket. This value is required to generate the correct access policies if creating IAM service credentials. | `string` | `""` |
| `create_access_group` | Set to `true` to create an access group for the operations of the DevSecOps toolchains. | `bool` | `false` |
| `create_cc_toolchain` | Boolean flag which determines if the DevSecOps CC toolchain is created. | `bool` | `true` |
| `create_cd_instance` | Set to `true` to create Continuous Delivery Service. | `bool` | `false` |
| `create_cd_toolchain` | Boolean flag which determines if the DevSecOps CD toolchain is created. | `bool` | `true` |
| `create_ci_toolchain` | Flag which determines if the DevSecOps CI toolchain is created. If this toolchain is not created then values must be set for the following variables, evidence_repo_url, issues_repo_url and inventory_repo_url. | `bool` | `true` |
| `create_code_engine_access_policy` | Add a Code Engine access policy to the generated IAM access key. See `create_ibmcloud_api_key`. | `bool` | `true` |
| `create_cos_api_key` | Set to `true` to create and add a `cos-api-key` to the Secrets Provider. | `bool` | `false` |
| `create_git_token` | Set to `true` to create and add the specified personal access token secret to the Secrets Provider. Use `repo_git_token_secret_value` for setting the value. | `bool` | `false` |
| `create_git_triggers` | Set to `true` to create the default Git triggers associated with the compliance repos and sample app. | `string` | `"true"` |
| `create_ibmcloud_api_key` | Set to `true` to create and add an `ibmcloud-api-key` to the Secrets Provider. | `bool` | `false` |
| `create_icr_namespace` | Set to `true` to have Terraform create the registry namespace. Setting to `false` will have the CI pipeline create the namespace if it does not already exist. Note: If a Terraform destroy is used, the ICR namespace along with all images will be removed. | `bool` | `false` |
| `create_kubernetes_access_policy` | Add a Kubernetes access policy to the generated IAM access key. See `create_ibmcloud_api_key`. | `bool` | `false` |
| `create_privateworker_secret` | Set to `true` to add a specified private worker service api key to the Secrets Provider. This also enables a private worker tool integration in the toolchains. | `bool` | `false` |
| `create_secret_group` | Set to `true` to create the specified Secrets Manager secret group. | `bool` | `false` |
| `create_signing_key` | Set to `true` to create and add a `signing-key` and the `signing-certificate` to the Secrets Provider. | `bool` | `false` |
| `create_triggers` | Set to `true` to create the default triggers associated with the compliance repos and sample app. | `string` | `"true"` |
| `enable_pipeline_notifications` | When enabled, pipeline run events will be sent to the Event Notifications and Slack integrations in the enclosing toolchain. | `string` | `""` |
| `enable_privateworker` | Set to `true` to enable private workers for the CI, CD, CC and PR pipelines. A valid service api key must be set in Secrets Manager. The name of this secret can be specified using `privateworker_credentials_secret_name`. | `string` | `"false"` |
| `enable_secrets_manager` | Set to `true` to enable the Secrets Manager integrations. | `string` | `"true"` |
| `enable_slack` | Set to `true` to create the Slack toolchain integration. This requires a valid `slack_channel_name`, `slack_team_name`, and a valid `webhook` (see `slack_webhook_secret_name`). This setting applies for CI, CD, and CC toolchains. | `string` | `"false"` |
| `environment_prefix` | By default `ibm:yp:`. This will be set as the prefix to regions automatically where required. For example `ibm:yp:us-south`. | `string` | `"ibm:yp:"` |
| `environment_tag` | Tag name that represents the target environment in the inventory. Example: prod_latest. | `string` | `"prod_latest"` |
| `event_notifications_crn` | Set the Event Notifications CRN to create an Events Notification integration. This paramater will apply to the CI, CD and CC toolchains. Can be set independently with `ci_event_notifications_crn`, `cd_event_notifications_crn`, `cc_event_notifications_crn`. | `string` | `""` |
| `event_notifications_tool_name` | The name of the Event Notifications integration. | `string` | `"Event Notifications"` |
| `evidence_group` | Specify the Git user or group for the evidence repository. | `string` | `""` |
| `evidence_repo_auth_type` | Select the method of authentication that is used to access the Git repository. Valid values are 'oauth' or 'pat'. Defaults to `oauth` when unset. `pat` is a git `personal access token`. | `string` | `""` |
| `evidence_repo_existing_git_id` | Set this value to `github` for github.com, or to the GUID of a custom GitHub Enterprise server. | `string` | `""` |
| `evidence_repo_existing_git_provider` | Git provider for evidence repo. If not set will default to `hostedgit`. | `string` | `""` |
| `evidence_repo_existing_url` | Set to use an existing evidence repository. | `string` | `""` |
| `evidence_repo_git_token_secret_crn` | The CRN of the Git token used for accessing the Evidence repository. | `string` | `""` |
| `evidence_repo_git_token_secret_name` | Name of the Git token secret in the secret provider used for accessing the evidence repository. | `string` | `""` |
| `evidence_repo_integration_owner` | The name of the repository integration owner. | `string` | `""` |
| `evidence_repo_name` | Set to use a custom name for the Evidence repository. | `string` | `""` |
| `evidence_repo_secret_group` | Secret group for the Evidence repository secret. Defaults to the value set in `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `force_create_standard_api_key` | Set to `true` to force create a standard api key. By default the generated apikey will be a service api key. It is recommended to use a Git Token when using the service api key. In the case where the user has been invited to an account and that user not the account owner, during toolchain creation the default compliance repositories will be created in that user's account and the service api will not have access to those repositories. In this case a Git Token for the repositories is required. See `repo_git_token_secret_name` for more details. The alternative is to set `force_create_standard_api_key` to `true` to create a standard api key. | `bool` | `false` |
| `ibmcloud_api` | The environment URL. When left unset this will default to `https://cloud.ibm.com` | `string` | `""` |
| `ibmcloud_api_key` | The API key used to create the toolchains. (See deployment guide). | `string` | n/a |
| `inventory_group` | Specify the Git user or group for the inventory repository. | `string` | `""` |
| `inventory_repo_auth_type` | Select the method of authentication that is used to access the Git repository. Valid values are 'oauth' or 'pat'. Defaults to `oauth` when unset. `pat` is a git `personal access token`. | `string` | `""` |
| `inventory_repo_existing_git_id` | Set this value to `github` for github.com, or to the GUID of a custom GitHub Enterprise server. | `string` | `""` |
| `inventory_repo_existing_git_provider` | Git provider for the inventory repo. If not set will default to `hostedgit`. | `string` | `""` |
| `inventory_repo_existing_url` | Set to use an existing inventory repository. | `string` | `""` |
| `inventory_repo_git_token_secret_crn` | The CRN of the Git token used for acessing the Inventory repository. | `string` | `""` |
| `inventory_repo_git_token_secret_name` | Name of the Git token secret in the secret provider used for accessing the inventory repository. | `string` | `""` |
| `inventory_repo_integration_owner` | The name of the repository integration owner. | `string` | `""` |
| `inventory_repo_name` | Set to use a custom name for the Inventory repository. | `string` | `""` |
| `inventory_repo_secret_group` | Secret group for the Inventory repository secret. Defaults to the value set in `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `issues_group` | Specify the Git user or group for the issues repository. | `string` | `""` |
| `issues_repo_auth_type` | Select the method of authentication that is used to access the Git repository. Valid values are 'oauth' or 'pat'. Defaults to `oauth` when unset. `pat` is a git `personal access token`. | `string` | `""` |
| `issues_repo_existing_git_id` | Set this value to `github` for github.com, or to the GUID of a custom GitHub Enterprise server. | `string` | `""` |
| `issues_repo_existing_git_provider` | Git provider for the issues repo. If not set will default to `hostedgit`. | `string` | `""` |
| `issues_repo_existing_url` | By default this gets set as 'hostedgit', else set to 'githubconsolidated' for GitHub repositories. | `string` | `""` |
| `issues_repo_git_token_secret_crn` | The CRN of the Git token used for accessing the Issues repository. | `string` | `""` |
| `issues_repo_git_token_secret_name` | Name of the Git token secret in the secret provider used for accessing the issues repository. | `string` | `""` |
| `issues_repo_integration_owner` | The name of the repository integration owner. | `string` | `""` |
| `issues_repo_name` | Set to use a custom name for the Issues repository. | `string` | `""` |
| `issues_repo_secret_group` | Secret group for the Issues repository secret. Defaults to the value set in `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `pipeline_config_group` | Specify the Git user or group for the compliance pipeline repository. | `string` | `""` |
| `pipeline_config_repo_auth_type` | Select the method of authentication that is used to access the Git repository. Valid values are 'oauth' or 'pat'. Defaults to `oauth` when unset. `pat` is a git `personal access token`. | `string` | `""` |
| `pipeline_config_repo_branch` | Specify the branch containing the custom pipeline-config.yaml file. | `string` | `""` |
| `pipeline_config_repo_clone_from_url` | Specify a repository containing a custom pipeline-config.yaml file. | `string` | `""` |
| `pipeline_config_repo_existing_url` | Specify and link to an existing repository containing a custom pipeline-config.yaml file. | `string` | `""` |
| `pipeline_config_repo_git_id` | Set this value to `github` for github.com, or to the GUID of a custom GitHub Enterprise server. | `string` | `""` |
| `pipeline_config_repo_git_provider` | Git provider for pipeline repo config | `string` | `""` |
| `pipeline_config_repo_git_token_secret_crn` | The CRN of the Git token for accessing the pipeline config repository. | `string` | `""` |
| `pipeline_config_repo_git_token_secret_name` | Name of the Git token secret in the secret provider used for accessing the pipeline config repository. | `string` | `""` |
| `pipeline_config_repo_secret_group` | Secret group for the Pipeline Config repository secret. Defaults to the value set in `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `pipeline_doi_api_key_secret_crn` | The CRN of the DOI (DevOps Insights) apikey used for accessing a specific toolchain Insights instance. Applies to the CI, CD and CC toolchains. | `string` | `""` |
| `pipeline_doi_api_key_secret_group` | Secret group for the pipeline DOI api key. Defaults to the value set in `sm_secret_group` if not set. Only used with `Secrets Manager`. Applies to the CI, CD and CC toolchains. | `string` | `""` |
| `pipeline_doi_api_key_secret_name` | Name of the Cloud API key secret in the secret provider to access the toolchain containing the Devops Insights instance. This will apply to the CI, CD and CC toolchains. | `string` | `""` |
| `pipeline_git_tag` | The GIT tag selector for the Compliance Pipelines definitions. | `string` | `""` |
| `pipeline_ibmcloud_api_key_secret_crn` | The CRN of the IBMCloud apikey used for running the pipelines. | `string` | `""` |
| `pipeline_ibmcloud_api_key_secret_group` | Secret group for the pipeline ibmcloud API key secret. Defaults to the value set in `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `pipeline_ibmcloud_api_key_secret_name` | Name of the Cloud API key secret in the secret provider for running the pipelines. Applies to the CI, CD and CC toolchains. | `string` | `"ibmcloud-api-key"` |
| `pipeline_ibmcloud_api_key_secret_value` | A user provided api key for running the toolchain pipelines that can be pushed to Secrets Manager. See `pipeline_ibmcloud_api_key_secret_name` and `create_ibmcloud_api_key`. | `string` | `""` |
| `pipeline_git_tag` | The GIT tag selector for the Compliance Pipelines definitions. | `string` | `""` |
| `prefix` | A prefix that is added to the toolchain resources. | `string` | `""` |
| `privateworker_credentials_secret_crn` | The CRN for the Private Worker secret secret. | `string` | `""` |
| `privateworker_credentials_secret_group` | Secret group prefix for the Private Worker secret. Defaults to using `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `privateworker_credentials_secret_name` | Name of the privateworker secret in the secret provider. | `string` | `""` |
| `privateworker_name` | The name of the private worker tool integration. | `string` | `"private-worker-tool-01"` |
| `privateworker_secret_value` | The private worker service api key that will be added to the `privateworker_credentials_secret_name` secret in the secrets provider. | `string` | `""` |
| `registry_namespace` | A unique namespace within the IBM Cloud Container Registry region where the application image is stored. | `string` | `""` |
| `repo_apply_settings_to_compliance_repos` | Set to `true` to apply the same settings to all the default compliance repositories. Set to `false` to apply these settings to only the sample application, pipeline config and the deployment repositories. | `bool` | `true` |
| `repo_blind_connection` | Setting this value to `true` means the server is not addressable on the public internet. IBM Cloud will not be able to validate the connection details you provide. Certain functionality that requires API access to the git server will be disabled. Delivery pipeline will only work using a private worker that has network access to the git server. | `bool` | `false` |
| `repo_git_id` | The Git ID for the compliance repositories. | `string` | `""` |
| `repo_git_provider` | The Git provider type. | `string` | `""` |
| `repo_git_token_secret_crn` | The CRN for the repositories Git Token. | `string` | `""` |
| `repo_git_token_secret_name` | Name of the Git token secret in the secret provider. Specifying a secret name for the Git Token automatically sets the authentication type to `pat`. | `string` | `""` |
| `repo_git_token_secret_value` | The personal access token that will be added to the `repo_git_token_secret_name` secret in the secrets provider. | `string` | `""` |
| `repo_group` | Specify the Git user or group for your application. This must be set if the repository authentication type is `pat` (personal access token). | `string` | `""` |
| `repo_root_url` | (Optional) The Root URL of the server. e.g. https://git.example.com. | `string` | `""` |
| `repo_secret_group` | Secret group in Secrets Manager that contains the secret for the repository. This variable will set the same secret group for all the repositories. Can be overriden on a per secret group basis. Only applies when using Secrets Manager. | `string` | `""` |
| `repo_title` | (Optional) The title of the server. e.g. My Git Enterprise Server. | `string` | `""` |
| `repositories_prefix` | Prefix name for the cloned compliance repos. For the repositories_prefix value only a-z, A-Z and 0-9 and the special characters `-_` are allowed. In addition the string must not end with a special character or have two consecutive special characters. | `string` | `"compliance"` |
| `rotate_signing_key` | Set to `true` to rotate the signing key and signing certificate. It is important to make a back up for the current code signing certificate as pending CD deployments might require image validation against the previous signing key. | `bool` | `false` |
| `rotation_period` | The number of days until the `ibmcloud-api-key` and the `cos-api-key` are auto rotated. | `number` | `90` |
| `sample_default_application` | The name of the sample application repository. The repository source URL is automatically computed based on the toolchain region. The other currently supported name is `code-engine-compliance-app`. Alternatively an integration can be created that can link to or clone from an existing repository. See `app_repo_existing_url` and `app_repo_clone_from_url` to override the sample application default behavior. | `string` | `"code-engine-compliance-app"` |
| `scc_attachment_id` | An attachment ID. An attachment is configured under a profile to define how a scan will be run. To find the attachment ID, in the browser, in the attachments list, click on the attachment link, and a panel appears with a button to copy the attachment ID. This parameter is only relevant when the `scc_use_profile_attachment` parameter is enabled. | `string` | `""` |
| `scc_enable_scc` | Adds the SCC tool integration to the toolchain. | `string` | `"true"` |
| `scc_instance_crn` | The Security and Compliance Center service instance CRN (Cloud Resource Name). This parameter is only relevant when the `scc_use_profile_attachment` parameter is enabled. | `string` | `""` |
| `scc_profile_name` | The name of a Security and Compliance Center profile. Use the `IBM Cloud Framework for Financial Services` profile, which contains the DevSecOps Toolchain rules. Or use a user-authored customized profile that has been configured to contain those rules. This parameter is only relevant when the `scc_use_profile_attachment` parameter is enabled. | `string` | `""` |
| `scc_profile_version` | The version of a Security and Compliance Center profile, in SemVer format, like `0.0.0`. This parameter is only relevant when the `scc_use_profile_attachment` parameter is enabled. | `string` | `""` |
| `scc_scc_api_key_secret_crn` | The CRN for the SCC apikey. | `string` | `""` |
| `scc_scc_api_key_secret_group` | Secret group for the Security and Compliance tool secret. Defaults to the value set in `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `scc_scc_api_key_secret_name` | The name of the Security and Compliance Center api-key secret in the secret provider. | `string` | `"scc-api-key"` |
| `scc_use_profile_attachment` | Set to `enabled` to enable use profile with attachment, so that the scripts in the pipeline can interact with the Security and Compliance Center service. When enabled, other parameters become relevant; `scc_scc_api_key_secret_name`, `scc_instance_crn`, `scc_profile_name`, `scc_profile_version`, `scc_attachment_id`. Can individually be `enabled` and `disabled` in the CD and CC toolchains using `cd_scc_use_profile_attachment` and `cc_scc_use_profile_attachment`. | `string` | `"disabled"` |
| `slack_channel_name` | The name of the Slack channel where notifications are posted. This applies to the CI, CD, and CC toolchains. To set independently see `ci_slack_channel_name`, `cd_slack_channel_name`, and `cc_slack_channel_name`. | `string` | `""` |
| `slack_integration_name` | The name of the Slack integration. | `string` | `"slack-compliance"` |
| `slack_team_name` | The Slack team name, which is the word or phrase before `.slack.com` in the team URL. This applies to the CI, CD, and CC toolchains. To set independently, see `ci_slack_team_name`, `cd_slack_team_name`, and `cc_slack_team_name`. | `string` | `""` |
| `slack_webhook_secret_crn` | The CRN of the Slack webhook secret used for accessing the specified Slack channel. | `string` | `""` |
| `slack_webhook_secret_group` | Secret group for the Slack webhook secret. Defaults to the value set in `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `slack_webhook_secret_name` | Name of the webhook secret in the secret provider used for accessing the configured Slack channel. This applies to the CI, CD, and CC toolchains. To set independently, see `ci_slack_webhook_secret_name`, `cd_slack_webhook_secret_name`, and `cc_slack_webhook_secret_name`. | `string` | `"slack-webhook"` |
| `sm_endpoint_type` | The types of service endpoints to target for Secrets Manager. Valid values are `private` and `public`. | `string` | `"private"` |
| `sm_instance_crn` | The CRN of the Secrets Manager instance. Will apply to CI, CD and CC toolchains unless set individually. Setting up the Secrets Manager integration using a CRN takes precendence over the non CRN setup. | `string` | `""` |
| `sm_integration_name` | The name of the Secrets Manager integration. | `string` | `"sm-compliance-secrets"` |
| `sm_location` | The region hosting the Secrets Manager instance. This applies to the CI, CD and CC Secret Manager integrations. | `string` | `"us-south"` |
| `sm_name` | The name of an existing Secret Managers instance. This applies to the CI, CD and CC Secret Manager integrations. | `string` | `"sm-instance"` |
| `sm_resource_group` | The name of the existing resource group containing the Secrets Manager instance for your secrets.. This applies to the CI, CD and CC Secret Manager integrations. | `string` | `"Default"` |
| `sm_secret_expiration_period` | The number of days until the secrets expire. Leave empty to not set an expiration for the created secrets. | `string` | `""` |
| `sm_secret_group` | The Secrets Manager secret group containing the secrets for the DevSecOps pipelines. This applies to the CI, CD and CC Secret Manager integrations. | `string` | `"Default"` |
| `sonarqube_integration_name` | The name of the SonarQube integration. | `string` | `"SonarQube"` |
| `sonarqube_is_blind_connection` | When set to `true`, instructs IBM Cloud Continuous Delivery to not validate the configuration of this integration. Set this to `true` if the SonarQube server is not addressable on the public internet. | `string` | `"true"` |
| `sonarqube_secret_crn` | The CRN of the secret used to access SonarQube. | `string` | `""` |
| `sonarqube_secret_group` | Secret group for the SonarQube secret. Defaults to the value set in `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `sonarqube_secret_name` | The name of the SonarQube secret in the secrets provider. | `string` | `"sonarqube-secret"` |
| `sonarqube_server_url` | The URL to the SonarQube server. | `string` | `""` |
| `sonarqube_user` | The name of the SonarQube user. | `string` | `""` |
| `toolchain_access_group_name` | The name of the DevSecOps access group. See `create_access_group`. | `string` | `"devsecops-toolchain"` |
| `toolchain_name` | This variable specifies the root name for the CI, CD and CC toolchain names. A fixed suffix will automatically be appended. Setting `DevSecOps` will generate toolchains with the names `DevSecOps-CI-Toolchain`,  `DevSecOps-CD-Toolchain` and `DevSecOps-CC-Toolchain`. The full name of each toolchain can be set independently using `ci_toolchain_name`, `cd_toolchain_name`, and `cc_toolchain_name`. | `string` | `"DevSecOps"` |
| `toolchain_region` | The region identifier that will be used, by default, for all resource creation and service instance lookup. This can be overridden on a per resource/service basis. | `string` | `"us-south"` |
| `toolchain_resource_group` | The resource group that will be used, by default, for all resource creation and service instance lookups. This can be overridden on a per resource/service basis. | `string` | `"Default"` |
| `use_app_repo_for_cd_deploy` | Set to `true` to use the CI sample application repository as the deployment repository in the CD pipeline. This will be set in the pipeline config integration. | `bool` | `true` |
| `legacy_ref` | Set to `true` to use the legacy secret reference format for Secrets Manager secrets. | `bool` | `true` |
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
| `app_repo_branch` | This is the repository branch used by the default sample application. Alternatively if `app_repo_existing_url` is provided, then the branch must reflect the default branch for that repository. Typically these branches are `main` or `master`. | `string` | `"main"` |
| `app_repo_clone_from_url` | Override the default sample app by providing your own sample app URL, which is cloned into the app repository. Note, uses `clone_if_not_exists` mode, so if the app repository already exists the repository contents are unchanged. | `string` | `""` |
| `app_repo_clone_to_git_id` | Set this value to `github` for github.com, or to the GUID of a custom GitHub Enterprise server. | `string` | `""` |
| `app_repo_clone_to_git_provider` | By default this gets set as 'hostedgit', else set to 'githubconsolidated' for GitHub repositories. | `string` | `""` |
| `app_repo_existing_git_id` | Set this value to `github` for github.com, or to the GUID of a custom GitHub Enterprise server. | `string` | `""` |
| `app_repo_existing_git_provider` | By default this gets set as 'hostedgit', else set to 'githubconsolidated' for GitHub repositories. | `string` | `""` |
| `app_repo_existing_url` | Bring your own existing application repository by providing the URL. This will create an integration for your application repository instead of cloning the default sample. Repositories existing in a different org will require the use of Git token. See `app_repo_git_token_secret_name` under optional variables. | `string` | `"__NOTSET__"` |
{: caption="Clone or use an existing repository" caption-side="top"}

To use an existing application in github.ibm.com, use the following example settings:

| Name | Value |
|------|-------------|
|`app_repo_existing_url` |`"https://github.ibm.com/{my-org}/{my-repo}.git"`|
|`app_repo_branch`|`"main"`|
|`app_repo_existing_git_provider`|`"githubconsolidated"`|
|`app_repo_existing_git_id`|`"integrated"`|
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
| `pipeline_ibmcloud_api_key_secret_name` | Name of the Cloud API key secret in the secret provider for running the pipelines. Applies to the CI, CD and CC toolchains. | `string` | `"ibmcloud-api-key"` |
|`ci_signing_key_secret_name`|Name of the signing key secret in the secret provider.|`"signing_key"`|
{: caption="Secret names" caption-side="top"}

For more secret names, search for "secret_name" in the variable list.


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
| `ci_app_name` | Name of the application image and inventory entry. | `string` | `"hello-compliance-app"` |
| `ci_app_repo_branch` | This is the repository branch used by the default sample application. Alternatively if `app_repo_existing_url` is provided, then the branch must reflect the default branch for that repository. Typically these branches are `main` or `master`. | `string` | `""` |
| `ci_cluster_name` | Name of the cluster where the application is deployed. (can be the same cluster used for prod | `string` | `""` |
| `ci_cluster_namespace` | Name of the cluster namespace where the application is deployed. | `string` | `"dev"` |
| `ci_cluster_region` | Region hosting the cluster where the application is deployed. Use the short form of the regions. For example `us-south`. | `string` | `""` |
| `ci_cluster_resource_group` | The cluster resource group. | `string` | `""` |
| `ci_code_engine_project` | The name of the Code Engine project to use. | `string` | `"DevSecOps_CE"` |
| `ci_code_engine_region` | The region to create/lookup for the Code Engine project. | `string` | `""` |
| `ci_code_engine_resource_group` | The resource group of the Code Engine project. | `string` | `""` |
| `ci_compliance_pipeline_pr_branch` | The PR Pipeline Compliance Pipeline branch. | `string` | `""` |
| `ci_doi_toolchain_id` | The ID of the toolchain containing the DevOps Insights integration. This variable is used to link the DevOps Insights toolcard to a specific instance. | `string` | `""` |
| `ci_doi_toolchain_id_pipeline_property` | The pipeline property for the DevOps Insights instance toolchain ID. | `string` | `""` |
| `ci_link_to_doi_toolchain` | Enable a link to a DevOps Insights instance in another toolchain. | `bool` | `false` |
| `ci_pipeline_config_repo_branch` | Specify the branch containing the custom pipeline-config.yaml file. | `string` | `""` |
| `ci_pipeline_properties` | This JSON represents the pipeline properties belonging to the both the CI and PR pipelines in the CI toolchain. Each element in the JSON represents a seperate pipeline property. Three attributes are required to create a property. These are the `name` field (how the name appears in the pipeline properties), the `type` (text, secure and enum) and then the `value`. Do not put secrets directly into JSON for the `secure` type, instead the value for a `secret` type should be a CRN to a secret in the configured secrets provider or a secret reference to a secret in the configured secrets provider. | `string` | `""` |
| `ci_pipeline_properties_filepath` | The path to the file containing the property JSON. If this is not set and `ci_pipeline_properties` is not set, it will by default read the `properties.json` file at the root of the CI module. | `string` | `""` |
| `ci_privateworker_credentials_secret_crn` | The CRN of the private worker service apikey that runs the pipeline tasks. | `string` | `""` |
| `ci_registry_region` | The IBM Cloud Region where the IBM Cloud Container Registry namespace is to be created. Use the short form of the regions. For example `us-south`. | `string` | `""` |
| `ci_repository_properties` | Stringified JSON containing the repositories and triggers that get created in the CI toolchain pipelines. | `string` | `""` |
| `ci_signing_key_secret_name` | Name of the signing key secret in the secret provider used for signing images/artifacts. | `string` | `"signing-key"` |
| `ci_toolchain_description` | Description for the CI Toolchain. | `string` | `"Toolchain created with terraform template for DevSecOps CI Best Practices."` |
| `ci_toolchain_name` | The name of the CI Toolchain. | `string` | `""` |
| `ci_trigger_git_enable` | Set to `true` to enable the CI pipeline Git trigger. | `bool` | `true` |
| `ci_trigger_manual_enable` | Set to `true` to enable the CI pipeline Manual trigger. | `bool` | `true` |
| `ci_trigger_manual_pruner_enable` | Set to `true` to enable the manual Pruner trigger. | `bool` | `true` |
| `ci_trigger_pr_git_enable` | Set to `true` to enable the PR pipeline Git trigger. | `bool` | `true` |
| `ci_trigger_timed_cron_schedule` | Only needed for timer triggers. Cron expression that indicates when this trigger will activate. Maximum frequency is every 5 minutes. The string is based on UNIX crontab syntax: minute, hour, day of month, month, day of week. Example: 0 *_/2 * * * - every 2 hours. | `string` | `"0 4 * * *"` |
| `ci_trigger_timed_enable` | Set to `true` to enable the CI pipeline Timed trigger. | `bool` | `false` |
| `ci_trigger_timed_pruner_enable` | Set to `true` to enable the timed Pruner trigger. | `bool` | `false` |
{: caption="Continuous integration" caption-side="bottom"}
{: #ci-parameters}
{: tab-title="Continuous integration"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `cd_change_management_group` | Specify group for change management repository | `string` | `""` |
| `cd_change_management_repo_auth_type` | Select the method of authentication that is used to access the Git repository. Valid values are 'oauth' or 'pat'. Defaults to `oauth` when unset. `pat` is a git `personal access token`. | `string` | `""` |
| `cd_change_management_repo_git_token_secret_crn` | The CRN for the Change Management repository Git Token. | `string` | `""` |
| `cd_change_management_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `cd_change_management_repo_secret_group` | Secret group for the Change Management repository secret. Defaults to the value set in `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `cd_change_repo_clone_from_url` | Override the default management repository, which is cloned into the application repository. Note, using clone_if_not_exists mode, so if the application repository already exists the repository contents are unchanged. | `string` | `""` |
| `cd_cluster_name` | Name of the cluster where the application is deployed. | `string` | `""` |
| `cd_cluster_namespace` | Name of the cluster namespace where the application is deployed. | `string` | `"prod"` |
| `cd_cluster_region` | Region hosting the cluster where the application is deployed. Use the short form of the regions. For example `us-south`. | `string` | `""` |
| `cd_code_engine_project` | The name of the Code Engine project to use for the CD pipeline promoted code. The project is created if it does not already exist. | `string` | `"Sample_CD_Project"` |
| `cd_code_engine_region` | The region to create/lookup for the Code Engine project. | `string` | `""` |
| `cd_code_engine_resource_group` | The resource group of the Code Engine project. | `string` | `""` |
| `cd_code_signing_cert_secret_name` | This is the name of the secret in the secrets provider for storing the code signing certificate. | `string` | `"signing-certificate"` |
| `cd_deployment_group` | Specify group for deployment. | `string` | `""` |
| `cd_deployment_repo_auth_type` | Select the method of authentication that is used to access the Git repository. Valid values are 'oauth' or 'pat'. Defaults to `oauth` when unset. `pat` is a git `personal access token`. | `string` | `""` |
| `cd_deployment_repo_clone_from_branch` | Used when deployment_repo_clone_from_url is provided, the default branch that is used by the CD build, usually either main or master. | `string` | `""` |
| `cd_deployment_repo_clone_from_url` | Override the default sample app by providing your own sample deployment URL, which is cloned into the app repository. Note, using clone_if_not_exists mode, so if the app repository already exists the repository contents are unchanged. | `string` | `""` |
| `cd_deployment_repo_clone_to_git_id` | By default absent, else custom server GUID, or other options for 'git_id' field in the browser UI. | `string` | `""` |
| `cd_deployment_repo_clone_to_git_provider` | By default 'hostedgit', else use 'githubconsolidated' or 'gitlab'. | `string` | `""` |
| `cd_deployment_repo_existing_branch` | Used when deployment_repo_existing_url is provided, the default branch that is by the CD build, usually either main or master. | `string` | `""` |
| `cd_deployment_repo_existing_git_id` | Set this value to `github` for github.com, or to the GUID of a custom GitHub Enterprise server. | `string` | `""` |
| `cd_deployment_repo_existing_git_provider` | Git provider for the deployment repo. If not set will default to `hostedgit`. | `string` | `""` |
| `cd_deployment_repo_existing_url` | Override to bring your own existing deployment repository URL, which is used directly instead of cloning the default deployment sample. | `string` | `""` |
| `cd_deployment_repo_git_token_secret_crn` | The CRN for the Deployment repository Git Token. | `string` | `""` |
| `cd_deployment_repo_git_token_secret_name` | Name of the Git token secret in the secret provider. | `string` | `""` |
| `cd_deployment_repo_secret_group` | Secret group for the Deployment repository secret. Defaults to the value set in `sm_secret_group` if not set. Only used with `Secrets Manager`. | `string` | `""` |
| `cd_doi_toolchain_id` | The ID of the toolchain containing the DevOps Insights integration. This variable is used to link the DevOps Insights toolcard to a specific instance. | `string` | `""` |
| `cd_link_to_doi_toolchain` | Enable a link to a DevOps Insights instance in another toolchain, true or false. | `bool` | `true` |
| `cd_pipeline_config_repo_branch` | Specify the branch containing the custom pipeline-config.yaml file. | `string` | `"main"` |
| `cd_pipeline_properties` | This JSON represents the pipeline properties belonging to the CD pipeline in the CD toolchain. Each element in the JSON represents a seperate pipeline property. Three attributes are required to create a property. These are the `name` field (how the name appears in the pipeline properties), the `type` (text, secure and enum) and then the `value`. Do not put secrets directly into JSON for the `secure` type, instead the value for a `secret` type should be a CRN to a secret in the configured secrets provider or a secret reference to a secret in the configured secrets provider. | `string` | `""` |
| `cd_pipeline_properties_filepath` | The path to the file containing the property JSON. If this is not set and `cd_pipeline_properties` is not set, it will by default read the `properties.json` file at the root of the CD module. | `string` | `""` |
| `cd_privateworker_credentials_secret_crn` | The CRN of the private worker service apikey that runs the pipeline tasks. | `string` | `""` |
| `cd_region` | IBM Cloud region used to prefix the `prod_latest` inventory repository branch. | `string` | `""` |
| `cd_repository_properties` | Stringified JSON containing the repositories and triggers that get created in the CI toolchain pipelines. | `string` | `""` |
| `cd_service_plan` | The Continuous Delivery service plan. Can be `lite` or `professional`. | `string` | `"professional"` |
| `cd_toolchain_description` | Description for the CD toolchain. | `string` | `"Toolchain created with terraform template for DevSecOps CD Best Practices."` |
| `cd_toolchain_name` | The name of the CD Toolchain. | `string` | `""` |
| `cd_trigger_git_enable` | Set to `true` to enable the CD pipeline Git trigger. | `bool` | `false` |
| `cd_trigger_git_promotion_validation_branch` | Branch for Git promotion validation listener. | `string` | `"prod"` |
| `cd_trigger_git_promotion_validation_enable` | Enable Git promotion validation for Git promotion listener. | `bool` | `false` |
| `cd_trigger_git_promotion_validation_listener` | Select a Tekton EventListener to use when Git promotion validation listener trigger is fired. | `string` | `"promotion-validation-listener-gitlab"` |
| `cd_trigger_manual_enable` | Set to `true` to enable the CD pipeline Manual trigger. | `bool` | `true` |
| `cd_trigger_manual_promotion_enable` | Set to `true` to enable the CD pipeline Manual Promotion trigger. | `bool` | `true` |
| `cd_trigger_manual_pruner_enable` | Set to `true` to enable the manual Pruner trigger. | `bool` | `true` |
| `cd_trigger_timed_cron_schedule` | Only needed for timer triggers. Cron expression that indicates when this trigger will activate. Maximum frequency is every 5 minutes. The string is based on UNIX crontab syntax: minute, hour, day of month, month, day of week. Example: 0 *_/2 * * * - every 2 hours. | `string` | `"0 4 * * *"` |
| `cd_trigger_timed_enable` | Set to `true` to enable the CD pipeline Timed trigger. | `bool` | `false` |
| `cd_trigger_timed_pruner_enable` | Set to `true` to enable the timed Pruner trigger. | `bool` | `false` |
| `change_management_existing_url` | The URL for an existing Change Management repository. | `string` | `""` |
| `change_management_repo_git_id` | Set this value to `github` for github.com, or to the ID of a custom GitHub Enterprise server. | `string` | `""` |
{: caption="Continuous deployment" caption-side="bottom"}
{: #cd-parameters}
{: tab-title="Continuous deployment"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `cc_app_repo_branch` | The default branch of the app repository. | `string` | `""` |
| `cc_doi_toolchain_id` | The ID of the toolchain containing the DevOps Insights integration. This variable is used to link the DevOps Insights toolcard to a specific instance. | `string` | `""` |
| `cc_link_to_doi_toolchain` | Enable a link to a DevOps Insights instance in another toolchain, true or false. | `bool` | `true` |
| `cc_pipeline_config_repo_branch` | Specify the branch containing the custom pipeline-config.yaml file. | `string` | `""` |
| `cc_pipeline_properties` | This JSON represents the pipeline properties belonging to the CC pipeline in the CC toolchain. Each element in the JSON represents a seperate pipeline property. Three attributes are required to create a property. These are the `name` field (how the name appears in the pipeline properties), the `type` (text, secure and enum) and then the `value`. Do not put secrets directly into JSON for the `secure` type, instead the value for a `secret` type should be a CRN to a secret in the configured secrets provider or a secret reference to a secret in the configured secrets provider. | `string` | `""` |
| `cc_pipeline_properties_filepath` | The path to the file containing the property JSON. If this is not set and `cc_pipeline_properties` is not set, it will by default read the `properties.json` file at the root of the CC module. | `string` | `""` |
| `cc_repository_properties` | Stringified JSON containing the repositories and triggers that get created in the CI toolchain pipelines. | `string` | `""` |
| `cc_toolchain_description` | Description for the CC Toolchain. | `string` | `"Toolchain created with terraform template for DevSecOps CC Best Practices."` |
| `cc_toolchain_name` | The name of the CC Toolchain. | `string` | `""` |
| `cc_trigger_manual_enable` | Set to `true` to enable the CC pipeline Manual trigger. | `bool` | `true` |
| `cc_trigger_manual_pruner_enable` | Set to `true` to enable the manual Pruner trigger. | `bool` | `true` |
| `cc_trigger_timed_cron_schedule` | Only needed for timer triggers. Cron expression that indicates when this trigger will activate. Maximum frequency is every 5 minutes. The string is based on UNIX crontab syntax: minute, hour, day of month, month, day of week. Example: 0 *_/2 * * * - every 2 hours. | `string` | `"0 4 * * *"` |
| `cc_trigger_timed_enable` | Set to `true` to enable the CI pipeline Timed trigger. | `bool` | `false` |
| `cc_trigger_timed_pruner_enable` | Set to `true` to enable the timed Pruner trigger. | `bool` | `false` |
{: caption="Continuous compliance" caption-side="bottom"}
{: #cc-parameters}
{: tab-title="Continuous compliance"}
{: tab-group="IAM-simple"}
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
{: caption="Outputs" caption-side="top"}
