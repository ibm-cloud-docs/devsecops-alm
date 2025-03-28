---

copyright:
  years: 2025
lastupdated: "2025-03-24"

keywords: devsecops-alm, deployment guide, deployable architecture

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Preparing to deploy the DevSecOps Solution for Apps Stack
{: #devsecops-alm-stack-req}

This deployable architecture is designed to showcase a fully automated deployment of a sample Node application through a {{site.data.keyword.cloud}} Project, providing a flexible and customizable foundation for your own application deployments on {{site.data.keyword.cloud}}. This architecture deploys a sample application by default or can be customized to bring your own application.

By leveraging this architecture, you can secure your deployments and tailor it to meet your unique business needs and enterprise goals.

{: shortdesc}

When you deploy the architecture, you can:

* **Implement security**: The architecture ensures security by deploying IBM Key Protect and IBM Secrets Manager.

* **Achieve Regulatory Compliance**: Ensures regulatory compliance by implementing a set of DevSecOps continuous integration (CI), continuous deployment (CD), and continuous compliance (CC) toolchains, along with IBM Security and Compliance Center for secure application lifecycle management.


## Before you deploy
{: #before-deploy-prereq}

Before you can deploy the DevSecOps Solution for Apps Stack, you must have the following prerequisites.

* A Pay-As-You-Go or Subscription {{site.data.keyword.cloud}} account. 

   Don't have one? [Create one](/docs/account?topic=account-account-getting-started). Have a Trial or Lite account? [Upgrade your account](/docs/account?topic=account-upgrading-account).
   {: tip}

* The required level of access to deploy and manage resources in the account.
* An [IBM Cloud API Key](/docs/account?topic=account-userapikey&interface=terraform#create_user_key-api-terra) in the target account with the sufficient permissions. Be sure to save the API key value for a later configuration.

   * **Evaluation environments**: If your environment is used for evaluating, grant the Administrator role on the **IAM Identity Service**, **All Identity and Access enabled services**, **Activity Tracker Event Routing** and **All Account Management** services.
   * **Production environments**: If your environment is used for production resources, restrict access to the minimum permissions level as indicated in the **Permissions** tab of the details page of the deployable architecture catalog entry.

   For more information, see [Using an API key with Secrets Manager to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-authorize-project).

* Optional: Install the {{site.data.keyword.cloud}} CLI Project plug-in by running the `ibmcloud plugin install project` command. For more information, see the [Project CLI reference](/docs/cli?topic=cli-projects-cli).
* Optional: Familiarize yourself with the [Customization options]({[link]}-customize-css).

You might see notifications in {{site.data.keyword.cloud}} Projects that new versions of a configuration are available. You can ignore these messages because they do not prevent you from deploying the stack. No specific action is required from you. These notifications are expected, as we are rapidly iterating on the development of the underlying components. As new stack versions become available, the versions of the underlying components are also updated.
{: tip}

## Deployments
{: #devsecops-alm-stack-note}

The DevSecOps Solution for Apps stack is highy configurable and allows for the reuse of existing services. By default, it will deploy the DevSecOps CI, CD and CC toolchains and all the required IBM Cloud services to support the operations of these toolchains.

## Required permissions
{: #devsecops-alm-stack-permissions}

The following tables outline the permissions required for an `administrator` to successfully run the DevSecOps Solution for Apps stack. The most permissive set of permissions allows for the creation of a resource group as well as service IDs for pipeline operations. These permissions can be reduced by scoping the access to an existing resource group in which to deploy the resources. The recommended configuration of the DevSecOps Solution stack is for the creation of Service IDs for the running of the pipelines and an access group for adding developers/users. By default the DevSecOps Solution for Apps stack will create these service IDs but, it is a requirement to provide a GIT personal access token as a Service ID will not be able to access the repositories otherwise. Configuring the repository tool integrations with a GIT personal access token is also the recommended configuration. An added benefit of using the Service ID is that Secrets Manager can rotate the service api keys secrets associated with the Service IDs automatically. This rotation period is set to 90 days by default. Alternatively, instead of using a service api key linked to a service ID, a standard api key can be created for running the pipelines. The access for this key should be scoped using an access group. Note, it is only the api key used for running the pipelines that can be created either as a service api key or a standard api key. The api key created for accessing the Cloud Object Storage bucket is a service api key only. Auto rotation of the standard api setup is not supported. Permissions for creating Service IDs are higher due to the need to create service authorizations. The tables have columns for `Role (Service IDs)` and `Role (Standard api key)` which highlight the different access requirements for for setting up Service IDs as opposed to creating a standard api key.

### Administrator role for full deployment
{: #devsecops-alm-stack-admin-full}

| Services | Resources | Role (Service IDs) | Role (Standard api key) |
|------|-------------|------|--------|
| `Toolchain` | `All` | `Administrator` | `Editor` |
| `Continuous Delivery` | `All` | `Administrator`| `Editor` |
| `Container Registry ` | `All` | `Administrator` | `Editor` |
| `Secrets Manager` | `All` | `Manager`, `Administrator` | `Manager`, `Administrator` |
| `Cloud Object Storage` | `All`| `Manager`,`Administrator` | `Manager`,`Administrator` |
| `Code Engine` or `Kubernetes Service` | `All` | `Administrator` | `Editor` |
| `Event Notifications` | `All` | `Administrator` | `Editor` |
| `User Managment` | `All` | `Administrator` | `Editor` |
| `IAM Access Groups Service` | `All` | `Administrator` | `Editor` |
| `IAM Identity Service` | `All`| `Administrator`| `Administrator` |
| `Security and Compliance Center` | `All` | `Manager`, `Administrator` | `Manager`, `Editor` |
| `Key Protect` | `All` | `Manager`, `Editor` | `Manager`, `Editor` |
| `All Identity and Access enabled services` | `All` | `Reader` | `Reader` | 
| `Resource Group` | `All` | `Administrator` | `Editor`|
{: caption="Access permissions for creating all the resources " caption-side="top"}

### Administrator role for deployment using an existing resource group
{: #devsecops-alm-stack-admin-rg}

The name of the resource group for the purposes of this example is called `my-resource-group`

| Services | Resources | Role (Service IDs) | Role (Standard api key) |
|------|-------------|------|--------|
| `Toolchain` | scoped to`my-resource-group` resource group | `Administrator` | `Editor` |
| `Continuous Delivery` | scoped to`my-resource-group` resource group | `Administrator`| `Editor` |
| `Container Registry ` | `All` | `Administrator` | `Manager` |
| `Secrets Manager` | scoped to`my-resource-group` resource group | `Manager`, `Administrator` | `Manager`, `Administrator` |
| `Cloud Object Storage` | scoped to`my-resource-group` resource group| `Manager`,`Administrator` | `Manager`,`Administrator` |
| `Code Engine` or `Kubernetes Service` | scoped to`my-resource-group` resource group | `Administrator` | `Editor` |
| `Event Notifications` | scoped to`my-resource-group` resource group | `Administrator` | `Editor` |
| `User Managment` | `All` | `Administrator` | `Editor` |
| `IAM Access Groups Service` | `All` | `Administrator` | `Editor` |
| `IAM Identity Service` | `All`| `Administrator`| |
| `Security and Compliance Center` | scoped to`my-resource-group` resource group | `Manager`, `Administrator` | `Manager`, `Editor` |
| `Key Protect` | scoped to`my-resource-group` resource group | `Manager`, `Editor` | `Manager`, `Editor` |
| `Resource Group` | scoped to`my-resource-group` resource group | `Administrator` | `Editor`|
{: caption="Access permissions for the resources with an existing resource group" caption-side="top"}
 
## Deploying the DevSecOps Solution for Apps stack
{: #devsecops-alm-stack-desc}

The stack is presented in a manner such that only a few variables are required to deploy everything. Refer to the tables in subsequent sections for more details  

## Adding to a project
{: #devsecops-alm-stack-project}
{: step}
1. Go to the [DevSecOps Solution For Apps](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-alm-stack){: external} catalog entry in the {{site.data.keyword.cloud_notm}} catalog.
1. Select the latest version in the **Architecture** section.
1. Select a variation, if more than one is available.
1. Click on the **Add to projects** button.
1. In the **Select a project for DevSecOps Solution For Apps** section, set a project name and review the configuration name, region and resource group before clicking on the create button. Alternatively, select from an exsiting project.

## Configuring the deployable architecture
{: #devsecops-alm-stack-configure}
{: step}

You are now ready to configure the security, required variables, and optional variables for a default deployment with steps for both the service api key and standard api key configurations.

1. In the **Configure** section, select your authentication method. You can use an existing secret in {{site.data.keyword.secrets-manager_short}}, add your API key directly or set a Trusted Profile. For more information, see [Using an API key or secret to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-authorize-project).
1. In the **Required** tab, enter values for required fields. In many cases, you can use the default option. For more information about required variables, see [Required input variables](/docs/devsecops-alm?topic=devsecops-alm-stack-prereqs#devsecops-alm-min).
1. In the **Optional** tab find the variable called `create_git_token` and toggle it to `true`.
1. Find the variable called `repo_git_token_secret_name` and set a name for your token. This will be the name that appears for the Git token secret in Secrets Manager
1. Find the variable called `repo_git_token_secret_value` and set the Git personal access token for access to the team's repositories
1. Find the variable called `repo_group` and set the name of the repositories group/owner name.
1. Find the variable called `force_create_standard_api_key` and ensure that it is set to `false`
1. To use a standard api key over the service api key. Set `force_create_standard_api_key` to `true`. In this case it is not necessary to set the variables in the previous steps relating to the Git personal access token.
1. Find the `prefix` variable and set a value for it. This will act as a prefix in the names of the provisioned resources.
1. Find the `resource_group_name` and set a name for the resource group that the stack will create. This will be prefixed by the value from the previous step when it is created.


## (Optional Step) Deploying to an existing resource group
{: #devsecops-alm-stack-optional-config}
{: step}

As the administrator, you can optionally deploy the resources to an existing resource group

1. Find the `resource_group_name` and set the name of the existing resource group in which the stack provisions the resources.
1. Find the `use_existing_resource_group` and this value to `true`
1. Click the save button at the top of the page. Do not validate at this point
1. Navigate back up to the Projects level and find the current stack. Expand the stack and locate the `Security and Compliance Center` member. Click on the Options/Kebab menu for it and select `edit`
1. This brings us to the configuration of the Security and Compliance Center. Click on the **Optional** tab.
1. Find the variable called `resource_groups_scope` and click on the edit button. Set the name of the existing resource group in quotes inide of the square brackets. For example ["my-resource-group"] and then click save.

## Running the stack
{: #devsecops-alm-stack-running}
{: step}

1. Navigate to the top level of the project. 
1. Click on the project level **Manage** tab
1. Click on the **Settings** option
1. Set `Auto-deploy` to on. This will deploy all the configurations of the stack without requiring a manual validation and deploy start for each.
1. Click on the **Configurations** tab.
1. Click on the options/Kebab menu of the stack configuration to expand it.
1. Click on **Validate and deploy**  The deployment will begin after the validation has completed.



## Required input variables
{: #devsecops-alm-min}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `region` | The region in which all resources are deployed except for SCC and Event Notifications. Those resources should be individually set and by default are set to us-south.| `string` | `us-south` |
| `en-region` | The region in which the Events Notification instance is provisioned.| `string` | `us-south` |
| `scc-region` | The region in which the Security and Compliance Center instance is provisioned.| `string` | `us-south` |
| `app_repo_existing_url` | Bring your own existing application repository by providing the URL. This will create an integration for your application repository instead of cloning the default sample. Repositories existing in a different org will require the use of Git token. See app_repo_git_token_secret_name under optional variables.| `string` | `` |
{: caption="Required input variables" caption-side="top"}

## Required for service ID creation and operations
{: #devsecops-alm-stack-service-id}

The creation of Service IDs and their associated service api keys requires a Git token to be set for successful pipeline operations. 
The variable `force_create_standard_api_key` determines whether a standard api key is created or a service api key. If `force_create_standard_api_key` is set to `false` meaning a Service ID will be created, then the following variables need to be set `create_git_token`, `repo_git_token_secret_name` and `repo_git_token_secret_value`.


| Name | Description | Type | Default |
|------|-------------|------|---------|
| `create_git_token` | Set to true to create a Git Token secret in the speficied Secrets Manager, using the name set in repo_git_token_secret_name and the value set in repo_git_token_secret_value.| `boolean` | `false` |
| `repo_git_token_secret_name` | The name for the Git Token secret in Secrets Manager.| `string` | `` |
| `repo_git_token_secret_value` | The value of the Git Token secret that is created if create_git_token is set to true.| `string` | `` |
| `force_create_standard_api_key` | Setting to `true` will instead create a standard apikey and access is scoped to the access group that the user has been assigned.| `boolean` | `false` |
{: caption="Required input variables provisioning Service IDs" caption-side="top"}



## Optional Code Engine specific input variables 
{: #devsecops-alm-min-ce}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `project_ci_name` | The name of the IBM Cloud Code Engine CI project.| `string` | `CI_Project` |
| `project_cd_name` | The name of the IBM Cloud Code Engine CD project.| `string` | `CD_Project` |
| `app_repo_branch` | This is the repository branch used by the default sample application. Alternatively if app_repo_existing_url is provided, then the branch must reflect the default branch for that repository. Typically these branches are main or master.| `string` | `master` |
{: caption="Code Engine setup inputs" caption-side="top"}

## Optional Kubernetes specific input variables 
{: #devsecops-alm-min-kube}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `ci_cluster_name` | The name of dev cluster.| `string` | `""` |
| `ci_cluster_namespace` | The name of dev cluster namespace.| `string` | `dev` |
| `ci_cluster_region` | The region containing the cluster.| `string` | `""` |
| `ci_cluster_resource_group` | The resource group containing the cluster.| `string` | `""` |
| `cd_cluster_name` | The name of production cluster| `string` | `""` |
| `cd_cluster_namespace` | The name of production cluster namespace.| `string` | `prod` |
| `app_repo_branch` | This is the repository branch used by the default sample application. Alternatively if app_repo_existing_url is provided, then the branch must reflect the default branch for that repository. Typically these branches are main or master.| `string` | `main` |
{: caption="Kubernetes setup inputs" caption-side="top"}

## Optional common input variables
{: #devsecops-alm-min-opt}

| Name | Description | Type | Default |
|------|-------------|------|---------|
| `prefix` | The value set in this variable when set acts as a prefix for the various resources that get created. A hyphen - will automatically be inserted as a separator between the prefix and resource names.| `string` | `devsecops` |
| `resource_group_name` | The name of the resource group in which all the resources are created.| `string` | `devsecops` |
| `bucket_name` | The name of the Cloud Object Storage bucket created as evidence storage for the DevSecOps toolchains.| `string` | `devsecops` |
| `create_icr_namespace` | Set to true to have Terraform create the registry namespace. Setting to false will have the CI pipeline create the namespace if it does not already exist. Note: If a Terraform destroy is used, the ICR namespace along with all images will be removed.| `boolean` | `true` |
| `registry_namespace` | The name of the registry namespace where images are stored.| `string` | `devsecops` |
| `create_cd_instance` | Set to true to create a Continuous Delivery Service. This is required for running the DevSecOps toolchain pipelines and to successfuly interact with a DevOps Insights integration.| `boolean` | `true` |
| `pipeline_ibmcloud_api_key_secret_name` | The name of the IBM Cloud api key in Secrets Manager which is used for running the pipelines.| `string` | `ibmcloud-api-key` |
| `ci_signing_key_secret_name` | The name of the signing key in Secrets Manager that is required for signing the images.| `string` | `signing-key` |
| `cd_code_signing_cert_secret_name` | The name of the signing certificate in Secrets Manager that is used for validating the integrity of signed images.| `string` | `signing-certificate` |
| `cos_api_key_secret_name` | The name of the Cloud Object Storage api key in Secrets Manager that is used for reading/writing to the evidence bucket.| `string` | `cos-api-key` |
| `sm_secret_group` | The secrets group created in Secrets Manager containing the secrets required by the DevSecOps toolchains.| `string` | `devsecops` |
| `sm_service_plan` | The pricing plan to use for Secrets Manager. Options are trial or standard. | `string` | `standard` |
| `scc_service_plan` | The pricing plan to use for Security and Compliance Center. Options are trial or standard. | `string` | `standard` |
| `use_existing_resource_group` | Setting to `true` will treat the resource_group_name as an existing resource group. Setting false will provision a new resource group based on the value in `resource_group_name`.| `string` | `false` |
| `app_repo_git_token_secret_name` | Name of the Git token secret in the secret provider used for accessing the sample (or bring your own) application repository| `string` | `""` |
| `existing_secrets_manager_crn` | The CRN of an existing Secrets Manager instance| `string` | `""` |
| `force_create_standard_api_key` | Setting to `true` will instead create a standard apikey and access is scoped to the access group that the user has been assigned.| `boolean` | `false` |
| `ibmcloud_api` | The environment URL. When left unset this will default to `https://cloud.ibm.com`| `string` | `""` |
| `create_git_token` | Set to true to create a Git Token secret in the speficied Secrets Manager, using the name set in repo_git_token_secret_name and the value set in repo_git_token_secret_value.| `boolean` | `false` |
| `repo_apply_settings_to_compliance_repos` | Set to `true` to apply the same settings to all the default compliance repositories. Set to `false` to apply these settings to only the sample application, pipeline config and the deployment repositories.| `boolean`| `false` |
| `repo_git_token_secret_name` | The name for the Git Token secret in Secrets Manager.| `string` | `` |
| `repo_git_token_secret_value` | The value of the Git Token secret that is created if create_git_token is set to true.| `string` | `` |
| `repo_group` | The region in which the Security and Compliance Center instance is provisioned.| `string` | `us-south` |
| `repo_git_provider` | By default this gets set as 'hostedgit', else set to 'githubconsolidated' for GitHub repositories or `gitlab` for GitLab. Applies to all the default DevSecOps repositories, except the `Compliance Pipelines` repository that get created at the time of running the DA. See `compliance_pipeline_repo_use_group_settings` to additionally apply the setting to the compliance pipelines. | `string` | `""` |
| `repo_title` | The region in which all resources are deployed except for SCC and Event Notifications. Those resources should be individually set and by default are set to us-south.| `string` | `us-south` |
| `repo_root_url` | (Optional) The Root URL of the server. e.g. https://git.example.com.| `string` | `us-south` |
| `repo_blind_connection` | Setting this value to `true` means the server is not addressable on the public internet. IBM Cloud will not be able to validate the connection details you provide. Certain functionality that requires API access to the git server will be disabled. Delivery pipeline will only work using a private worker that has network access to the git server.| `boolean` | `false` |
| `repo_git_id` | Set this value to `github` for github.com, `gitlabcustom` for GitLab or to the ID of a custom GitHub Enterprise server.| `string` | `""` |
| `evidence_repo_existing_url` | Set to use an existing evidence repository.| `string` | `""` |
| `issues_repo_existing_url` | Set to use an existing issues repository.| `string` | `""` |
| `inventory_repo_existing_url` | Set to use an existing inventory repository.| `string` | `""` |
| `cd_deployment_repo_existing_url` | Override to bring your own existing deployment repository URL, which is used directly instead of cloning the default deployment sample.| `string` | `""` |
| `change_management_existing_url` | Override to bring your own existing change management repository URL, which is used directly instead of cloning the default change management repository.| `string` | `""` |
| `create_triggers` | Set to `true` to create the triggers used by the DevSecOps pipelines..| `boolean` | `true` |
| `create_git_triggers` | Set to `true` to create the Git triggers used by the DevSecOps pipelines.| `boolean` | `true` |
| `compliance_pipeline_repo_use_group_settings` | Set to `true` to apply group level repository settings to the compliance pipeline repository. See `repo_git_provider` as an example.| `boolean` | `false` |
| `compliance_pipeline_repo_git_provider` | By default this gets set as 'hostedgit', else set to 'githubconsolidated' for GitHub repositories or `gitlab` for GitLab.| `string` | `""` |
| `compliance_pipeline_repo_git_id` | Set this value to `github` for github.com, `gitlabcustom` for GitLab or to the ID of a custom GitHub Enterprise server.| `string` | `""` |
| `compliance_pipeline_existing_repo_url` | Override to bring your own existing compliance pipelines repository URL, which is used directly instead of cloning the default change compliance pipelines repository.| `string` | `""` |
| `add_pipeline_definitions` | Set to `true` to add the compliance pipelines definitions to the DevSecOps pipelines.| `boolean` | `true` |
| `create_privateworker_secret` | Set to `true` to add a specified private worker service api key to the Secrets Provider.| `boolean` | `false` |
| `enable_privateworker` | Set to `true` to enable private workers for the CI, CD, CC and PR pipelines. A valid service api key must be set in Secrets Manager. The name of this secret can be specified using `privateworker_credentials_secret_name`.| `boolean` | `false` |
| `privateworker_credentials_secret_name` | Name of the private worker secret in the secret provider.| `string` | `""` |
| `privateworker_secret_value` | The private worker service api key that will be added to the `privateworker_credentials_secret_name` secret in the secrets provider.| `password` | `""` |
| `toolchain_access_group_name` | The name of the DevSecOps access group that is created.| `string` | `"devsecops-toolchain"` |
| `use_legacy_ref` | Set to `true` to use the legacy secret reference format for Secrets Manager secrets.| `boolean` | `true` |
{: caption="Optional inputs" caption-side="top"}

## Permissions for toolchain operations
There are two scenarios supported for this and the steps for both cases are outlined below.
- The toolchains are operated by a service api key for accessing the Cloud Object Storage bucket, a service api for the running of the pipelines and an access group providing permissions for the developers/users. A Git token is also required for access to the repositories.

- The toolchains are operated by a service api key for accessing the Cloud Object Storage bucket and an access group(s) for the api key for pipeline operations and developer/user access. A Git token can optionally be used to configure the repository access.

### Cloud Object Storage service api key (cos-api-key)
This key gets created in both the Service ID and standard api key setups

| Service | Resources | Role | Note |
|------|-------------|------|--------|
| `Cloud Object Storage` | 1). Resource instance should be set to the Cloud Object Storage instance ID (not the CRN). 2) The resource type should be set to “bucket”. 3) The resource should be set to the name of the bucket. | `Object Reader`, `Writer` | The permissions for bucket access require these specific permissions and roles |
{: caption="Cloud Object Stroage bucket permissions" caption-side="top"}

### Service ID and service api key (ibmcloud-api-key)
This key gets created in the Service ID set up with `force_create_standard_api_key` set to `false`

| Service | Resources | Role | Note |
|------|-------------|------|--------|
| `Container Registry` | All | `Manager` | If the namespace already exists the permissions can be reduced to `Reader` and `Writer` and scoped to a resource group |
| `Toolchain` | Scoped to “dev-team-rg” resource group | `Editor` | |
| `Resource group` | Scoped to “dev-team-rg” resource group | `Editor` | |
| `Code Engine` | Scoped to “dev-team-rg” resource group | `Manager`, `Editor` | |
| Or `Kubernetes Service` | Scoped to “dev-team-rg” resource group | `Manager`, `Editor`| |
{: caption="Service api key permissions for ibmcloud-api-key" caption-side="top"}

### Access group
The created access group provides broader access than required as it provides access both to developers and an api key in the standard api key setup with `force_create_standard_api_key` set to `true`. And it is to allow for testing of the various components.
| Service | Resources | Role | Note |
|------|-------------|------|--------|
| `Container Registry` | All | `Manager` | If the namespace already exists the permissions can be reduced to `Reader` and `Writer` and scoped to a resource group |
| `Toolchain` | Scoped to “dev-team-rg” resource group | `Editor` | |
| `Resource group` | Scoped to “dev-team-rg” resource group | `Editor` | |
| `Secrets Manager` | Airgapped-StackALM resource group | `Manager` ||
| `Code Engine` | Scoped to “dev-team-rg” resource group | `Manager`, `Editor` | |
| Or `Kubernetes Service` | Scoped to “dev-team-rg” resource group | `Manager`, `Editor`| |
{: caption="Standard api key permissions for ibmcloud-api-key" caption-side="top"}

### Restricted access group
Using a service api key for running the pipelines allows the developer access to be further reduced. The stack does not create this version of the access group.

| Service | Resources | Role | Note |
|------|-------------|------|--------|
| `Toolchain` | Scoped to “dev-team-rg” resource group | `View`, `PipelineRunner` | Changing `PipelineRunner` to `Operator` would allow develops to modify the toolchain settings |
| `Resource group` | Scoped to “dev-team-rg” resource group | `Viewer` | |
| `Secrets Manager` | Airgapped-StackALM resource group | `Reader` ||
{: caption="Restricted access group" caption-side="top"}

## Create an {{site.data.keyword.cloud}} API key with permissions to create the deployable achitecture
{: #devsecops-alm-acc}

Create an {{site.data.keyword.cloud}} API key

Depending on your {{site.data.keyword.cloud_notm}} account type, access to certain resources might be limited. Depending on your account plan limits, certain capabilities that are required by some DevSecOps toolchains might not be available. For more information, see [Setting up your IBM Cloud account](/docs/account?topic=account-account-getting-started) and [Upgrading your account](/docs/account?topic=account-upgrading-account).

## Create an {{site.data.keyword.cloud}} Trusted profile with permissions to create the deployable achitecture
{: #devsecops-alm-acc}

Create an {{site.data.keyword.cloud}} Trusted Profile
