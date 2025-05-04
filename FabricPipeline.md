# Run an Microsoft Fabric Data Pipeline
## Overview

**Work in Progress**

These instructions allow you to execute a Fabric Data Pipeline from Azure Data Factory.  The result of this is included in the [Execute Fabric Pipeline using SPN](https://github.com/jcbendernh/ADFOrchestrator/blob/main/files/Execute%20Fabric%20Pipeline%20using%20SPN.zip) Azure Data Factory Template within this repo. Thus, you do not need to build this pipeline from scratch, all the steps are already included in the template. You only need to follow the steps below.

Here is an overview of the Data Factory Pipeline<br>&nbsp;<br>
<img src="img/ADFFabricPipelineOverview" alt="Pipeline Overview" width="800">


The template utilizes the the following Fabric REST API call:
- [Job Scheduler - Run On Demand Item Job](https://learn.microsoft.com/en-us/rest/api/fabric/core/job-scheduler/run-on-demand-item-job?tabs=HTTP

## Prerequisites
- Azure Service Principal
- Azure Key Vault
- Key Vault Linked Service in Azure Data Factory
- REST API Enabled in Fabric Portal
- SPN roles in the target Fabric Workspace