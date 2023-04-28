---

copyright:
  years: 2023
lastupdated: "2023-04-28"

keywords:

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Deploying DevSecOps Application Lifecycle Management by using {{site.data.keyword.cloud_notm}} projects
{: #devsecops-alm-proj}

Deploy an instance of the DevSecOps Application Lifecycle Management deployable architecture through {{site.data.keyword.cloud_notm}} projects.
{: shortdesc}

To deploy, follow these steps:

1.  Make sure that you comply with the prerequisites.
1.  Go to the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external} and search for the DevSecOps Application Lifecycle Management architecture.
1.  Click the tile for the deployable architecture to open the details.
1.  Select the latest product version in the **Architecture** section.
1.  Select a variation, if more than one is available.
1.  Agree to the terms and conditions on the Overview page and click **Review deployment options**.
1.  Select the **Add to project** deployment type in Deployment options, and then click **Add to project**.
    1.  Name your project, enter a description, and specify a configuration name. Click **Create**.
1.  Edit and validate the configuration:
    1.  Select your authentication method. You can use an existing secret in {{site.data.keyword.secrets-manager_short}} or add your API key directly. For more information, see [Using an API key or secret to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-authorize-project).
    1.  Enter values for other required fields from the **Required** tab. For more information about required fields, see [Required input variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars#devsecops-alm-min).
    1.  Optional: Specify other values from the **Optional** tab. For more information about optional values, see [Optional input variables](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-vars#devsecops-alm-opt). 
    1.  Save the configuration.
    1.  Click **Validate**. Validation takes a few minutes

        {{site.data.keyword.cloud_notm}} projects run a Code Risk Analyzer scan that includes a set of [{{site.data.keyword.compliance_short}} goals](/docs/code-risk-analyzer-cli-plugin?topic=code-risk-analyzer-cli-plugin-cra-cli-plugin#terraform-scc-goals). Controls that are part of the deployable architecture and supported by {{site.data.keyword.cloud_notm}} projects are checked. Any extra controls that are not included in the list of supported {{site.data.keyword.compliance_short}} goals are not checked when you validate the configuration.

        If the validation fails because of the Code Risk Analyzer scan, you can [troubleshoot the failure](/docs/secure-enterprise?topic=secure-enterprise-ts-na-failures#na-checks-fail).
1.  Click **Deploy** after the validation succeeds.

    Deploying the deployable architecture can take more than an hour. You are notified when the deployment is successful.

    The **Output** tab populates a website URL in addition to other values. Copy the website URL and paste it into your browser to view the website that is created from your configuration.

During the validation and deployment process, monitor the [needs attention items](/docs/secure-enterprise?topic=secure-enterprise-needs-attention-projects). The widget reflects any issue that occurs in your configurations.
{: remember}
