---

copyright:
  years: 2023
lastupdated: "2023-06-08"

keywords:

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Deploying DevSecOps Application Lifecycle Management by using the {{site.data.keyword.cloud_notm}} CLI 
{: #devsecops-alm-cli}

Deploy an instance of the DevSecOps Application Lifecycle Management deployable architecture through the {{site.data.keyword.cloud_notm}} CLI.
{: shortdesc}

Follow these steps:

1. Make sure that you comply with the prerequisites.
1. Go to the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external} and search for the DevSecOps Application Lifecycle Management architecture.
1. Click the tile for the deployable architecture to open the details.
1. Select the latest product version in the **Architecture** section.
1. Select a variation, if more than one is available.
1. Agree to the terms and conditions on the Overview page and click **Review deployment options**.
1. Select the **Create from the CLI** deployment type in Deployment options.
1. On your local computer, create a `values.json` file to contain your custom configuration, for example product version, cluster, and namespace. A sample `values.json` that you can modify is as follows:

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
   
1. Copy and paste the `ibmcloud catalog install` command in the {{site.data.keyword.cloud_notm}} CLI. For more information, see [`ibmcloud catalog install`](/docs/cli?topic=cli-manage-catalogs-plugin#install-software-version).
1. Run the `ibmcloud catalog install` to install your custom configuration. 

