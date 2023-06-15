---

copyright:
  years: 2023
lastupdated: "2023-06-15"

keywords:

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Deploying DevSecOps Application Lifecycle Management by using the {{site.data.keyword.cloud_notm}} CLI 
{: #devsecops-alm-cli}

Deploy an instance of the DevSecOps Application Lifecycle Management deployable architecture through the {{site.data.keyword.cloud_notm}} CLI.
{: shortdesc}

Follow these steps:

1. Create a `values.json` file on your local computer. `values.json` contains your custom configuration, for example product version, cluster, and namespace. A sample `values.json` that you can modify is as follows:

   ```json
   {
    "toolchain_name" : "DevSecOps",
    "toolchain_region" : "us-south",
    "toolchain_resource_group" : "Default",
    "registry_name_space" : "my-registry-namespace",
    "cluster_name" : "mycluster-free",
    "sm_location" : "us-south",
    "sm_name" : "sm-compliance-secrets",
    "sm_resource_group" : "Default",
    "sm_secret_group" : "Default"
   }
   ```
   {: codeblock}

1. Edit `values.json` to reflect your configuration. You can find your configuration values as follows:
   
   * You can specify your own toolchain name ("toolchain_name") and a toolchain region ("toolchain_region").
   * From the {{site.data.keyword.cloud_notm}} console, click  **Manage > Account > Resource groups** to find your resource group ("toolchain_resource_group").
   * From the {{site.data.keyword.cloud_notm}} console, click the menu icon ![Hamburger icon](../icons/icon_hamburger.svg) and click **Kubernetes**. Use the **Cluster** and **Container Registry** options to find the cluster and container registry details ("cluster_name" and "registry_name_space").
   * From the {{site.data.keyword.cloud_notm}} console, click the menu icon ![Hamburger icon](../icons/icon_hamburger.svg) and select **Resource List**. In the **Security** section, click the instance of Secrets Manager and then click **Details** to find the name ("sm_name"), resource group ("sm_resource_group") and location ("sm_location") of the Secrets Manager instance. 
   * Use the `ibmcloud secrets-manager secret-groups` command to list the available secret groups ("sm_secret_group"). For more information, see [ibmcloud secrets-manager secret-groups](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-groups-command).

1. Go to the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external} and search for the DevSecOps Application Lifecycle Management architecture.
1. Click the tile for the deployable architecture to open the details.
1. Select the latest product version in the **Architecture** section.
1. Select a variation, if more than one is available.
1. Agree to the terms and conditions on the Overview page and click **Review deployment options**.
1. Select the **Create from the CLI** deployment type in Deployment options.   
1. Copy and paste the `ibmcloud catalog install` command in the {{site.data.keyword.cloud_notm}} CLI. For more information, see [`ibmcloud catalog install`](/docs/cli?topic=cli-manage-catalogs-plugin#install-software-version).

1. Run `ibmcloud catalog install` to install your custom configuration. Choose one of the following options:

   1. Change directory to the directory where `values.json` is located and then run `ibmcloud catalog install`. For example:
   
      ```text
      ibmcloud catalog install --vl 1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.3aa45988-c0db-4e28-b01d-d31ef406280e-global --override-values values.json
      ```
      {: codeblock}

   1. Specify the path to `values.json` and then run `ibmcloud catalog install`. For example:
   
      ```text
      ibmcloud catalog install --vl 1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.3aa45988-c0db-4e28-b01d-d31ef406280e-global --override-values /<mypath>/values.json
      ```
      {: codeblock}

