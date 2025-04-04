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

The deployable architecture is designed to showcase a fully automated deployment of a sample node application through {{site.data.keyword.cloud}} Projects. It provides a flexible and customizable foundation for your own application deployments on {{site.data.keyword.cloud_notm}}. The architecture deploys a sample application by default or you can customize to bring your own application.
{: shortdesc}

By using this architecture, you can customize deployments to meet your unique business needs and enterprise goals. You can ensure the following when you deploy the architecture.

- **Implement security**: The architecture helps ensure security by deploying {{site.data.keyword.keymanagementservicelong}} and {{site.data.keyword.secrets-manager_full}}.

- **Achieve Regulatory Compliance**: Ensures regulatory compliance by implementing a set of DevSecOps Continuous Integration (CI), Continuous Deployment (CD), and Continuous Compliance (CC) toolchains, along with {{site.data.keyword.compliance_full}} for secure application lifecycle management.

## Before you deploy
{: #before-deploy-prereq}

The following prerequisites must be completed before you deploy the DevSecOps Solution for Apps Stack.

- A [Pay-As-You-Go](/docs/account?topic=account-linkedusage) or Subscription {{site.data.keyword.cloud}} account.

   Don't have one? [Create one](/docs/account?topic=account-account-getting-started). Have a trial or Lite account? [Upgrade your account](/docs/account?topic=account-upgrading-account).
   {: tip}

- The required level of access to deploy and manage resources in the account.

- An [IBM Cloud API Key](/docs/account?topic=account-userapikey&interface=terraform#create_user_key-api-terra) in the target account with the sufficient permissions. Be sure to save the API key value for a later configuration.

- **Evaluation environments**: Evaluates your environment is used for evaluating, grant an Administrator role on the **IAM Identity Service**, **All Identity and Access enabled services**, **Activity Tracker Event Routing** and **All Account Management** services.

- **Production environments**: Evaluates your environment is used for production resources, restrict access to the minimum permissions level as indicated in the **Permissions** tab of the details page of the deployable architecture catalog entry.

For more information, see [Using an API key with {{site.data.keyword.secrets-manager_short}} to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-authorize-project).

- Optional: Install the {{site.data.keyword.cloud_notm}} CLI project plug-in by running the `ibmcloud plugin install project` command. For more information, see the [Project CLI reference](/docs/cli?topic=cli-projects-cli).

- Optional: Familiarize yourself with the [Customization options]({[link]}-customize-css).

You might see notifications in {{site.data.keyword.cloud_notm}} projects that new versions of a configuration are available. You can ignore these messages because they do not prevent you from deploying the stack. No specific action is required from you. These notifications are expected, as we are rapidly iterating on the development of the underlying components. As new stack versions become available, the versions of the underlying components are also updated.
{: tip}

## Deployments
{: #devsecops-alm-stack-note}

The DevSecOps Solution for Apps Stack is highly configurable and allows for the reuse of existing services. By default, it deploys the DevSecOps CI, CD, and CC toolchains and all the required {{site.data.keyword.cloud_notm}} services to support the operations of these toolchains.

## Required permissions
{: #devsecops-alm-stack-permissions}

The following tables outline the permissions required for an `administrator` to successfully run the DevSecOps Solution for Apps Stack.

- The most permissive set of permissions allows for the creation of a resource group as well as service IDs for pipeline operations.
- These permissions can be reduced by scoping the access to an existing resource group in which to deploy the resources.
- The suggested configuration of the DevSecOps Solution stack is for the creation of service IDs to run the pipelines and an access group for adding developers, or users.
- By default the DevSecOps Solution for Apps Stack creates the service IDs. It is the requirement to provide a Git personal access token as a service ID is unable to access the repositories. Configuring the repository tool integrations with a Git personal access token is recommended configuration. 
- An added benefit of using the service ID is that Secrets Manager can rotate the service API keys secrets that are associated with the service IDs automatically.
- This rotation period is set to 90 days by default. Alternatively, instead of using a service API key that is linked to a service ID, a standard API key can be created for running the pipelines. The access for this key should be scoped by using an access group.

   The API key is used for running the pipelines that can be created either as a service API key or a standard API key. The API key that is created for accessing the Cloud Object Storage bucket is a service API key only. Auto rotation of the standard API setup is not supported.
   {: note}

- Permissions for creating service IDs are higher due to the need to create service authorizations. The tables have columns for `Role (Service IDs)` and `Role (Standard api key)` that highlights the different access requirements for setting up service IDs as opposed to create a standard API key.

### Administrator role for full deployment
{: #devsecops-alm-stack-admin-full}

Following are the access permissions that an administrator needs to create all the resources.

| Services | Resources | Role (Service IDs) | Role (Standard api key) |
|------|-------------|------|--------|
| `Toolchain` | `All` | `Administrator` | `Editor` |
| `Continuous Delivery` | `All` | `Administrator`| `Editor` |
| `Container Registry` | `All` | `Administrator` | `Editor` |
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
{: caption="Access permissions for creating all the resources" caption-side="top"}

### Administrator role for deployment using an existing resource group
{: #devsecops-alm-stack-admin-rg}

Following are the access permissions that are required for the resources with an existing resource group.

The name of the resource group for the purposes of the example is named `my-resource-group`

| Services | Resources | Role (Service IDs) | Role (Standard api key) |
|------|-------------|------|--------|
| `Toolchain` | scoped to`my-resource-group` resource group | `Administrator` | `Editor` |
| `Continuous Delivery` | scoped to`my-resource-group` resource group | `Administrator`| `Editor` |
| `Container Registry` | `All` | `Administrator` | `Manager` |
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
 

 ## What's Next?
 {: #devsecops-alm-stack-whatnext}

 Now that you successfully completed the prerequisties to deploy the DevSecOps Solution for Apps Stack, you can now [Deploy the DevSecOps Solution for Apps Stack](
 https://test.cloud.ibm.com/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-deploy-stack-req).
