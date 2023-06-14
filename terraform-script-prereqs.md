---

copyright:
  years: 2023
lastupdated: "2023-06-14"

keywords: devsecops-alm, deployment guide, deployable architecture

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Prerequisites for provisioning DevSecOps toolchains by using the Terraform script 
{: #devsecops-alm-planning-req}

Before you begin the deployment, ensure that the prerequisites for the deployable architecture are in place.
{: shortdesc}

* Provision the DevSecOps toolchains by using the Terraform script.
* Implement DevSecOps toolchain operations.

## Create an {{site.data.keyword.cloud}} account
{: #devsecops-alm-acc}

Create an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}. 

Depending on your {{site.data.keyword.cloud_notm}} account type, access to certain resources might be limited. Depending on your account plan limits, certain capabilities that are required by some DevSecOps toolchains might not be available. For more information, see [Setting up your IBM Cloud account](/docs/account?topic=account-account-getting-started) and [Upgrading your account](/docs/account?topic=account-upgrading-account).

## Choose a secrets management vault
{: #devsecops-alm-vault}

Ensure that all of the secret values that you need are stored in a secrets management vault. 

[Comparison between {{site.data.keyword.secrets-manager_short}} and related {{site.data.keyword.cloud_notm}} services](/docs/secrets-manager?topic=secrets-manager-manage-secrets-ibm-cloud) can help you to choose from various secrets management and data protection offerings. 

If you don't already have an instance of the secrets management vault provider of your choice, create one. For more information about {{site.data.keyword.secrets-manager_full}} see [Getting started with {{site.data.keyword.secrets-manager_short}}](https://cloud.ibm.com/docs/secrets-manager?topic=secrets-manager-getting-started).

## Create a resource group
{: #devsecops-alm-resgrp}

Create a resource group where all the relevant resources are collected. 

Resource groups organize your account resources in customizable groupings so that you can assign users access to multiple resources at a time. Every toolchain is associated with a resource group. By default, the toolchain is created in the `Default` resource group. For more information, see [Creating a resource group](/docs/account?topic=account-rgs&interface=ui#create_rgs).

## Install the {{site.data.keyword.cloud_notm}} CLI and the catalogs management plug-in
{: #devsecops-alm-inst-cli}

If you plan to deploy DevSecOps Application Lifecycle Management by using the {{site.data.keyword.cloud_notm}} CLI, you must install the {{site.data.keyword.cloud_notm}} CLI and the Catalogs management CLI plug-in as follows:

* For more information on installing the {{site.data.keyword.cloud_notm}} CLI, see [Installing the stand-alone IBM Cloud CLI](/docs/cli?topic=cli-install-ibmcloud-cli).
* For more information on installing the Catalogs management CLI plug-in, see [Catalogs management CLI plug-in](/docs/cli?topic=cli-manage-catalogs-plugin).

