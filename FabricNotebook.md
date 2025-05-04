# Run an Microsoft Fabric Notebook
## Overview

**Work in Progress**

These instructions allow you to execute a Fabric Notebook from Azure Data Factory.  The result of this is included in the [Execute Fabric Notebook using SPN](https://github.com/jcbendernh/ADFOrchestrator/blob/main/files/Execute%20Fabric%20Notebook%20using%20SPN.zip) Azure Data Factory Template within this repo. Thus, you do not need to build this pipeline from scratch, all the steps are already included in the template. You only need to follow the steps below.

Here is an overview of the Data Factory Pipeline<br>&nbsp;<br>
<img src="img/ADFFabricNotebookOverview.png" alt="Pipeline Overview" width="800">


The template utilizes the the following Fabric REST API call: [Job Scheduler - Run On Demand Item Job](https://learn.microsoft.com/en-us/rest/api/fabric/core/job-scheduler/run-on-demand-item-job?tabs=HTTP)

## Prerequisites
- Azure Service Principal
- Azure Key Vault
- Key Vault Linked Service in Azure Data Factory
- REST API Enabled in Fabric Portal
- SPN roles in the target Fabric Workspace
