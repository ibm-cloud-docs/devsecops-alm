---

copyright:
  years: 2023
lastupdated: "2023-04-17"

keywords: devsecops-alm, deployment guide, deployable architecture

subcollection: devsecops-alm

---

{{site.data.keyword.attribute-definition-list}}

# Why won't my deployment process finish successfully? 
{: #devsecops-alm-issue-analysis}

As part of the development of the Deployable Architecture we verify the workflows described in this guide. In the stable {{site.data.keyword.cloud}} environment, no errors are expected. But because several infrastructure layers and components are included in the deployment, you might see issues that aren't anticipated. For example, the maintenance activity in the particular data center, errors in DevSecOps Application Lifecycle Management, or network services, or issues in Operating System software updates can cause deployment failures.
{: tsSymptoms}

If the deployment process does not finish successfully, first look at the {{site.data.keyword.bplong}} logs. These logs typically provide the information about the component that causes the issue. 
{: tsCauses}

If you still cannot solve your issue, open a case in the {{site.data.keyword.cloud}} support center.
{: tsResolve}
