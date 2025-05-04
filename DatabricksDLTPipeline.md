# Run an Azure Databricks DLT Pipeline

## Overview
These instructions are an adaption of the [Leverage Azure Databricks Pipelines orchestration from Azure Data Factory](https://techcommunity.microsoft.com/blog/analyticsonazure/leverage-azure-databricks-Pipelines-orchestration-from-azure-data-factory/3123862) article that was published in Feb 22 by [Clinton W Ford](https://www.linkedin.com/in/clintonwford/) and [Leo Furlong](https://www.linkedin.com/in/leoafurlongiv/) from Databricks. 

For DLT Pipelines I have modified their instructions to accommodate utilizing the following REST calls: 
- [Start a pipeline](https://docs.databricks.com/api/azure/workspace/pipelines/startupdate)
- [List pipeline updates](https://docs.databricks.com/api/azure/workspace/pipelines/listupdates)
- [Get a pipeline update](https://docs.databricks.com/api/azure/workspace/pipelines/getupdate)

The result of this is included in the [Execute Databricks Pipeline using MI](https://github.com/jcbendernh/ADFOrchestrator/blob/main/files/Execute%20Databricks%20Pipeline%20using%20MI.zip) Azure Data Factory Template within this repo. Thus, you do not need to build this pipeline from scratch, all the steps are already included in the template.  You only need to follow the steps below.

If you do not have a Databricks DLT Pipeline already created to run this against, you can use the following instructions to create one that can be utilized in the steps below: [Tutorial: Run your first DLT pipeline](https://learn.microsoft.com/en-us/azure/databricks/dlt/tutorial-pipelines)

## Instructions
To utilize the Azure Data Factory template, download it to your computer and install it within your Azure Data Factory instance using the following steps: 
1. Within your Azure Data Factory workspace, Click the <b>Home</b> button at the top of the left navigation bar.
2. On the home page click <b>Pipeline Templates</b> under the <b>Discover more</b> section at the bottom of the page.
3. In the Template gallery, click the <b>Import pipeline template</b> in the upper right and upload the zipped file.
4. Once it is installed in the gallery, select it and click <b>Continue</b>. The following screenshot shows what it will look like in the gallery.<br>&nbsp;<br>
<img src="img/ExecuteDatabricksPipelineusingMI.png" alt="Execute Databricks Pipelines using MI" width="200"><br>

5. On the <b>Execute Databricks Pipeline using MI</b> screen, click the <b>Use this template</b> button in the bottom left.
6. This will bring you to the editor screen of the ADF Pipeline.  You will need to modify 2 values on the <b>Parameters</b> tab of the overall pipeline to use it.
    1. <b>PipelineID:</b> The Databricks Pipeline ID - This can be found on the Pipeline Listing within your Databricks Workspace.<br> 
        1. Click on <b>Pipelines</b> in the Navigation Bar and copy the ID value in the listing.<br>&nbsp;<br>
        <img src="img/PipelineID.png" alt="Pipeline ID" width="900">
    2. <b>DatabricksWorkspaceID:</b> This is in the Databricks URL. For example, you want to take the bolded portion of the following URL<br> 
    https://<b>adb-1235678910111213.8.</b>azuredatabricks.net/<br>&nbsp;<br>
    When finished, your parameters should look like the screenshot below.<br><img src="img/PipelineParameters.png" alt="Pipeline Parameters" width="600"><br>&nbsp;<br>
7. Finally, you will need to add your <b>Azure Data Factory Managed Identity</b> to your Databricks workspace and give it the proper permissions to execute a Pipeline.  To do so, grant the <b>Contributor</b> RBAC role to the Managed Identity in the Azure Databricks <b>Access Control (IAM)</b> blade of the Azure Portal.<br>&nbsp;</br> <img src="img/ADFRBAC.png" alt="ADF RBAC" width="900">

## Overview of the steps
Below are the overview of the steps utilized in this template.<br>&nbsp;<br>
<img src="img/PipelineOverview.png" alt="Pipeline Overview" width="600">

1. <b>Web Activity - Execute Start Pipeline API</b>: Utilizes the [Start a pipeline](https://docs.databricks.com/api/azure/workspace/pipelines/startupdate) Databricks REST API to trigger the specific DLT Pipeline within your Databricks workspace.<BR>
NOTE: This utilizes the universal Managed Identity of the Databricks service to make the REST call.
2. <b>Until Activity - Wait Until Pipeline Completes</b>: Creates a loop of checking the state of the Pipeline until it is not equal to "RUNNING".
    1. <b>Web Activity - List pipeline updates API</b>: This utilizes the [List pipeline updates](https://docs.databricks.com/api/azure/workspace/pipelines/listupdates) Databricks REST API to retrieve the Update ID of the specific DLT Pipeline.<br>
    NOTE: This utilizes the universal Managed Identity of the Databricks service to make the REST call.
    2. <b>Web Activity - Get a pipeline update API</b>: This utilizes the [Get a pipeline update](https://docs.databricks.com/api/azure/workspace/pipelines/getupdate) Databricks REST API to retrieve the Update State of the specific Workflow/Pipeline.<br>
    NOTE: This utilizes the universal Managed Identity of the Databricks service to make the REST call.
    3. <b>Variable Activity - Check Pipeline Status</b>: Applies an iif statement to identify the Pipeline as either "RUNNING" or not "RUNNING".
    4. <b>Wait Activity - Wait to Recheck API</b>: Waits "x" seconds based on the  pipeline parameter of <b>WaitSeconds</b>. 