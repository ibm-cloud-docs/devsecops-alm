---

copyright:
  years: 2025
lastupdated: "2025-01-31"

keywords: devsecops-alm, deployment guide, deployable architecture

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Preparing to deploy DevSecOps toolchains in a firewalled environment
{: #devsecops-alm-firewall-req}

The following set of steps outline the configuration and permissions that are needed to create the DevSecOps Solution for Apps Stack in an IBM Cloud account when the a customers source code is hosted in a firewalled environment. The firewall needs to enable network connectivity between the customer environment and IBM Cloud endpoints.  


## Create an {{site.data.keyword.cloud}} service API key with required permissions fo DA creation
{: #devsecops-alm-key}
Refer to the permissions outlined in [DevSecOps Solutions for Apps Stack](/devsecops-alm?topic=devsecops-alm-devsecops-alm-stack-req#devsecops-alm-acc).

## Create an {{site.data.keyword.cloud}} service API key for Private Worker
{: #devsecops-alm-key}
Steps to create a Service API key with no permissions for the purposes of registering private worker.

## Create an access token for connecting source repository
{: #devsecops-alm-token}
Steps to create access token with required permissions

## Clone required repositories for DevSecOps to your firewalled environment
{: #devsecops-alm-repos}

Link to script to help clone repositories

## Variables to configure for a firewalled environment
{: #devsecops-alm-opt}

Scenario 1: Partially firewalled environment where access is permitted to the DevSecOps Managed pipelines on IBM Cloud. In this scenario, we have an application in our private GitLab instance that we want to use. The URL to the application is e.g `https://my-private-gitlab/test-user/code-engine-compliance-app` and the name of the git profile of the developer who has access is `test-user`. 



| Name | Value |
|------|-------------|
| `app_repo_branch` | `main` |
| `app_repo_existing_url` | `https://my-private-gitlab/test-admin/code-engine-app` |
| `use_app_repo_for_cd_deploy` | false |
| `cd_deployment_repo_existing_url` | `https://my-private-gitlab/test-admin/hello-compliance-deployment` |
| `create_git_token` | true |
| `create_triggers` | true |
| `create_git_triggers` | false |
| `compliance_pipeline_repo_use_group_settings` | true |
| `add_pipeline_definitions` | true |
| `create_privateworker_secret` | true |
| `enable_privateworker` | true |
| `privateworker_credentials_secret_name` | private-worker-key |
| `privateworker_secret_value` | `<Service-API-Key>` |
| `repo_apply_settings_to_compliance_repos` | false |
| `repo_title` | `My Git Enterprise Server`| 
| `repo_root_url` | `https://my-private-gitlab/` | 
| `repo_blind_connection` | true | 
| `repo_git_id` | `gitlabcustom` |
| `repo_group` | `test-user` |
| `repo_git_provider` | `gitlab` |
| `repo_git_token_secret_name` | `gitlab-token` |
| `repo_git_token_secret_value` | `<Access-Token>` |

If you are using an existing resource group, set `use_existing_resource_group` to `true` and `resource_group_name` to the name of the existing resource group you want to use. Additionally you need to set the `resource_group_scope` variable in the Security and Compliance Center member of the Stack by editing that configuration.
{: tip}

If you are using an application repository that also contains the deployment scripts such as the code engine example, `use_app_repo_for_cd_deploy` can be set to `true` and `cd_deployment_repo_existing_url` can be left unset.
{: tip}

Use `existing_secrets_manager_crn` if you already have an existing Secrets Manager instance you want to use.
{: tip}

Scenario 2: Fully firewalled environment where access is permitted to the DevSecOps Managed pipelines on IBM Cloud. In this scenario, we have an application in our private GitLab instance that we want to use. The URL to the application is e.g `https://my-private-gitlab/test-user/code-engine-compliance-app` . 

In this scenario, all the compliance repositories need to exist in the airgapped private GitLab instance. The URLs to these repositories need to be provided in the variables outlined in the table below. Additionaly `repo_apply_settings_to_compliance_repos` should be set to `true` to indicate that that all the repos should use the same settings.

| Name | Value |
|------|-------------|
| `evidence_repo_existing_url` | `https://my-private-gitlab/test-user/compliance-evidence-locker` |
| `issues_repo_existing_url` | `https://my-private-gitlab/test-admin/compliance-incident-issues` |
| `inventory_repo_existing_url` | `https://my-private-gitlab/test-user/compliance-inventory` |
| `cd_deployment_repo_existing_url` | `https://my-private-gitlab/test-admin/hello-compliance-deployment` |
| `change_management_existing_url` | `https://my-private-gitlab/test-admin/compliance-change-management` |
| `add_pipeline_definitions` | false |
| `repo_apply_settings_to_compliance_repos` | true |
| `compliance_pipeline_repo_use_group_settings` | `true` |
| `compliance_pipeline_existing_repo_url` | `https://my-private-gitlab/test-admin/compliance-pipelines` |

`compliance_pipeline_repo_use_group_settings` can optionally be set to `false` and `add_pipeline_definitions` set to `true`. This would exclude the need to self host and manage the repository containing the pipeline definitions and instead always use the latest version as supplied and hosted by IBM. This is only suitable to adopters that are able to make an exeption for this repository given their hosting and security requirements.
{: tip}


## Installing private worker
{: #devsecops-alm-private-worker}
Steps to install private worker..
https://cloud.ibm.com/docs/ContinuousDelivery?topic=ContinuousDelivery-install-private-workers&interface=cli

DevSecOps Endpoint documentation -


## Creating Generic webhook Trigger
{: #devsecops-alm-webhook-trigger}
Creating Generic Webhooks 

## Creating source repository webhook
{: #devsecops-alm-key-repo-trigger}
