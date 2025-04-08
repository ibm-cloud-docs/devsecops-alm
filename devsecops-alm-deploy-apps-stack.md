---

copyright:
  years: 2025
lastupdated: "2025-03-24"

keywords: devsecops-alm, deployment guide, deployable architecture, apps stack, devsecops solution

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Deploying the DevSecOps Solution with Apps Stack
{: #devsecops-alm-deploy-stack-req}

Deploying a DevSecOps solution for an Apps stack involves embedding automated security controls, continuous monitoring, and compliance checks into the Continuous Integration (CI) or Continuous Deployment (CD) pipeline. The stack is presented in a manner such that only a few variables are required to deploy everything.
{: shortdesc}

## Before you deploy
{: #before-deploy-prereq}

Before you begin the deployment, can ensure that the prerequisites for the deploying the DevSecOps for Apps stack are in place.

- [Preparing to deploy the DevSecOps Solution for Apps stack](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-stack-req).

## Adding to a project
{: #devsecops-alm-deploy-stack-project}
{: step}

1. Go to the [DevSecOps solution for Apps](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-alm-stack){: external} catalog entry in the {{site.data.keyword.cloud_notm}} catalog.
2. Select the latest version in the **Architecture** section.
3. Select a variation, if more than one is available.
4. Click the **Add to projects** button.
5. In the **Select a project for DevSecOps Solution For Apps** section, set a project name and review the configuration name, region and resource group before clicking the create button. Alternatively, select from an existing project.

## Configuring the deployable architecture
{: #devsecops-alm-deploy-stack-configure}
{: step}

You are now ready to configure the security, required variables, and optional variables for a default deployment with steps for both the service API key and standard API key configurations.

1. In the **Configure** section, select your authentication method. You can use an existing secret in {{site.data.keyword.secrets-manager_short}}, add your API key directly, or set a trusted profile. For more information, see [Using an API key or secret to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-authorize-project).
2. In the **Required** tab, enter values for required fields. Often, you can use the default option. For more information about required variables, see [required input variables](/docs/devsecops-alm?topic=devsecops-alm-stack-prereqs#devsecops-alm-min).
3. In the **Optional** tab, find the following variables and set the value as stated.
   - `create_git_token` - toggle to `true`.
   - `repo_git_token_secret_name` - set a name for your token. This is the name that appears for the Git token secret in Secrets Manager.
   - `repo_git_token_secret_value` - set the Git personal access token for access to the team's repositories.
   - `repo_group` - set the name of the repositories group or owner name.
   - `force_create_standard_api_key` - ensure is set to `false`.
   - to use a standard API key over the service API key. Set `force_create_standard_api_key` as `true`. In this case it is not necessary to set the variables in the previous steps relating to the Git personal access token.
   - `prefix` - set a value. It acts as a prefix in the names of the provisioned resources.
   - `resource_group_name` - set a name for the resource group that the stack creates. It prefixes by the value from the previous step when created.

## (Optional Step) Deploying to an existing resource group
{: #devsecops-alm-deploy-stack-optional-config}
{: step}

As the administrator, you can optionally deploy the resources to an existing resource group.

1. In the **Optional** tab, find the following variables and set the value as stated.
   - `resource_group_name` - set the name of the existing resource group in which the stack provisions the resources.
   - `use_existing_resource_group` - set to `true`.
2. Click **Save**. Do not validate at this point.
3. Navigate to the projects level and find the current stack. Expand the stack and locate the `{{site.data.keyword.compliance_short}}` member. Click the **Options or Kebab** menu.
4. Select **Edit** > **Optional** tab, and find the variable `resource_groups_scope` and click the edit. Set the name of the existing resource group in quotes inside the square brackets. For example ["my-resource-group"].
5. Click **Save**.

## Running the stack
{: #devsecops-alm-deploy-stack-running}
{: step}

1. Navigate to the top level of the project.
2. Click project level **Manage** > **Settings**.
3. Set `Auto-deploy` to on. It deploys all the configurations of the stack without requiring a manual validation and deploy start for each.
4. Click **Configurations** tab.
5. Click **Options** or **Kebab** menu of the stack configuration to expand.
6. Click **Validate and deploy** to begin the validation and start the deployment.

## Input variables for service configuration and deployment
{: #devsecops-alm-deploy-stack-input-variables}

The input variables for service configuration and deployment include both required and optional values that guide how services are set up and managed. This covers essential variables for service ID creation and operations, along with optional inputs customized for {{site.data.keyword.codeengineshort}}, {{site.data.keyword.containershort}}, and commonly shared configurations.
{: shortdesc}

### Required input variables
{: #devsecops-alm-deploy-stack-min}

To ensure a smooth deployment, you need to define the required input variables that guide the configuration.

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `region` | The region in which all resources are deployed, except for Security and Compliance Center and {{site.data.keyword.en_full}}. Those resources should be individually set and by default are set to us-south.| `string` | `us-south` |
| `en-region` | The region in which the {{site.data.keyword.en_short}} instance is provisioned.| `string` | `us-south` |
| `scc-region` | The region in which the Security and Compliance Center instance is provisioned.| `string` | `us-south` |
| `app_repo_existing_url` | Bring your own existing application repository by providing the URL. It creates an integration for your application repository instead of cloning the default sample. Repositories existing in a different organization requires the use of the Git token. See `app_repo_git_token_secret_name` optional variables. | `string` | `` |
{: caption="Required input variables" caption-side="top"}

### Required for service ID creation and operations
{: #devsecops-alm-deploy-stack-service-id}

The creation of service IDs and their associated service API keys requires a Git token to set for successful pipeline operations. The tables provided details of the variable.

The variable `force_create_standard_api_key` determines whether a standard API key is created or a service API key. If `force_create_standard_api_key` is set to `false` meaning a service ID creates, then the following variables need to be set `create_git_token`, `repo_git_token_secret_name`, and `repo_git_token_secret_value`.
{: note}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `create_git_token` | Set to true to create a Git token secret in the specified {{site.data.keyword.secrets-manager_short}}, by using the name set in `repo_git_token_secret_name` and the value set in `repo_git_token_secret_value`.| `boolean` | `false` |
| `repo_git_token_secret_name` | The name for the Git token secret in {{site.data.keyword.secrets-manager_short}}.| `string` | `` |
| `repo_git_token_secret_value` | The value of the Git token secret that is created if `create_git_token` is set to true.| `string` | `` |
| `force_create_standard_api_key` | Setting to `true` creates a standard API key and access is scoped to the access group that the user is assigned. | `boolean` | `false` |
{: caption="Required input variables provisioning Service IDs" caption-side="top"}

### Optional {{site.data.keyword.codeengineshort}} specific input variables 
{: #devsecops-alm-deploy-stack-min-ce}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `project_ci_name` | The name of the {{site.data.keyword.codeenginefull_notm}} CI project.| `string` | `CI_Project` |
| `project_cd_name` | The name of the {{site.data.keyword.codeenginefull_notm}} CD project.| `string` | `CD_Project` |
| `app_repo_branch` | This is the repository branch that is used by the default sample application. Alternatively if app_repo_existing_url is provided, then the branch must reflect the default branch for that repository. Typically these branches are main or master.| `string` | `master` |
{: caption="{{site.data.keyword.codeenginefull_notm}} setup inputs" caption-side="top"}

### Optional Kubernetes specific input variables 
{: #devsecops-alm-deploy-stack-min-kube}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `ci_cluster_name` | The name of `dev` cluster.| `string` | `""` |
| `ci_cluster_namespace` | The name of `dev` cluster namespace.| `string` | `dev` |
| `ci_cluster_region` | The region containing the cluster.| `string` | `""` |
| `ci_cluster_resource_group` | The resource group containing the cluster.| `string` | `""` |
| `cd_cluster_name` | The name of the production cluster.| `string` | `""` |
| `cd_cluster_namespace` | The name of the production cluster namespace.| `string` | `prod` |
| `app_repo_branch` | The repository branch used by the default sample application. Alternatively, if `app_repo_existing_url` is provided, then the branch must reflect the default branch for that repository. Typically these branches are `main` or `master`.| `string` | `main` |
{: caption="Kubernetes setup inputs" caption-side="top"}

### Optional common input variables
{: #devsecops-alm-deploy-stack-min-opt}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `prefix` | The value set acts as a prefix for the various resources that get created. A `hyphen -` is automatically be inserted as a separator between the prefix and resource names.| `string` | `devsecops` |
| `resource_group_name` | The name of the resource group in which all the resources are created.| `string` | `devsecops` |
| `bucket_name` | The name of the {{site.data.keyword.os_full_notm}} bucket that is created as evidence storage for the DevSecOps toolchains.| `string` | `devsecops` |
| `create_icr_namespace` | Set to `true` to have Terraform create the registry namespace. Setting to `false` has the CI pipeline create the namespace if it does not exist. Note: If a Terraform destroy is used, the ICR namespace along with all images are removed.| `boolean` | `true` |
| `registry_namespace` | The name of the registry namespace where images are stored.| `string` | `devsecops` |
| `create_cd_instance` | Set to `true` to create a CD Service. This is required for running the DevSecOps toolchain pipelines and to successfully interact with a {{site.data.keyword.DRA_short}} integration.| `boolean` | `true` |
| `pipeline_ibmcloud_api_key_secret_name` | The name of the {{site.data.keyword.cloud_notm}} API key in {{site.data.keyword.secrets-manager_short}} that is used for running the pipelines.| `string` | `ibmcloud-api-key` |
| `ci_signing_key_secret_name` | The name of the signing key in {{site.data.keyword.secrets-manager_short}} that is required for signing the images.| `string` | `signing-key` |
| `cd_code_signing_cert_secret_name` | The name of the signing certificate in {{site.data.keyword.secrets-manager_short}} that is used for validating the integrity of signed images.| `string` | `signing-certificate` |
| `cos_api_key_secret_name` | The name of the {{site.data.keyword.cos_full_notm}} API key in {{site.data.keyword.secrets-manager_short}} that is used for reading or writing to the evidence bucket.| `string` | `cos-api-key` |
| `sm_secret_group` | The secrets group that is created in {{site.data.keyword.secrets-manager_short}} containing the secrets required by the DevSecOps toolchains.| `string` | `devsecops` |
| `sm_service_plan` | The pricing plan to use for {{site.data.keyword.secrets-manager_short}}. Options are `trial` or `standard`. | `string` | `standard` |
| `scc_service_plan` | The pricing plan is used for Security and Compliance Center. Options are `trial` or `standard`. | `string` | `standard` |
| `use_existing_resource_group` | Setting to `true` treats the `resource_group_name` as an existing resource group. Setting `false` provisions a new resource group based on the value in `resource_group_name`.| `string` | `false` |
| `app_repo_git_token_secret_name` | Name of the Git token secret in the secret provider that is used for accessing the sample (or bring your own) application repository| `string` | `""` |
| `existing_secrets_manager_crn` | The CRN of an existing {{site.data.keyword.secrets-manager_short}} instance| `string` | `""` |
| `force_create_standard_api_key` | Setting to `true` creates a standard API key and access is scoped to the access group that the user is assigned.| `boolean` | `false` |
| `ibmcloud_api` | The environment URL. When left unset defaults to `https://cloud.ibm.com`| `string` | `""` |
| `create_git_token` | Set to `true` to create a Git token secret in the specified {{site.data.keyword.secrets-manager_short}}, using the name set in `repo_git_token_secret_name` and the value set in `repo_git_token_secret_value`.| `boolean` | `false` |
| `repo_apply_settings_to_compliance_repos` | Set to `true` to apply the same settings to all the default compliance repositories. Set to `false` to apply these settings to only the sample application, pipeline configuration, and the deployment repositories.| `boolean`| `false` |
| `repo_git_token_secret_name` | The name for the Git token secret in {{site.data.keyword.secrets-manager_short}}.| `string` | `` |
| `repo_git_token_secret_value` | The value of the Git token secret that are created if `create_git_token` is set to `true`.| `string` | `` |
| `repo_group` | The region in which the Security and Compliance Center instance is provisioned.| `string` | `us-south` |
| `repo_git_provider` | By default sets as `hostedgit`, else set to `githubconsolidated` for GitHub repositories or `gitlab` for GitLab. Applies to all the default DevSecOps repositories that are created. The exception is the `Compliance Pipelines` repository. By default this always uses `hostedgit`. To include the Compliance pipelines repository under the same settings, set `compliance_pipeline_repo_use_group_settings` to true. | `string` | `""` |
| `repo_title` | The title of the server. For example, `My Git Enterprise Server`.| `string` | `""` |
| `repo_root_url` | (Optional) The root URL of the server. For example, `https://git.example.com`.| `string` | `us-south` |
| `repo_blind_connection` | Setting a value to `true` means that the server is not addressable on the public internet. {{site.data.keyword.cloud_notm}} cannot validate the connection details that you provide. Certain functions that requires API access to the Git server disables. The delivery pipeline works by using a private worker that has network access to the Git server.| `boolean` | `false` |
| `repo_git_id` | Set this value to `github` for github.com, `gitlabcustom` for GitLab or to the ID of a custom GitHub Enterprise server.| `string` | `""` |
| `evidence_repo_existing_url` | Set to use an existing evidence repository.| `string` | `""` |
| `issues_repo_existing_url` | Set to use an existing issues repository.| `string` | `""` |
| `inventory_repo_existing_url` | Set to use an existing inventory repository.| `string` | `""` |
| `cd_deployment_repo_existing_url` | Override to bring your own existing deployment repository URL, that is used directly instead of cloning the default deployment sample.| `string` | `""` |
| `change_management_existing_url` | Override to bring your own existing change management repository URL, which is used directly instead of cloning the default change management repository.| `string` | `""` |
| `create_triggers` | Set to `true` to create the triggers used by the DevSecOps pipelines.| `boolean` | `true` |
| `create_git_triggers` | Set to `true` to create the Git triggers used by the DevSecOps pipelines.| `boolean` | `true` |
| `compliance_pipeline_repo_use_group_settings` | Set to `true` to apply group level repository settings to the compliance pipeline repository. See `repo_git_provider` as an example.| `boolean` | `false` |
| `compliance_pipeline_repo_git_provider` | By default it gets set as `hostedgit`, else set to `githubconsolidated` for GitHub repositories or `gitlab` for GitLab.| `string` | `""` |
| `compliance_pipeline_repo_git_id` | Set this value to `github` for `github.com`, `gitlabcustom` for GitLab or to the ID of a custom GitHub Enterprise server.| `string` | `""` |
| `compliance_pipeline_existing_repo_url` | Override to bring your own existing compliance pipelines repository URL, which is used directly instead of cloning the default change compliance pipelines repository.| `string` | `""` |
| `add_pipeline_definitions` | Set to `true` to add the compliance pipelines definitions to the DevSecOps pipelines.| `boolean` | `true` |
| `create_privateworker_secret` | Set to `true` to add a specified private worker service API key to the Secrets Provider.| `boolean` | `false` |
| `enable_privateworker` | Set to `true` to enable private workers for the CI, CD, CC, and PR pipelines. A valid service API key must be set in {{site.data.keyword.secrets-manager_short}}. The name of this secret can be specified by using `privateworker_credentials_secret_name`.| `boolean` | `false` |
| `privateworker_credentials_secret_name` | Name of the private worker secret in the secret provider.| `string` | `""` |
| `privateworker_secret_value` | The private worker service API key that is added to the `privateworker_credentials_secret_name` secret in the secrets provider.| `password` | `""` |
| `toolchain_access_group_name` | The name of the DevSecOps access group that is created.| `string` | `"devsecops-toolchain"` |
| `use_legacy_ref` | Set to `true` to use the old secret reference format for {{site.data.keyword.secrets-manager_short}} secrets.| `boolean` | `true` |
{: caption="Optional inputs" caption-side="top"}

## Permissions for toolchain operations
{: #devsecops-alm-deploy-stack-permission}

There are two scenarios that are supported for toolchains and the steps for both cases are outlined.

- The toolchains are operated by a service API key for accessing the {{site.data.keyword.cos_full_notm}} bucket. A service API key for running of the pipelines and an access group providing permissions for the developers or users. A Git token is also required to access the repositories.

- The toolchains are operated by a service API key for accessing the {{site.data.keyword.cos_full_notm}} bucket and an access groups for the API key for pipeline operations and developer or a user access. A Git token can optionally be used to configure the repository access.

### {{site.data.keyword.cos_full_notm}} service API key (cos-api-key)
{: #devsecops-alm-deploy-stack-cos-key}

The `cos-api-key` key gets created in both the service ID and standard API key setups.

| Service | Resources | Role | Note |
|------|-------------|------|--------|
| `{{site.data.keyword.cos_full_notm}}` | 1) Resource instance should be set to the {{site.data.keyword.cos_full_notm}} instance ID (not the CRN). 2) The resource type should be set to `bucket`. 3) The resource must be set to the name of the bucket. | `Object Reader`, `Writer` | The permissions for bucket access require these specific permissions and roles. |
{: caption="{{site.data.keyword.cos_full_notm}}  bucket permissions" caption-side="top"}

### Service ID and service API key (ibmcloud-api-key)
{: #devsecops-alm-deploy-stack-service-key}

The `ibmcloud-api-key` key gets created in the service ID set up with `force_create_standard_api_key` set to `false`.

| Service | Resources | Role | Note |
|------|-------------|------|--------|
| `Container Registry` | All | `Manager` | If the namespace exists the permissions can be reduced to `Reader` and `Writer` and scoped to a resource group. |
| `Toolchain` | Scoped to `dev-team-rg` resource group | `Editor` | |
| `Resource group` | Scoped to `dev-team-rg` resource group | `Editor` | |
| `{{site.data.keyword.codeenginefull_notm}}` | Scoped to `dev-team-rg` resource group. | `Manager`, `Editor` | |
| Or `Kubernetes Service` | Scoped to `dev-team-rg` resource group. | `Manager`, `Editor`| |
{: caption="Service API key permissions for ibmcloud-api-key" caption-side="top"}

### Access group
{: #devsecops-alm-deploy-stack-access-group}

The created access group provides broader access than required as it provides access both to developers and an API key in the standard API key setup with `force_create_standard_api_key` set to `true`. And allows for testing of the various components.

| Service | Resources | Role | Note |
|------|-------------|------|--------|
| `Container Registry` | All | `Manager` | If the namespace exists the permissions can be reduced to `Reader` and `Writer` and scoped to a resource group. |
| `Toolchain` | Scoped to `dev-team-rg` resource group. | `Editor` | |
| `Resource group` | Scoped to `dev-team-rg` resource group | `Editor` | |
| `Secrets Manager` | `Airgapped-StackALM` resource group | `Manager` ||
| `{{site.data.keyword.codeenginefull_notm}}` | Scoped to `dev-team-rg` resource group. | `Manager`, `Editor` | |
| Or `Kubernetes Service` | Scoped to `dev-team-rg` resource group. | `Manager`, `Editor`| |
{: caption="Standard API key permissions for ibmcloud-api-key" caption-side="top"}

### Restricted access group
{: #devsecops-alm-deploy-stack-restricted-grp}

Using a service API key for running the pipelines allows the developer access further gets reduced. The stack does not create this version of the access group.

| Service | Resources | Role | Note |
|------|-------------|------|--------|
| `Toolchain` | Scoped to `dev-team-rg` resource group. | `View`, `PipelineRunner` | Changing `PipelineRunner` to `Operator` develops to modify the toolchain settings. |
| `Resource group` | Scoped to `dev-team-rg` resource group. | `Viewer` | |
| `Secrets Manager` | `Airgapped-StackALM` resource group. | `Reader` ||
{: caption="Restricted access group" caption-side="top"}

## Create an {{site.data.keyword.cloud}} API key with permissions to create the deployable architecture
{: #devsecops-alm-deploy-stack-da-permission}

Create an {{site.data.keyword.cloud}} API key to create the deployable architecture.

Depending on your {{site.data.keyword.cloud_notm}} account type, access to certain resources might be limited. Depending on your account plan limits, certain capabilities that are required by some DevSecOps toolchains might not be available. For more information, see [Setting up your IBM Cloud account](/docs/account?topic=account-account-getting-started) and [Upgrading your account](/docs/account?topic=account-upgrading-account).
{: note}

## Create an {{site.data.keyword.cloud}} trusted profile with permissions to create the deployable architecture
{: #devsecops-alm-deploy-stack-da-tp-permisison}

Create an [{{site.data.keyword.cloud}} Trusted Profile](https://cloud.ibm.com/docs/account?topic=account-create-trusted-profile&interface=ui), and refer to, [Using trusted profiles to authorize a project to deploy an architecture](https://cloud.ibm.com/docs/secure-enterprise?topic=secure-enterprise-tp-project).
