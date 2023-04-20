---

copyright:
  years: 2023
lastupdated: "2023-04-20"

keywords: devsecops-alm, deployment guide, deployable architecture

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Planning for the DevSecOps Application Lifecycle Management Deployable Architecture
{: #devsecops-alm-planning-items}

Before you begin the deployment, ensure that the prerequisites for the Deployable Architecture are in place.
{: shortdesc}

## Prerequisites for provisioning DevSecOps toolchains using the Terraform script 
{: #devsecops-alm-planning-req}

* Provision the DevSecOps toolchains by using the Terraform script.
* Implement DevSecOps toolchain operations.

### Create an {{site.data.keyword.cloud}} account
{: #devsecops-alm-acc}

Create an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}. Depending on your {{site.data.keyword.cloud_notm}} account type, access to certain resources might be limited. Depending on your account plan limits, certain capabilities that are required by some DevSecOps toolchains might not be available. For more information, see [Setting up your IBM Cloud account](/docs/account?topic=account-account-getting-started) and [Upgrading your account](/docs/account?topic=account-upgrading-account).

### Choose a secrets management vault
{: #devsecops-alm-vault}

Ensure that all of the secret values that you need are stored in a secrets management vault. [Comparison between {{site.data.keyword.secrets-manager_short}} and related {{site.data.keyword.cloud_notm}} services](/docs/secrets-manager?topic=secrets-manager-manage-secrets-ibm-cloud) can help you to choose from various secrets management and data protection offerings. If you don't already have an instance of the secrets management vault provider of your choice, create one. For information about {{site.data.keyword.secrets-manager_full}} see [Getting started with {{site.data.keyword.secrets-manager_short}}](https://cloud.ibm.com/docs/secrets-manager?topic=secrets-manager-getting-started).

### Create a resource group
{: #devsecops-alm-resgrp}

Create a resource group where all the relevant resources are collected. Resource groups organize your account resources in customizable groupings so that you can assign users access to multiple resources at a time. Every toolchain is associated with a resource group. By default, the toolchain is created in the `Default` resource group. For more information, see [Creating a resource group](/docs/account?topic=account-rgs&interface=ui#create_rgs).


## Prerequisites for provisioning DevSecOps toolchain operations 
{: #devsecops-alm-ops-req}

Use these steps to provision DevSecOps toolchain operations. These steps are not required for a Terraform deployment of the DevSecOps toolchains.

### Create a continuous delivery service
{: #devsecops-alm-cd}

Continuous delivery supports the repositories and pipelines in the toolchain. To create a continuous delivery service, see [Continuous Delivery](https://cloud.ibm.com/catalog/services/continuous-delivery){: external}.

You can evaluate the continuous delivery service for 30 days by using the lite plan.
{: note}

### Create a Kubernetes cluster
{: #devsecops-alm-k8}

Create either a [Kubernetes cluster](https://cloud.ibm.com/kubernetes/catalog/cluster/create){: external}, or an [OpenShift cluster](https://cloud.ibm.com/kubernetes/catalog/create?platformType=openshift){: external}. While you are evaluating the service, you can use the free pricing plan. The cluster might take some time to provision. As the cluster is created, it progresses through the following stages: Deploying, Pending, and Ready. For more information, see [Getting started with Container Registry](/docs/Registry?topic=Registry-getting-started).

### Create a {{site.data.keyword.registryshort}} namespace
{: #devsecops-alm-namespace}

Create a [{{site.data.keyword.registryshort_notm}} namespace](https://cloud.ibm.com/registry/namespaces){: external}. {{site.data.keyword.registryshort_notm}} provides a multi-tenant private image registry that you can use to store and share your container images with users in your {{site.data.keyword.cloud_notm}} account. Select the location for your namespace, and click **Create**. For more information, see [Getting started with Container Registry](/docs/Registry?topic=Registry-getting-started).

### Create a signing key
{: #devsecops-alm-signkey}

Create an image signing key with the proper encoding to sign your application Docker images. This key is required for the CI pipeline. The default secret name entry is `signing_key`. For more information, see [Generating a GPG key](/docs/devsecops?topic=devsecops-devsecops-image-signing).

### Create an {{site.data.keyword.cloud_notm}} API key
{: #devsecops-alm-apikey}

Create an [API key](https://cloud.ibm.com/iam/apikeys){: external}. Save the API key value by either copying, downloading it, or adding it to your vault. The default secret name entry is `ibmcloud_api_key`.

### Create an {{site.data.keyword.cos_full}} instance (optional)
{: #devsecops-alm-cos}

Create an {{site.data.keyword.cos_full_notm}} instance and bucket. For more information, see [Configuring {{site.data.keyword.cos_full_notm}} for storing evidence](/docs/devsecops?topic=devsecops-cd-devsecops-cos-config), and [What is {{site.data.keyword.cos_full_notm}}?](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage) 

The default {{site.data.keyword.cos_full_notm}} API key is `cos_api_key`.
