---

copyright:
  years: 2024
lastupdated: "2025-03-27"

keywords: devsecops-alm, deployment guide, deployable architecture

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Prerequisites for provisioning DevSecOps toolchain operations
{: #devsecops-alm-ops-req}

Use these steps to provision DevSecOps toolchain operations. These steps are not required for a Terraform deployment of the DevSecOps toolchains.

## Create a continuous delivery service
{: #devsecops-alm-cd}

Continuous delivery supports the repositories and pipelines in the toolchain. To create a continuous delivery service, see [Continuous Delivery](https://cloud.ibm.com/catalog/services/continuous-delivery){: external}.

You can evaluate the continuous delivery service for 30 days by using the lite plan.
{: note}

## Create a Kubernetes cluster
{: #devsecops-alm-k8}

If you are using the Kubernetes variation of the DevSecOps Application Lifestyle Management deployable architecture, you need to create either a [Kubernetes cluster](https://cloud.ibm.com/kubernetes/catalog/cluster/create){: external}, or a [Red Hat OpenShift cluster](https://cloud.ibm.com/kubernetes/catalog/create?platformType=openshift){: external}.

While you are evaluating the service, you can use the free pricing plan. The cluster might take some time to provision. As the cluster is created, it progresses through the following stages: Deploying, Pending, and Ready. For more information, see [Getting started with Container Registry](/docs/Registry?topic=Registry-getting-started).

## Code Engine project
{: #devsecops-alm-k8-codeengine}

If you are using the Code Engine variation of the DevSecOps Application Lifestyle Management deployable architecture, you must have an account with permissions to create a Code Engine project. You can use an existing project, otherwise a project will be created automatically. For more information, see [Managing user access](/docs/codeengine?topic=codeengine-iam) and [Managing projects](/docs/codeengine?topic=codeengine-manage-project).

## Create a {{site.data.keyword.registryshort}} namespace
{: #devsecops-alm-namespace}

1. Create a [{{site.data.keyword.registryshort_notm}} namespace](https://cloud.ibm.com/registry/namespaces){: external}. {{site.data.keyword.registryshort_notm}} provides a multi-tenant private image registry that you can use to store and share your container images with users in your {{site.data.keyword.cloud_notm}} account.
2. Select the location for your namespace, and click **Create**.

For more information, see [Getting started with Container Registry](/docs/Registry?topic=Registry-getting-started).

## Create a GPG signing key and a GPG public key
{: #devsecops-alm-signkey}

Create an image signing key with the proper encoding to sign your application Docker images.

This key is required for the CI pipeline. The default secret name entry is `signing-key`. For more information, see [Generating a GPG key](/docs/devsecops?topic=devsecops-devsecops-image-signing).

The public key can be created using an existing GPG private key. It is required for the artifact signature validation step as part of the CD pipeline run. This can be generated during a CI pipeline run by adding the `print-code-signing-certificate` property and setting the value to `1`. The public key cert will be displayed in the logs of the CI pipeline run under the `build-artifact` step. The output should then be stored in a secrets provider. This is set by default from version `1.1.0`. For more information, see [Generating a GPG public key](/docs/devsecops?topic=devsecops-devsecops-publickey).

Alterntively, the DevSecOps Application Lifecycle Management deployable architecture can auto generate these keys. For more information, refer to [Update](/docs/devsecops-alm?topic=devsecops-alm-release-notesdevsecops-alm-nov2024) and [Variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars).

## Create an {{site.data.keyword.cloud_notm}} API key
{: #devsecops-alm-apikey}

Create an [API key](https://cloud.ibm.com/iam/apikeys){: external}.

Save the API key value by either copying, downloading it, or adding it to your vault. The default secret name entry is `ibmcloud_api_key`.

## Create an {{site.data.keyword.cos_full}} instance (optional)
{: #devsecops-alm-cos}

Create an {{site.data.keyword.cos_full_notm}} instance and bucket.

For more information, see [Configuring {{site.data.keyword.cos_full_notm}} for storing evidence](/docs/devsecops?topic=devsecops-cd-devsecops-cos-config), and [What is {{site.data.keyword.cos_full_notm}}?](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage)

The default {{site.data.keyword.cos_full_notm}} API key is `cos_api_key`.
