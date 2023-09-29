---

copyright:
  years: 2023
lastupdated: "2023-09-29"

keywords:

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Deploying DevSecOps Application Lifecycle Management by using {{site.data.keyword.cloud_notm}} Schematics
{: #devsecops-alm-schema}

This deployment guide walks you through how to customize and deploy an instance of the DevSecOps Application Lifecycle Management deployable architecture by using {{site.data.keyword.cloud}} Schematics.
{: shortdesc}

## Before you begin
{: #devsecops-alm-begin}

### Review planning topics
{: #devsecops-alm-review}

Make sure that you complete the prerequisites in the planning topics:

- [Prerequisites for provisioning DevSecOps toolchains by using the Terraform script](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-planning-req)
- [Prerequisites for provisioning DevSecOps toolchain operations](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-ops-req)

### Create an {{site.data.keyword.cloud_notm}} API key
{: #devsecops-alm-api-key}

The DevSecOps Application Lifecycle Management deployable architecture requires an [{{site.data.keyword.cloud_notm}} API key](/docs/account?topic=account-manapikey) to deploy the toolchains. Cloud API keys are distinct from Service API keys. Service API keys cannot create pipelines in the toolchains and would cause the deployment process to fail. Create an [API key](https://cloud.ibm.com/iam/apikeys){: external}. The deployment process provisions the toolchains into the account that owns the API key.

Two similar `ibmcloud api key` entries are used. `ibmcloud_api_key` is used for creating the toolchains by using Schematics and is set during the deployment process. The other entry, `ibmcloud-api-key` is required to run the toolchains and is created in a secrets provider.
{: note}

### Configure your workspace
{: #devsecops-alm-wksp}

Your workspace is an {{site.data.keyword.bplong}} workspace that is configured to run the Terraform code that creates the toolchains.

To find the {{site.data.keyword.bplong_notm}} workspaces you created, go to [{{site.data.keyword.cloud}}](https://cloud.ibm.com){: external} and click **Menu** ![Menu icon](../icons/icon_hamburger.svg)> **Schematics** > **Workspaces**. You can create and deploy the DevSecOps continuous integration (CI), continuous deployment (CD), and continuous compliance (CC) toolchains by using the default variables.

| Variable | Description |
|------|-------------|
|Name |The name of the workspace.|
|Resource Group|The resource group where the workspace is created.|
|Location|The location where the workspace is created.|
{: caption="Table 1. Workspace variables" caption-side="top"}

## Deploy the architecture
{: #deploy}

To deploy DevSecOps Application Lifecycle Management toolchains by using Schematics, complete the following steps:

1. Go to the [DevSecOps Application Lifecycle Management](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-devsecops-alm){: external} catalog entry in the {{site.data.keyword.cloud_notm}} catalog.
1. Select the latest version in the **Architecture** section.
1. Select a variation, if more than one is available.
1. Agree to the terms and conditions on the Overview page and click **Review deployment options**.
1. Select **Deploy with {{site.data.keyword.cloud_notm}} Schematics**.
1. In the **Configure your workspace** section, review the workspace name and update if necessary.
1. Specify a resource group that is used, by default, for all resource creation and service instance lookups. If you have more than one resource group in your account, choose a group. If not, you can use the default.
1. Select a location.
1. Scroll down to **Set the input variables** and complete the **Required input variables**.

    | Required value | Action | Example |
    |---|---|---|
    | `toolchain_name` | Enter the prefix name for the toolchain. The toolchain name is appended with `CI Toolchain`, `CD Toolchain`, or `CC Toolchain` followed by a timestamp. | `DevSecOps` |
    | `toolchain_region` | Enter the region identifier that is used, by default, for all resource creation and service instance lookup. | `us-south` |
    | `toolchain_resource_group` | Enter the resource group that is used, by default, for all resource creation and service instance lookups. If you have more than one resource group in your account, choose a group. If not, you can use the default. | `Default` |
    | `registry_namespace` | Enter the namespace of the registry within the IBM Cloud Container Registry region where the application image is stored. Namespaces need to be unique in the region that you selected. |`my-registry-namespace` |
    | `cluster_name` | Enter the name of the Kubernetes cluster that you already created. The assumption is that it is in the resource group you selected. You can modify this in Advanced options. | `mycluster_free` |
    | `sm_location` | Enter the region location of the Secrets Manager instance that you previously set up. | `us-south` |
    | `sm_name` | Enter the name of the Secrets Manager instance that you previously set up. | `sm-instance`|
    | `sm_resource_group`| Enter the resource group that contains the Secrets Manager instance that you previously set up. | `Default` |
    | `sm_secret_group` | Enter the group in Secrets Manager instance that you previously set up for organizing or grouping secrets.| `Default` |
    {: caption="Table 1. List of required values for deployment" caption-side="bottom"}

    For more information, see [Required input variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars#devsecops-alm-min).
    {: note}

1. Optional. Click **Optional input variables** and modify any optional variables to extend the experience. For more information, see [Optional input variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars#devsecops-alm-opt).
1. Click **Deploy**.
1. Click **Generate plan** to create the resource deployable architecture.


