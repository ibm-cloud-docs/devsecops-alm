---

copyright:
  years: 2023
lastupdated: "2023-09-29"

keywords:

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Deploying DevSecOps Application Lifecycle Management by using the {{site.data.keyword.cloud_notm}} CLI
{: #devsecops-alm-cli}

This deployment guide walks you through how to deploy an instance of the DevSecOps Application Lifecycle Management deployable architecture by using the {{site.data.keyword.cloud_notm}} CLI.
{: shortdesc}

## Before you begin
{: #devsecops-alm-cli-prereqs}

Make sure that you complete the prerequisites in the planning topics:

- [Prerequisites for provisioning DevSecOps toolchains by using the Terraform script](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-planning-req)
- [Prerequisites for provisioning DevSecOps toolchain operations](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-ops-req)

## Create a values.json file
{: #devsecops-alm-cli-file}
{: step}

To use the CLI to deploy DevSecOps Application Lifecycle Management, you must create a `values.json` file that contains your custom configuration.

1. Create a starter `values.json` file on your local computer that you can modify with configuration values. You can use the following sample `values.json` to get started:

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

1. If you want a toolchain name other than `DevSecOps`, update the `toolchain_name` value.
1. Specify the toolchain region (`toolchain_region`) that is used for all resource creation and service instance lookup. For more information, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).
1. From the {{site.data.keyword.cloud_notm}} console, click  **Manage > Account > Resource groups** to find your resource group (`toolchain_resource_group`).
1. From the {{site.data.keyword.cloud_notm}} console, click the menu icon ![Hamburger icon](../icons/icon_hamburger.svg) > **Kubernetes**.
    - Click **Clusters** to find the cluster name (`cluster_name`).
    - Click **Container Registry** > **Namespaces** to find the Container Registry namespace (`registry_name_space`).
1. From the {{site.data.keyword.cloud_notm}} console, click the menu icon ![Hamburger icon](../icons/icon_hamburger.svg) > **Resource List** > **Security**. Click the instance of Secrets Manager and then click **Details**. Update the JSON file with the following Secrets Manager instance information:
   - name (`sm_name`),
   - resource group (`sm_resource_group`)
   - location (`sm_location`)
1. Run the [**`ibmcloud secrets-manager secret-groups`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-groups-command) CLI command to create a list of available secret groups (`sm_secret_group`).

   ```sh
   ibmcloud secrets-manager secret-groups
   ```
   {: codeblock}

1. Save the `values.json` file.

## Deploy the architecture by using the CLI
{: #devsecops-alm-cli-deploy}
{: step}

Now that you created your `values.json` file with your custom configuration, you can use the file in the CLI command.

1. Go to the [DevSecOps Application Lifecycle Management](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-devsecops-alm){: external} catalog entry in the {{site.data.keyword.cloud_notm}} catalog.
1. Select the latest product version in the **Architecture** section.
1. Select a variation, if more than one is available.
1. Agree to the terms and conditions on the Overview page and click **Review deployment options**.
1. Select the **Create from the CLI** deployment type in Deployment options.
1. Copy and paste the `ibmcloud catalog install` command in the {{site.data.keyword.cloud_notm}} CLI.
1. Update and run the command with one of the following options:

   1. Change directory to the directory where the `values.json` file is located and then run `ibmcloud catalog install`. For example:

      ```text
      ibmcloud catalog install --vl 1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.3aa45988-c0db-4e28-b01d-d31ef406280e-global --override-values values.json
      ```
      {: codeblock}

   1. Specify the path to the `values.json` file and then run `ibmcloud catalog install`. For example:

      ```text
      ibmcloud catalog install --vl 1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.3aa45988-c0db-4e28-b01d-d31ef406280e-global --override-values /<mypath>/values.json
      ```
      {: codeblock}

   For more information, see [`ibmcloud catalog install`](/docs/cli?topic=cli-manage-catalogs-plugin#install-software-version).
