---

copyright:
  years: 2023
lastupdated: "2023-04-19"

keywords:

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Deploying DevSecOps Application Lifecycle Management by using {{site.data.keyword.cloud_notm}} Schematics
{: #devsecops-alm-schema}

Customize and deploy an instance of the DevSecOps Application Lifecycle Management deployable architecture by using {{site.data.keyword.cloud_notm}} Schematics.
{: shortdesc}

## Before you begin
{: #devsecops-alm-begin}

### Reviewing access requirements
{: #devsecops-alm-review}

The DevSecOps Application Lifecycle Management deployable architecture requires an [{{site.data.keyword.cloud}} API key](/docs/account?topic=account-manapikey) to deploy the toolchains. Cloud API keys are distinct from Service API keys. Service API keys cannot create pipelines in the toolchains. Using a Service API key would cause the deployment process to fail. Create an [API key](https://cloud.ibm.com/iam/apikeys){: external}. The deployment process provisions the toolchains into the account that owns the API key. 

Two similar `ibmcloud api key` entries are used. `ibmcloud_api_key` is used for creating the toolchains by using Schematics, which is set during the deployment process. The other entry, `ibmcloud-api-key` is required to run the toolchains and is created in a secrets provider.
{: note}

### Configuring your workspace
{: #devsecops-alm-wksp}

Your workspace is a {{site.data.keyword.bplong}} workspace that is configured to run the Terraform code that creates the toolchains. Any workspaces that you created previously are listed here.

| Parameter | Description | 
|------|-------------|
|Name |The name of the workspace.|
|Resource Group|The resource group where the workspace is created.|
|Location|The location where the workspace is created.|
{: caption="Table 1. Workspace parameters" caption-side="top"}

You can find the {{site.data.keyword.bplong_notm}} workspaces in [Workspaces](https://cloud.ibm.com/schematics/workspaces){: external}. Alternatively, go to [{{site.data.keyword.cloud}}](https://cloud.ibm.com){: external} and go to the {{site.data.keyword.bplong_notm}} workspaces by clicking **Menu** ![Menu icon](../icons/icon_hamburger.svg), then clicking **Schematics**, and then **Workspaces**. 

Create and deploy the DevSecOps continuous integration (CI), continuous deployment (CD), and continuous compliance (CC) toolchains by using the default parameters.

## Deploying
{: #deploy}

To deploy DevSecOps Application Lifecycle Management toolchains using Schematics, take the following steps:

1. Open the [{{site.data.keyword.cloud}} Catalog Solution Library](https://cloud.ibm.com/catalog){: external} page. 
1. Open the catalog tile for `DevSecOps Application Lifecycle Management`.
1. Select an architecture version (default is the latest version).
1. On the overview page, agree to the terms and conditions and click **Review deployment options**.
1. On the DevSecOps Application Lifecycle Management catalog page, click the **Deploy with {{site.data.keyword.cloud}} Schematics** option.
1. Specify a resource group and select a location in the **Configure your workspace**.
1. Scroll down to **Set the input variables** and complete the **Required input variables**. For more information on required variables, see [Required input parameters](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars#devsecops-alm-min). 
1. Optional. Click **Optional input variables** and modify any optional variables to extend the out of the box experience. For more information on optional variables, see [Optional input parameters](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars#devsecops-alm-opt). 
1. Click **Deploy**.
1. Click **Generate plan** to create the resource deployable architecture.

