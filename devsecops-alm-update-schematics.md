---

copyright:
  years: 2023
lastupdated: "2023-04-19"

keywords: devsecops-alm, deployment guide, deployable architecture

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Updating toolchains using Schematics
{: #devsecops-alm-update}

After the toolchains are deployed, you can update them by using Schematics. 
{: shortdesc}

Updates can include, for example, adding or removing integrations, or updating pipeline parameters. 

To update schematics, take the following steps:

1. Open [Schematics Workspaces](https://cloud.ibm.com/schematics/workspaces){: external} and click the relevant workspace for your DevSecOps toolchains. Use the following options: 

   * **Settings** lists all the inputs that you can edit.
   * **Jobs** shows the actions that are run by Schematics.

1. Click `Settings` and then search for the inputs that you want to update.

   The parameters prefixed with `ci`, `cd` and `cc` apply to the CI, CD, and CC toolchains respectively. Nonprefixed parameters apply to all the toolchains. Also, modifying the default values for the prefixed parameter inputs causes the prefixed parameters inputs to take precedence over nonprefixed parameter inputs. 
   {: note}

1. For each input that you want to update, take the following steps:

   1. Click the **Actions** icon ![Actions icon](../icons/actions-icon-vertical.svg) on an input.
   1. Update the values in the dialog that opens and click **Save**.

1. When you have updated all the required input changes, click **Jobs**. 
1. Click **Generate plan**.
1. On the **Jobs** page, verify that the plan succeeded. If not then review the problematic input changes.
1. Click **Apply plan** to update the toolchains. Check that the apply job succeededs.
