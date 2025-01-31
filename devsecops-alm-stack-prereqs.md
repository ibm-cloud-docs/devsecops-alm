---

copyright:
  years: 2025
lastupdated: "2025-01-31"

keywords: devsecops-alm, deployment guide, deployable architecture

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Preparing to deploy the DevSecOps Solution for Apps Stack
{: #devsecops-alm-stack-req}

This deployable architecture is designed to showcase a fully automated deployment of a sample Node application through an {{site.data.keyword.cloud}} Project, providing a flexible and customizable foundation for your own application deployments on {{site.data.keyword.cloud}}. This architecture deploys a sample application by default or can be customized to bring your own application.

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


## Required input variables
{: #devsecops-alm-min}


| Name | Description | Type | Default |
|------|-------------|------|---------|
| `region` | The region in which all resources are deployed except for SCC and Event Notifications. Those resources should be individually set and by default are set to us-south.| `string` | `us-south` |
| `en-region` | The region in which the Events Notification instance is provisioned.| `string` | `us-south` |
| `scc-region` | The region in which the Security and Compliance Center instance is provisioned.| `string` | `us-south` |
| `app_repo_existing_url` | Bring your own existing application repository by providing the URL. This will create an integration for your application repository instead of cloning the default sample. Repositories existing in a different org will require the use of Git token. See app_repo_git_token_secret_name under optional variables.| `string` | `` |


## Optional input variables
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
| `use_existing_resource_group` | Setting to true will treat the resource_group_name as an existing resource group. Setting false will provision a new resource group based on the value in resource_group_name.| `string` | `devsecops` |
| `app_repo_branch` | This is the repository branch used by the default sample application. Alternatively if app_repo_existing_url is provided, then the branch must reflect the default branch for that repository. Typically these branches are main or master.| `string` | `main` |
| `app_repo_git_token_secret_name` | Name of the Git token secret in the secret provider used for accessing the sample (or bring your own) application repository| `string` | `` |
| `project_names` | The names of the projects to add the IBM Cloud Code Engine.| `array` | `["CI_Project","CD_Project"]` |
| `existing_secrets_manager_crn` | The CRN of an existing Secrets Manager instance| `string` | `` |
| `autostart` | Set to true to automatically run the CI pipeline. `boolean` | `false` |
| `create_git_token` | Set to true to create a Git Token secret in the speficied Secrets Manager, using the name set in repo_git_token_secret_name and the value set in repo_git_token_secret_value.| `boolean` | `false` |
| `custom_app_repo_title` | The title of the server. e.g. My Git Enterprise Server. Applies to the sample application repository, pipeline config repository and additionally the deployment repository of the CD toolchain. Takes precedence over repo_title, if also set.| `string` | `` |
| `custom_app_repo_root_url` | The Root URL of the server. e.g. https://git.example.com. Applies to the sample application repository, pipeline config repository and additionally the deployment repository of the CD toolchain. Takes precedence over repo_root_url, if also set.| `string` | `` |
| `custom_app_repo_blind_connection` | Setting this value to true means the server is not addressable on the public internet. IBM Cloud will not be able to validate the connection details you provide. Certain functionality that requires API access to the git server will be disabled. Delivery pipeline will only work using a private worker that has network access to the git server.| `boolean` | `false` |
| `custom_app_repo_git_id` | The Git ID for the application repositories. Used by the sample application repository, pipeline config repository and additionally the deployment repository of the CD toolchain. Takes precedence for these repositories over the value set in repo_git_id.| `string` | `` |
| `custom_app_repo_group` | Specify the Git user or group for your application. This must be set if the repository authentication type is pat (personal access token). Used by the sample application repository, pipeline config repository and additionally the deployment repository of the CD toolchain. Takes precedence for these repositories over the value set in repo_group. `string` | `` |
| `custom_app_repo_git_provider` | The Git provider type. Used by the sample application repository, pipeline config repository and additionally the deployment repository of the CD toolchain. Takes precedence for these repositories over the value set in repo_git_provider.| `string` | `` |
| `custom_app_repo_git_token_secret_name` | The name of the Git token secret in the secret provider used for accessing the sample application repository, pipeline config repository and additionally the deployment repository of the CD toolchain. Takes precedence for these repositories over the value set in repo_git_token_secret_name.| `string` | `` |
| `custom_app_repo_git_token_secret_value` | The personal access token that will be added to the custom_app_repo_git_token_secret_name secret in the secrets provider. Note if also using repo_git_token_secret_name to set a Git Token in Secrets Manager, the names of the secrets must be different.| `string` | `` |
| `repo_git_token_secret_name` | The name for the Git Token secret in Secrets Manager.| `string` | `` |
| `repo_git_token_secret_value` | The value of the Git Token secret that is created if create_git_token is set to true.| `string` | `` |
| `repo_group` | The region in which the Security and Compliance Center instance is provisioned.| `string` | `us-south` |
| `repo_git_provider` | Bring your own existing application repository by providing the URL. This will create an integration for your application repository instead of cloning the default sample. Repositories existing in a different org will require the use of Git token. See app_repo_git_token_secret_name under optional variables.| `string` | `us-south` |
| `repo_title` | The region in which all resources are deployed except for SCC and Event Notifications. Those resources should be individually set and by default are set to us-south.| `string` | `us-south` |
| `repo_root_url` | The region in which the Events Notification instance is provisioned.| `string` | `us-south` |
| `repo_blind_connection` | The region in which the Security and Compliance Center instance is provisioned.| `string` | `us-south` |
| `repo_git_id` | Bring your own existing application repository by providing the URL. This will create an integration for your application repository instead of cloning the default sample. Repositories existing in a different org will require the use of Git token. See app_repo_git_token_secret_name under optional variables.| `string` | `us-south` |
| `evidence_repo_existing_url` | The region in which all resources are deployed except for SCC and Event Notifications. Those resources should be individually set and by default are set to us-south.| `string` | `us-south` |
| `issues_repo_existing_url` | The region in which the Events Notification instance is provisioned.| `string` | `us-south` |
| `inventory_repo_existing_url` | The region in which the Security and Compliance Center instance is provisioned.| `string` | `us-south` |
| `cd_deployment_repo_existing_url` | Bring your own existing application repository by providing the URL. This will create an integration for your application repository instead of cloning the default sample. Repositories existing in a different org will require the use of Git token. See app_repo_git_token_secret_name under optional variables.| `string` | `us-south` |
| `change_management_existing_url` | The region in which all resources are deployed except for SCC and Event Notifications. Those resources should be individually set and by default are set to us-south.| `string` | `us-south` |
| `create_triggers` | The region in which the Events Notification instance is provisioned.| `string` | `us-south` |
| `create_git_triggers` | The region in which the Security and Compliance Center instance is provisioned.| `string` | `us-south` |
| `compliance_pipeline_repo_use_group_settings` | Bring your own existing application repository by providing the URL. This will create an integration for your application repository instead of cloning the default sample. Repositories existing in a different org will require the use of Git token. See app_repo_git_token_secret_name under optional variables.| `string` | `us-south` |
| `compliance_pipeline_repo_git_provider` | The region in which all resources are deployed except for SCC and Event Notifications. Those resources should be individually set and by default are set to us-south.| `string` | `us-south` |
| `compliance_pipeline_repo_git_id` | The region in which the Events Notification instance is provisioned.| `string` | `us-south` |
| `compliance_pipeline_existing_repo_url` | The region in which the Security and Compliance Center instance is provisioned.| `string` | `us-south` |
| `add_pipeline_definitions` | Bring your own existing application repository by providing the URL. This will create an integration for your application repository instead of cloning the default sample. Repositories existing in a different org will require the use of Git token. See app_repo_git_token_secret_name under optional variables.| `string` | `us-south` |
| `create_privateworker_secret` | The region in which all resources are deployed except for SCC and Event Notifications. Those resources should be individually set and by default are set to us-south.| `string` | `us-south` |
| `enable_privateworker` | The region in which the Events Notification instance is provisioned.| `string` | `us-south` |
| `privateworker_credentials_secret_name` | The region in which the Security and Compliance Center instance is provisioned.| `string` | `us-south` |
| `privateworker_secret_value` | Bring your own existing application repository by providing the URL. This will create an integration for your application repository instead of cloning the default sample. Repositories existing in a different org will require the use of Git token. See app_repo_git_token_secret_name under optional variables.| `string` | `us-south` |



## Create an {{site.data.keyword.cloud}} API key with permissions to create the deployable achitecture
{: #devsecops-alm-acc}

Create an {{site.data.keyword.cloud}} API key

Depending on your {{site.data.keyword.cloud_notm}} account type, access to certain resources might be limited. Depending on your account plan limits, certain capabilities that are required by some DevSecOps toolchains might not be available. For more information, see [Setting up your IBM Cloud account](/docs/account?topic=account-account-getting-started) and [Upgrading your account](/docs/account?topic=account-upgrading-account).

## Create an {{site.data.keyword.cloud}} Trusted profile with permissions to create the deployable achitecture
{: #devsecops-alm-acc}

Create an {{site.data.keyword.cloud}} Trusted Profile
