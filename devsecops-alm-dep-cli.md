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

1. Go to the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external} and search for the DevSecOps Application Lifecycle Management architecture.
1. Click the tile for the deployable architecture to open the details.
1. Select the latest product version in the **Architecture** section.
1. Select a variation, if more than one is available.
1. Agree to the terms and conditions on the Overview page and click **Review deployment options**.
1. Select the **Create from the CLI** deployment type in Deployment options.   
1. Copy and paste the `ibmcloud catalog install` command in the {{site.data.keyword.cloud_notm}} CLI. For more information, see [`ibmcloud catalog install`](/docs/cli?topic=cli-manage-catalogs-plugin#install-software-version).
   
   You must the `values.json` file that you created in [Create a `values.json` file](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-planning-req#devsecops-alm-inst-values) in your `ibmcloud catalog install` command.
   {: note}

1. Run `ibmcloud catalog install` to install your custom configuration. 

