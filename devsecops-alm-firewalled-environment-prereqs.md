---

copyright:
  years: 2025, 2025
lastupdated: "2025-04-04"

keywords: devsecops alm, deployment guide, deployable architecture

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Preparing to deploy DevSecOps toolchains in a firewalled environment
{: #devsecops-alm-firewall-req}

In a secure enterprise setting, the DevOps team builds toolchains in a private hosted environment to make sure compliance, control, and automation are fully functional in a toolchain. The team streamlines the software delivery to maintain the complete ownership of their infrastructure.

During the toolchain creation and execution in a private hosted environment, the DevOps team faces integrating challenges, security management issues through the pipeline workloads.

To overcome the challenges. Follow the steps that outlines the prerequisites, configuration and permissions to create the DevSecOps solution for the Apps Stack in an {{site.data.keyword.cloud}} account when the customers source code is hosted in a firewalled environment.

## Planning and setting infrastructure
{: #devsecops-alm-firewall-plan}

Identify your hosting environment such as on-premises data center, private cloud, or hybrid cloud, where to deploy, and how to network and secure your environment.

### Installing a private worker on your infrastructure
{: #devsecops-alm-firewall-install}

To install the private worker, see [installation of delivery pipeline private worker](
https://cloud.ibm.com/docs/ContinuousDelivery?topic=ContinuousDelivery-install-private-workers&interface=cli) documentation.

### Securing the environment
{: #devsecops-alm-firewall-secure}

You need to secure the environment by using IAM, secrets manager, API keys, or encryption. Create an {{site.data.keyword.cloud}} service API key with right permissions for Deployable Architecture (DA) creation. Refer to the permissions outlined in [DevSecOps Solutions for Apps Stack](/devsecops-alm?topic=devsecops-alm-devsecops-alm-stack-req#devsecops-alm-acc).
{: note}

- Create an {{site.data.keyword.cloud}} service API key for private worker.

- Create an access token for connecting the source repository.

### Configuring the network
{: #devsecops-alm-firewall-network}

The firewall must enable network connectivity between the customer environment and {{site.data.keyword.cloud_notm}} endpoints. For more information about {{site.data.keyword.cloud_notm}} access endpoints, see [{{site.data.keyword.cloud_notm}} endpoints](https://cloud.ibm.com/docs/devsecops?topic=devsecops-cd-devsecops-pipeline-hosts).

## Listing the Scenarios
{{: #devsecops-alm-scenarios}}

- [Scenario 1: Fully Air Gapped prerequisites](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-firewall-req#devsecops-alm-firewall-scenario1-prereq)
- [Scenario 1: Fully Air Gapped configuration](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-firewall-req#devsecops-alm-firewall-scenario1-config)
- [Scenario 1: (Optional) Fully Air Gapped configuration by using a pre-existing resource group](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-firewall-req#devsecops-alm-firewall-scenario-rg-config)
- [Scenario 1: Fully Air Gapped running of the stack](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-firewall-req#devsecops-alm-stack-running)
- [Scenario 2: Fully Air Gapped apart from compliance pipelines](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-firewall-req#devsecops-alm-stack-config-pipelines)
- [Scenario 3: Hybrid air gapped configuration](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-firewall-req#devsecops-alm-stack-config-hybrid)

## Configuring Git triggering
{: #devsecops-alm-firewall-triggering}

You need to create and configure pipeline triggers so that events on the firewalled repositories run the correct pipeline triggers.

Following are the three triggers:

- [Continuous Integration (CI) Pipeline trigger](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-firewall-req#devsecops-alm-firewall-ci-trigger) runs on application source code changes.
- [Pull Request (PR) Pipeline trigger](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-firewall-req#devsecops-alm-firewall-pr-trigger) runs on merge requests on the source code repository.
- [Continuous Delivery (CD) Pipeline trigger](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-firewall-req#devsecops-alm-firewall-cd-trigger) runs on merge requests on the inventory repository.

### CI pipeline trigger
{: #devsecops-alm-firewall-ci-trigger}

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click the **Menu** icon ![hamburger icon](images/icon_hamburger.svg) > **Platform Automation** > **Toolchains**.
3. Click your CI toolchain > `<your CI pipeline>`.
4. Get the pipelineID from the URL `https://cloud.ibm.com/devops/pipelines/tekton/<CI_PIPELINE_ID>?env_id=ibm:yp:<REGION>`
5. Create the following JSON file, `data_push.json` :

    ```JSON
    {
        "enabled": true,
        "event_listener": "ci-listener-gitlab",
        "filter": "header['x-gitlab-event'] == 'Push Hook' && body.ref == 'refs/heads/main'",
        "max_concurrent_runs": 1,
        "name": "Git Trigger",
        "secret": {
            "key_name": "X-Gitlab-Token",
            "source": "header",
            "type": "token_matches",
            "value": "<WEBHOOK_SECRET>"
        },
        "type": "generic",
        "worker": {
            "id": "inherit"
        }
    }
    ```
    {: codeblock}
  
6. Get IBM account credentials
   - Login to the {{site.data.keyword.cloud_notm}} CLI with `ibmcloud login`.
   - Obtain an IAM Bearer token as `IAM_TOKEN=$(ibmcloud iam oauth-tokens)`
7. Set Environment variables

    ```sh
    IAM_TOKEN=${IAM_TOKEN:33}
    CI_PIPELINE_ID=36127d0b-1858-404b-b3ee-7a68aefec994
    REGION=us-south
    ```
    {: codeblock}

8. Run the following CURL command.

    ```curl
    `curl -X POST --location --header "Authorization: Bearer ${IAM_TOKEN}" --header "Accept: application/json" --header "Content-Type: application/json" --data @data_push.json "https://api.${REGION}.devops.cloud.ibm.com/pipeline/v2/tekton_pipelines/${CI_PIPELINE_ID}/triggers"`
    ```
    {: pre}

   The JSON response from the CURL command looks as shown in the code block.

    ```json
    {
      "type": "generic",
      "name": "Git Trigger",
      "event_listener": "ci-listener-gitlab",
      "id": "ef110ae8-2395-4928-9dba-f80b49cab276",
      "filter": "header['x-gitlab-event'] == 'Push Hook' && body.ref == 'refs/heads/main'",
      "secret": {
        "type": "token_matches",
        "value": "hash:SHA3-512:7a4f5b0701a921c9054eb41d97258979389922592801d21d213bdb5aa99cb8aec71368153fe0a084e7c9e5be119976a8279cfa0e794de90036fe138a57e4fd31",
        "source": "header",
        "key_name": "X-Gitlab-Token"
      },
      "properties": [],
      "webhook_url": "https://devops-api.us-south.devops.cloud.ibm.com/v1/tekton-webhook/f6e201e0-ba61-4e88-9a1b-6fc81febb952/run/ef110ae8-2395-4928-9dba-f80b49cab276",
      "max_concurrent_runs": 1,
      "enabled": true,
      "href": "https://api.us-south.devops.cloud.ibm.com/pipeline/v2/tekton_pipelines/f6e201e0-ba61-4e88-9a1b-6fc81febb952/triggers/ef110ae8-2395-4928-9dba-f80b49cab276"
    }
    ```
    {: codeblock}

Copy the `href` field from the response and add the `private` keyword in the endpoint, as shown in the example - `https://api.private.us-south.devops.cloud.ibm.com/pipeline/v2/tekton_pipelines/f6e201e0-ba61-4e88-9a1b-6fc81febb952/triggers/ef110ae8-2395-4928-9dba-f80b49cab276`
{: note}

### Creating source repository webhook for CI pipeline
{: #devsecops-alm-firewall-ci-webhook}

You need to create a repository webhook to invoke the created trigger.

1. Navigate to the webhooks page of your firewalled source code repository, for example `https://my-private-gitlab/test-admin/code-engine-compliance-app/-/hooks`
2. Click the `Add new webhook`.
3. Complete the URL field by using the modified `href` field from the trigger created earlier. For example,`https://api.private.us-south.devops.cloud.ibm.com/pipeline/v2/tekton_pipelines/f6e201e0-ba61-4e88-9a1b-6fc81febb952/triggers/ef110ae8-2395-4928-9dba-f80b49cab276`
4. Choose a meaningful name. For example, `CI Pipeline webhook`.
5. Complete the Secret `token` with the `<WEBHOOK_SECRET>` value used when creating the pipeline trigger
6. Select **Push events** > **All branches** as the Trigger options.
7. Click **Add webhook** to persist your changes.

The value for the URL field can be retrieved from the trigger `Webhook URL` field value through the pipeline UI after the CI pipeline creation. The secret used by the webhook and trigger pair can be modified.
{: note}

### PR pipeline trigger
{: #devsecops-alm-firewall-pr-trigger}

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click the **Menu** icon ![hamburger icon](images/icon_hamburger.svg) > **Platform Automation** > **Toolchains**.
3. Click your PR toolchain > `<your CR pipeline>`.
4. Get the pipelineID from the URL. For example, `https://cloud.ibm.com/devops/pipelines/tekton/<PR_PIPELINE_ID>?env_id=ibm:yp:<REGION>`.
5. Create the following JSON file, `data_merge.json`.

    ```json
    {
        "enabled": true,
        "event_listener": "pr-listener-gitlab",
        "filter": "header['x-gitlab-event'] == 'Merge Request Hook' && (body.object_attributes.action == 'open' || body.object_attributes.action == 'update' || body.object_attributes.action == 'reopen') && body.object_attributes.target_branch == 'main'",
        "max_concurrent_runs": 1,
        "name": "PR Git Trigger",
        "secret": {
            "key_name": "X-Gitlab-Token",
            "source": "header",
            "type": "token_matches",
            "value": "<WEBHOOK_SECRET>"
        },
        "type": "generic",
        "worker": {
            "id": "inherit"
        }
    }
    ```
    {: codeblock}

6. Get IBM account credentials
   - Login to the {{site.data.keyword.cloud_notm}} CLI with `ibmcloud login`.
   - Obtain an IAM Bearer token as `IAM_TOKEN=$(ibmcloud iam oauth-tokens)`
7. Set Environment variables

    ```sh
    IAM_TOKEN=${IAM_TOKEN:33}
    CI_PIPELINE_ID=36127d0b-1858-404b-b3ee-7a68aefec994
    REGION=us-south
    ```
    {: codeblock}

8. Run the following CURL command.

   ```curl
   `curl -X POST --location --header "Authorization: Bearer ${IAM_TOKEN}" --header "Accept: application/json" --header "Content-Type: application/json" --data @data_merge.json "https://api.${REGION}.devops.cloud.ibm.com/pipeline/v2/tekton_pipelines/${PR_PIPELINE_ID}/triggers"`
   ```
   {: pre}

   The JSON response from the CURL command looks as shown in the code block.

    ```json
    {
      "type": "generic",
      "name": "PR Git Trigger",
      "event_listener": "ci-listener-gitlab",
      "id": "ef110ae8-2395-4928-9dba-f80b49cab276",
      "filter": "header['x-gitlab-event'] == 'Merge Request Hook' && (body.object_attributes.action == 'open' || body.object_attributes.action == 'update' || body.object_attributes.action == 'reopen') && body.object_attributes.target_branch == 'main'",
      "secret": {
        "type": "token_matches",
        "value": "hash:SHA3-512:7a4f5b0701a921c9054eb41d97258979389922592801d21d213bdb5aa99cb8aec71368153fe0a084e7c9e5be119976a8279cfa0e794de90036fe138a57e4fd31",
        "source": "header",
        "key_name": "X-Gitlab-Token"
      },
      "properties": [],
      "webhook_url": "https://devops-api.us-south.devops.cloud.ibm.com/v1/tekton-webhook/f6e201e0-ba61-4e88-9a1b-6fc81febb952/run/ef110ae8-2395-4928-9dba-f80b49cab276",
      "max_concurrent_runs": 1,
      "enabled": true,
      "href": "https://api.us-south.devops.cloud.ibm.com/pipeline/v2/tekton_pipelines/f6e201e0-ba61-4e88-9a1b-6fc81febb952/triggers/ef110ae8-2395-4928-9dba-f80b49cab276"
    }
    ```
    {: codeblock}

Copy the `href` field from the response and add the `private` keyword in the endpoint. For example, `https://api.private.us-south.devops.cloud.ibm.com/pipeline/v2/tekton_pipelines/f6e201e0-ba61-4e88-9a1b-6fc81febb952/triggers/ef110ae8-2395-4928-9dba-f80b49cab276`
{: note}

### Creating source repository webhook for PR pipeline
{: #devsecops-alm-firewall-pr-webhook}

You need to create a repository webhook to invoke the created trigger.

1. Navigate to the webhooks page of your firewalled source code repository. For example, `https://my-private-gitlab/test-admin/code-engine-compliance-app/-/hooks`.
2. Click the `Add new webhook`.
3. Complete the URL field using the modified `href` field from the trigger created earlier. For example, `https://api.private.us-south.devops.cloud.ibm.com/pipeline/v2/tekton_pipelines/f6e201e0-ba61-4e88-9a1b-6fc81febb952/triggers/ef110ae8-2395-4928-9dba-f80b49cab276`.
4. Choose a meaningful name. For example, `PR Pipeline webhook`.
5. Complete the Secret `token` with the `<WEBHOOK_SECRET>` value used when creating the pipeline trigger
6. Select **Push events** > **All branches** as the Trigger options.
7. Click **Add webhook** to persist your changes.

The value for the URL field can be retrieved from the trigger `Webhook URL` field value in the pipeline UI at any time post creation. The secret that is used by the webhook and trigger pair can be modified.
{: note}

### CD pipeline trigger
{: #devsecops-alm-firewall-cd-trigger}

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click the **Menu** icon ![hamburger icon](images/icon_hamburger.svg) > **Platform Automation** > **Toolchains**.
3. Click your CD toolchain > `<your CD pipeline>`.
4. Get the pipelineID from the URL. For example, `https://cloud.ibm.com/devops/pipelines/tekton/<CD_PIPELINE_ID>?env_id=ibm:yp:<REGION>`.
5. Create the following JSON file, `data_cd_merge.json` :

    ```json

        "enabled": true,
        "event_listener": "promotion-validation-listener-gitlab",
        "filter": "header['x-gitlab-event'] == 'Merge Request Hook' && (body.object_attributes.action == 'open' || body.object_attributes.action == 'reopen') && body.object_attributes.target_branch == 'prod'",
        "max_concurrent_runs": 1,
        "name": "Git Promotion Validation Trigger",
        "secret": {
            "key_name": "X-Gitlab-Token",
            "source": "header",
            "type": "token_matches",
            "value": "<WEBHOOK_SECRET>"
        },
        "type": "generic",
        "worker": {
            "id": "inherit"
        }
    ```
    {: codeblock}

  
6. Get IBM account credentials
   - Login to the {{site.data.keyword.cloud_notm}} CLI with `ibmcloud login`.
   - Obtain an IAM Bearer token as `IAM_TOKEN=$(ibmcloud iam oauth-tokens)`
7. Set Environment variables

    ```sh
    IAM_TOKEN=${IAM_TOKEN:33}
    CI_PIPELINE_ID=36127d0b-1858-404b-b3ee-7a68aefec994
    REGION=us-south
    ```
    {: codeblock}

8. Run the following CURL command.

   ```curl
   `curl -X POST --location --header "Authorization: Bearer ${IAM_TOKEN}" --header "Accept: application/json" --header "Content-Type: application/json" --data @data_cd_merge.json "https://api.${REGION}.devops.cloud.ibm.com/pipeline/v2/tekton_pipelines/${CD_PIPELINE_ID}/triggers"`
   ```
   {: codeblock}

   The JSON response from the CURL command looks as shown in the code block.

    ```json
    {
      "type": "generic",
      "name": "Git Promotion Validation Trigger",
      "event_listener": "ci-listener-gitlab",
      "id": "ef110ae8-2395-4928-9dba-f80b49cab276",
      "filter": "header['x-gitlab-event'] == 'Merge Request Hook' && (body.object_attributes.action == 'open' || body.object_attributes.action == 'reopen') && body.object_attributes.target_branch == 'prod'",
      "secret": {
        "type": "token_matches",
        "value": "hash:SHA3-512:7a4f5b0701a921c9054eb41d97258979389922592801d21d213bdb5aa99cb8aec71368153fe0a084e7c9e5be119976a8279cfa0e794de90036fe138a57e4fd31",
        "source": "header",
        "key_name": "X-Gitlab-Token"
      },
      "properties": [],
      "webhook_url": "https://devops-api.us-south.devops.cloud.ibm.com/v1/tekton-webhook/f6e201e0-ba61-4e88-9a1b-6fc81febb952/run/ef110ae8-2395-4928-9dba-f80b49cab276",
      "max_concurrent_runs": 1,
      "enabled": true,
      "href": "https://api.us-south.devops.cloud.ibm.com/pipeline/v2/tekton_pipelines/f6e201e0-ba61-4e88-9a1b-6fc81febb952/triggers/ef110ae8-2395-4928-9dba-f80b49cab276"
    }
    ```
    {: codeblock}

Copy the `href` field from the response and add the `private` keyword. For example, `https://api.private.us-south.devops.cloud.ibm.com/pipeline/v2/tekton_pipelines/f6e201e0-ba61-4e88-9a1b-6fc81febb952/triggers/ef110ae8-2395-4928-9dba-f80b49cab276`
{: note}

### Creating inventory repository webhook for CD pipeline
{: #devsecops-alm-firewall-cd-webhook}

You need to create a repository webhook to invoke the created trigger.

1. Navigate to the webhooks page of your firewalled source code repository. For example, `https://my-private-gitlab/test-admin/compliance-inventory2/-/hooks`.
2. Click the `Add new webhook`.
3. Complete the URL field by using the modified `href` field from the trigger created earlier. For example, `https://api.private.us-south.devops.cloud.ibm.com/pipeline/v2/tekton_pipelines/f6e201e0-ba61-4e88-9a1b-6fc81febb952/triggers/ef110ae8-2395-4928-9dba-f80b49cab276`.
4. Choose a meaningful name. For example, `Promotion Validation webhook`.
5. Complete the Secret `token` with the `<WEBHOOK_SECRET>` value used when creating the pipeline trigger
6. Select **Push events** > **All branches** as the Trigger options
7. Click **Add webhook** to persist your changes.

The value for the URL field can be retrieved from the trigger `Webhook URL` field value through the pipeline UI after the CD pipeline creation. The secret that is used by the webhook and trigger pair can be modified.
{: note}

## Scenario 1: Fully Air Gapped prerequisites
{: #devsecops-alm-firewall-scenario1-prereq}

In this scenario, you have self hosted an application repository in a private GitLab instance and all the DevSecOps compliance repositories required to operate the toolchains. For these steps the air gapped GitLab instance is referred to as `https://my-private-gitlab/` and the Git user, or owner as `test-user`

The required repositories can be downloaded from the following URLs. Us-south is the given example but can be replaced with any of the following: `us-south`, `eu-gb`, `au-syd`, `eu-de`, `jp-tok`, `jp-osa`, `eu-es`, or `ca-tor`.

| URL | Description |
|------|-------------|
| `https://us-south.git.cloud.ibm.com/open-toolchain/code-engine-compliance-app` | The sample {{site.data.keyword.codeengineshort}} application repository. |
| `https://us-south.git.cloud.ibm.com/open-toolchain/hello-compliance-app` | The sample Kubernetes application repository. |
| `https://us-south.git.cloud.ibm.com/open-toolchain/compliance-evidence-locker` | The compliance evidence repository. |
| `https://us-south.git.cloud.ibm.com/open-toolchain/compliance-incident-issues` | The compliance issues repository. |
| `https://us-south.git.cloud.ibm.com/open-toolchain/compliance-inventory` | The compliance inventory repository. |
| `https://us-south.git.cloud.ibm.com/open-toolchain/compliance-change-management` | The compliance change management repository. |
| `https://us-south.git.cloud.ibm.com/open-toolchain/hello-compliance-deployment` | The sample repository containing deployment scripts. Not required when using the {{site.data.keyword.codeengineshort}} sample that contains all the necessary scripts.|
| `https://us-south.git.cloud.ibm.com/open-toolchain/compliance-pipelines` | The compliance repository containing all the pipeline definitions. |
{: caption="The required repositories for a fully air gapped configuration" caption-side="bottom"}

The repositories or the contents of the repositories need to be mirrored into the air gapped environment.
For example, the URL to the sample code application is `https://my-private-gitlab/test-user/code-engine-compliance-app`

In this example, the repositories are set up as follows for the {{site.data.keyword.codeengineshort}} application.

| URL |
|-----|
| `https://my-private-gitlab/test-admin/code-engine-compliance-app` |
| `https://my-private-gitlab/test-admin/compliance-evidence` |
| `https://my-private-gitlab/test-admin/compliance-issues` |
| `https://my-private-gitlab/test-admin/compliance-inventory` |
| `https://my-private-gitlab/test-admin/compliance-change-management` |
| `https://my-private-gitlab/test-admin/compliance-pipelines` |
{: caption="The repositories after being setup in the air gapped private GitLab" caption-side="bottom"}


A Git personal access token is required to configure the repository tool integrations that are stood up as part of the DevSecOps toolchains and the service API key for the pipeline tool integration.

## Scenario 1: Fully Air Gapped configuration
{: #devsecops-alm-firewall-scenario1-config}

You are now ready to configure the security, required, and optional variables for an air gapped deployment by using the sample {{site.data.keyword.codeengineshort}} application.

The repository URLs are specific to this example and the URLs used should reflect your environment.
{: note}

1. In the **Configure** section, select your authentication method. You can use an existing secret in {{site.data.keyword.secrets-manager_short}}, add your API key directly, or set a Trusted Profile. For more information, see [Using an API key or secret to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-authorize-project).
2. In the **Required** tab, enter values for required fields.
3. There are three region fields:
    - The first declares the region in which all the services get are deployed.
    - The second is for the region in which the Security and Compliance service is deployed.
    - The third is for {{site.data.keyword.en_short}}. By default these are set to `us-south`. If you  change the default, ensure that the three regions are all set the same.
4. In the `app_repo_existing_url` set `https://my-private-gitlab/test-admin/code-engine-compliance-app`.
5. In the **Optional** tab, find the following variables and set the values as stated.
   - `create_git_token` - toggle to `true`.
   - `repo_git_token_secret_name` - set a name for your token. This is the name that appears for the Git token secret in {{site.data.keyword.secrets-manager_short}}.
   - `repo_git_token_secret_value` - set the Git personal access token to access the team's repositories.
   - `repo_group` - set the name of the repositories group, or owner name.
   - `repo_apply_settings_to_compliance_repos` - `true`
   - `repo_git_provider` - `gitlab`
   - `repo_title` - set a repository title name. For example `development server`.
   - `repo_root_url` - set according to your own private GitLab instance. For example, `https://my-private-gitlab`
   - `repo_blind_connection` - `true`
   - `repo_git_id` - `gitlabcustom`
   - `evidence_repo_existing_url` - `https://my-private-gitlab/test-admin/compliance-evidence`
   - `issues_repo_existing_url` - `https://my-private-gitlab/test-admin/compliance-issues`
   - `inventory_repo_existing_url` - `https://my-private-gitlab/test-admin/compliance-inventory`
   - `change_management_existing_url` - `https://my-private-gitlab/test-admin/compliance-change-management`
   - `compliance_pipeline_existing_repo_url` - `https://my-private-gitlab/test-admin/compliance-pipelines`
   - `app_repo_branch` - Make sure that the branch name matches the branch of an example. For the {{site.data.keyword.codeengineshort}} sample set as `main`.
   - `create_triggers` - `true`
   - `create_git_triggers` - `false`
   - `compliance_pipeline_repo_use_group_settings` - `true`
   - `create_privateworker_secret` - `true`
   - `enable_privateworker` - `true`
   - `privateworker_credentials_secret_name` - set a name that you would like to see the private worker secret appear as in {{site.data.keyword.secrets-manager_short}}.
   - `privateworker_secret_value` - Provide private worker service API key.
6. Click the **Save** button.

## Scenario 1: (Optional) Fully Air Gapped configuration using a pre-existing resource group
{: #devsecops-alm-firewall-scenario-rg-config}

1. Navigate to the projects level and find the current stack. Expand the stack and locate the `{{site.data.keyword.compliance_short}}` member. Click the **Options or Kebab** menu.
2. Select **Edit** > **Optional** tab, and find the following variables and set the values as stated.
   - `resource_group_name` - Provide the name of an existing resource group in which the stack provisions the resources.
   - `use_existing_resource_group` - `true`
3. Click the **Save** button. Do not validate at this point.
4. Navigate back up to the Projects level and find the current stack. Expand the stack and find the `{{site.data.keyword.compliance_short}}` member. Click the **Options or Kebab** menu.
5. Select  **Optional** tab to view the configuration of the {{site.data.keyword.compliance_short}}.
6. Edit the `resource_groups_scope` variable, and set the name of the existing resource group in quotes within the square brackets. For example ["my-resource-group"].
7. Click the **Save** button.

## Scenario 1: Fully Air Gapped running of the stack
{: #devsecops-alm-stack-running}

1. Navigate to the top level of the project.
2. Click the project level **Manage** > **Settings** option.
3. Set `Auto-deploy` on to deploy all the configurations of the stack without requiring each configuration deployment to start manually.
4. Click the **Configurations** tab.
5. Click the **options or Kebab** menu of the stack configuration to expand.
6. Click **Validate and deploy to start the deployment after the validation completes.

## Scenario 2: Fully Air Gapped apart from compliance pipelines
{: #devsecops-alm-stack-config-pipelines}

In this scenario, you can link to an existing compliance pipelines repository and allows you to always keep up to date with the latest pipeline definitions.

Otherwise, the user must periodically check the state of the repository at `https://us-south.git.cloud.ibm.com/open-toolchain/compliance-pipelines` and manually copy the changes to the air gapped private GitLab.
{: note}

1. Follow the steps as stated in [Scenario 1: Fully Air Gapped configuration](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-firewall-req#devsecops-alm-firewall-scenario1-config).
2. When you get to the step of setting `compliance_pipeline_existing_repo_url`, leave the value empty.
3. Find the input `compliance_pipeline_repo_use_group_settings` and set as `false`.
4. Continue the remaining steps of [Scenario 1: Fully Air Gapped configuration](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-firewall-req#devsecops-alm-firewall-scenario1-config).

## Scenario 3: Hybrid air gapped configuration
{: #devsecops-alm-stack-config-hybrid}

In this scenario, only the repositories containing application code and scripts are stored in the private GitLab instance. The remaining compliance repositories use the default IBM Git hosted solution.

1. Follow the steps as stated in [Scenario 1: Fully Air Gapped configuration](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-firewall-req#devsecops-alm-firewall-scenario1-config).
2. When you get to the step of setting `compliance_pipeline_existing_repo_url`, leave the value empty.
3. Find the input `compliance_pipeline_repo_use_group_settings` and set as `false`, and leave the following variable inputs as empty.
   - `evidence_repo_existing_url`
   - `issues_repo_existing_url`
   - `inventory_repo_existing_url`
   - `change_management_existing_url`
4. Find the input `repo_apply_settings_to_compliance_repos` and set as `false`.
5. Continue the remaining steps of [Scenario 1: Fully Air Gapped configuration](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-firewall-req#devsecops-alm-firewall-scenario1-config).

  | Name | Value |
  |------|-------------|
  | `app_repo_branch` | `main` |
  | `app_repo_existing_url` | `https://my-private-gitlab/test-admin/code-engine-app` |
  | `use_app_repo_for_cd_deploy` | false |
  | `cd_deployment_repo_existing_url` | `https://my-private-gitlab/test-admin/hello-compliance-deployment` |
  | `create_git_token` | true |
  | `create_triggers` | true |
  | `create_git_triggers` | false |
  | `compliance_pipeline_repo_use_group_settings` | true |
  | `add_pipeline_definitions` | true |
  | `create_privateworker_secret` | true |
  | `enable_privateworker` | true |
  | `privateworker_credentials_secret_name` | private-worker-key |
  | `privateworker_secret_value` | `<Service-API-Key>` |
  | `repo_apply_settings_to_compliance_repos` | false |
  | `repo_title` | `My Git Enterprise Server`| 
  | `repo_root_url` | `https://my-private-gitlab/` | 
  | `repo_blind_connection` | true | 
  | `repo_git_id` | `gitlabcustom` |
  | `repo_group` | `test-user` |
  | `repo_git_provider` | `gitlab` |
  | `repo_git_token_secret_name` | `gitlab-token` |
  | `repo_git_token_secret_value` | `<Access-Token>` |
  {: caption="Configuration of the {{site.data.keyword.codeengineshort}} compliance app scenario 1" caption-side="bottom"}

Following are the important tips to configure the parameter in the application

- If you are using an application repository that contains the deployment scripts such as the {{site.data.keyword.codeengineshort}} then you can set `use_app_repo_for_cd_deploy` to `true` and leave `cd_deployment_repo_existing_url` unset.

- If you already have an existing Secrets Manager instance that you need, then use `existing_secrets_manager_crn`.
