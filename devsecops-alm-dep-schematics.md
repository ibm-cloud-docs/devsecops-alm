---

copyright:
  years: 2023
lastupdated: "2025-03-27"

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
{: caption="Workspace variables" caption-side="top"}

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
    | `toolchain_name` | This variable specifies the root name for the CI, CD and CC toolchain names. A fixed suffix will automatically be appended. Setting `DevSecOps` will generate toolchains with the names `DevSecOps-CI-Toolchain`,  `DevSecOps-CD-Toolchain` and `DevSecOps-CC-Toolchain`. The full name of each toolchain can be set independently using `ci_toolchain_name`, `cd_toolchain_name`, and `cc_toolchain_name`. | `DevSecOps` |
    | `toolchain_region` | Enter the region identifier that is used, by default, for all resource creation and service instance lookup. | `us-south` |
    | `toolchain_resource_group` | Enter the resource group that is used, by default, for all resource creation and service instance lookups. If you have more than one resource group in your account, choose a group. If not, you can use the default. | `Default` |
    | `registry_namespace` | A unique namespace within the IBM Cloud Container Registry region where the application image is stored.|`my-registry-namespace` |
    | `cluster_name` | Name of the Kubernetes cluster where the application is deployed. This sets the same cluster name for both CI and CD toolchains. See `ci_cluster_name` and `cd_cluster_name` to set different cluster names. By default , the cluster namespace for CI will be set to `dev` and CD to `prod`. These can be changed using `ci_cluster_namespace` and `cd_cluster_namespace`. | `mycluster_free` |
    | `sm_location` | The region hosting the Secrets Manager instance. This applies to the CI, CD and CC Secret Manager integrations. | `us-south` |
    | `sm_name` | The name of an existing Secret Managers instance. This applies to the CI, CD and CC Secret Manager integrations. | `sm-instance`|
    | `sm_resource_group`| The name of the existing resource group containing the Secrets Manager instance for your secrets. This applies to the CI, CD and CC Secret Manager integrations. | `Default` |
    | `sm_secret_group` | The Secrets Manager secret group containing the secrets for the DevSecOps pipelines. | `Default` |
    {: caption="List of required values for deployment" caption-side="bottom"}

    For more information, see [Required input variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars#devsecops-alm-min).
    {: note}

1. Optional. Click **Optional input variables** and modify any optional variables to extend the experience. For more information, see [Optional input variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars#devsecops-alm-opt).
1. Click **Deploy**.
1. Click **Generate plan** to create the resource deployable architecture.
