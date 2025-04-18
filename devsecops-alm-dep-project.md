---

copyright:
  years: 2023
lastupdated: "2025-03-27"

keywords:

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Deploying DevSecOps Application Lifecycle Management by using {{site.data.keyword.cloud_notm}} projects
{: #devsecops-alm-proj}

This deployment guide walks you through how to deploy an instance of the DevSecOps Application Lifecycle Management deployable architecture by using {{site.data.keyword.cloud_notm}} projects. By completing this tutorial, you learn how to find the architecture, edit and validate the configuration, and deploy by using a [project](#x2035151){: term}.
{: shortdesc}

You might want to deploy by using a project to ensure that the configuration of your deployable architecture is always compliant, cost effective, and secure.


## Before you begin
{: #devsecops-alm-proj-prereqs}

Make sure that you complete the prerequisites in the planning topics:

- [Prerequisites for provisioning DevSecOps toolchains by using the Terraform script](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-planning-req)
- [Prerequisites for provisioning DevSecOps toolchain operations](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-ops-req)

## Adding to a project
{: #devsecops-alm-project}
{: step}

1. Go to the [DevSecOps Application Lifecycle Management](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-devsecops-alm){: external} catalog entry in the {{site.data.keyword.cloud_notm}} catalog.
1. Select the latest product version in the **Architecture** section.
1. Select a variation, if more than one is available.
1. Agree to the terms and conditions on the Overview page and click **Review deployment options**.
1. Select the **Add to project** deployment type in Deployment options, and then click **Add to project**.
1. Name your project, enter a description, and specify a configuration name. Click **Create**.

## Configuring the deployable architecture
{: #devsecops-alm-configure}
{: step}

You are now ready to configure the security, required variables, and optional variables.

1. In the **Configure** section, select your authentication method. You can use an existing secret in {{site.data.keyword.secrets-manager_short}} or add your API key directly. For more information, see [Using an API key or secret to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-authorize-project).
1. In the **Required** tab, enter values for required fields. In many cases, you can use the default option. For more information about required fields, see [Required input variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars#devsecops-alm-min).

    | Name | Description | Value |
    |---|---|---|
    | `toolchain_name` | Enter the prefix name for the toolchain. The toolchain name is appended with `CI Toolchain`, `CD Toolchain`, or `CC Toolchain` followed by a timestamp. | `DevSecOps` |
    | `toolchain_region` | Enter the region identifier that is used, by default, for all resource creation and service instance lookup. | `us-south` |
    | `toolchain_resource_group` | Enter the resource group that is used, by default, for all resource creation and service instance lookups. If you have more than one resource group in your account, choose a group. If not, you can use the default. | `Default` |
    | `registry_namespace` | Enter the namespace of the registry within the {{site.data.keyword.cloud_notm}} Container Registry region where the application image is stored. Namespaces need to be unique in the region that you selected. |`myregistry_free` |
    | `cluster_name` | Enter the name of the Kubernetes cluster that you already created. The assumption is that it is in the resource group you selected. You can modify this in Advanced options. | `mycluster_free` |
    | `sm_location` | Enter the region location of the Secrets Manager instance that you previously set up. | `us-south` |
    | `sm_name` | Enter the name of the Secrets Manager instance that you previously set up. | `sm-instance`|
    | `sm_resource_group`| Enter the resource group that contains the Secrets Manager instance that you previously set up. | `Default` |
    | `sm_secret_group` | Enter the group in Secrets Manager instance that you previously set up for organizing or grouping secrets.| `Default` |
    | `cc_pipeline_properties` | This JSON represents the pipeline properties belonging to the CC pipeline in the CC toolchain. Each element in the JSON represents a seperate pipeline property. Three attributes are required to create a property. These are the name field (how the name appears in the pipeline properties), the type (text, secure and enum) and then the value. Do not put secrets directly into JSON for the secure type, instead the value for a secret type should be a CRN to a secret in the configured secrets provider or a secret reference to a secret in the configured secrets provider.| (too big to list) |
    | `cd_pipeline_properties` | This JSON represents the pipeline properties belonging to the CD pipeline in the CD toolchain. Each element in the JSON represents a seperate pipeline property. Three attributes are required to create a property. These are the name field (how the name appears in the pipeline properties), the type (text, secure and enum) and then the value. Do not put secrets directly into JSON for the secure type, instead the value for a secret type should be a CRN to a secret in the configured secrets provider or a secret reference to a secret in the configured secrets provider.| (too big to list) |
    | `ci_pipeline_properties` | This JSON represents the pipeline properties belonging to the both the CI and PR pipelines in the CI toolchain. Each element in the JSON represents a seperate pipeline property. Three attributes are required to create a property. These are the name field (how the name appears in the pipeline properties), the type (text, secure and enum) and then the value. Do not put secrets directly into JSON for the secure type, instead the value for a secret type should be a CRN to a secret in the configured secrets provider or a secret reference to a secret in the configured secrets provider.| (too big to list) |
    | `app_repo_branch` | This is the repository branch used by the default sample application. Alternatively if `app_repo_existing_url` is provided, then the branch must reflect the default branch for that repository. Typically these branches are main or master.| `master` |
    | `app_repo_existing_url` | Bring your own existing application repository by providing the URL. This will create an integration for your application repository instead of cloning the default sample. Repositories existing in a different org will require the use of Git token. See app_repo_git_token_secret_name under optional variables.| `__NOTSET__` |
    

    | Name | Description | Value |
    |---|---|---|
    | `toolchain_name` | Enter the prefix name for the toolchain. The toolchain name is appended with `CI Toolchain`, `CD Toolchain`, or `CC Toolchain` followed by a timestamp. | `DevSecOps` |
    | `toolchain_region` | Enter the region identifier that is used, by default, for all resource creation and service instance lookup. | `us-south` |
    | `toolchain_resource_group` | Enter the resource group that is used, by default, for all resource creation and service instance lookups. If you have more than one resource group in your account, choose a group. If not, you can use the default. | `Default` |
    | `registry_namespace` | Enter the namespace of the registry within the {{site.data.keyword.cloud_notm}} Container Registry region where the application image is stored. Namespaces need to be unique in the region that you selected. |`myregistry_free` |
    | `ci_code_engine_project` | The name of the Code Engine project to use for the CI pipeline promoted code. The project is created if it does not already exist. |`Sample_CI_Project` |
    | `cd_code_engine_project` | The name of the Code Engine project to use for the CD pipeline promoted code. The project is created if it does not already exist. |`Sample_CD_Project` |
    | `registry_namespace` | Enter the namespace of the registry within the {{site.data.keyword.cloud_notm}} Container Registry region where the application image is stored. Namespaces need to be unique in the region that you selected. |`myregistry_free` |
    | `sm_location` | Enter the region location of the Secrets Manager instance that you previously set up. | `us-south` |
    | `sm_name` | Enter the name of the Secrets Manager instance that you previously set up. | `sm-instance`|
    | `sm_resource_group`| Enter the resource group that contains the Secrets Manager instance that you previously set up. | `Default` |
    | `sm_secret_group` | Enter the group in Secrets Manager instance that you previously set up for organizing or grouping secrets.| `Default` |
    | `cc_pipeline_properties` | This JSON represents the pipeline properties belonging to the CC pipeline in the CC toolchain. Each element in the JSON represents a seperate pipeline property. Three attributes are required to create a property. These are the name field (how the name appears in the pipeline properties), the type (text, secure and enum) and then the value. Do not put secrets directly into JSON for the secure type, instead the value for a secret type should be a CRN to a secret in the configured secrets provider or a secret reference to a secret in the configured secrets provider.| (too big to list) |
    | `cd_pipeline_properties` | This JSON represents the pipeline properties belonging to the CD pipeline in the CD toolchain. Each element in the JSON represents a seperate pipeline property. Three attributes are required to create a property. These are the name field (how the name appears in the pipeline properties), the type (text, secure and enum) and then the value. Do not put secrets directly into JSON for the secure type, instead the value for a secret type should be a CRN to a secret in the configured secrets provider or a secret reference to a secret in the configured secrets provider.| (too big to list) |
    | `ci_pipeline_properties` | This JSON represents the pipeline properties belonging to the both the CI and PR pipelines in the CI toolchain. Each element in the JSON represents a seperate pipeline property. Three attributes are required to create a property. These are the name field (how the name appears in the pipeline properties), the type (text, secure and enum) and then the value. Do not put secrets directly into JSON for the secure type, instead the value for a secret type should be a CRN to a secret in the configured secrets provider or a secret reference to a secret in the configured secrets provider.| (too big to list) |
    | `app_repo_branch` | This is the repository branch used by the default sample application. Alternatively if `app_repo_existing_url` is provided, then the branch must reflect the default branch for that repository. Typically these branches are main or master.| `main` |
    | `app_repo_existing_url` | Bring your own existing application repository by providing the URL. This will create an integration for your application repository instead of cloning the default sample. Repositories existing in a different org will require the use of Git token. See app_repo_git_token_secret_name under optional variables.| `__NOTSET__` |
   

1.  Optional: Specify other values from the **Optional** tab. For more information about optional values, see [Optional input variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars#devsecops-alm-opt).
1.  Click **Save**.


## Validating and deploying the deployable architecture
{: #devsecops-alm-validate}
{: step}

Now that you saved the configuration, you can validate and deploy the deployable architecture.

1. Click **Validate** and wait for validation to complete. Validation takes a few minutes.

1. Click **Deploy** after the validation succeeds. Deployment can take more than an hour. You are notified when the deployment is successful.

1. Copy the website URL that the **Output** tab populates and paste it into your browser to view the website that is created from your configuration.

During the validation and deployment process, monitor the [needs attention items](/docs/secure-enterprise?topic=secure-enterprise-needs-attention-projects). The widget reflects any issue that occurs in your configurations.
{: remember}
