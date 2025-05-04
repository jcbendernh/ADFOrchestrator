# Using Azure Dta Factory as an Orchestrator for Azure Databricks and Microsoft Fabric

I have multiple customers that use a blended approach of both Azure Databricks and Microsoft Fabric. In most cases they are using Azure Data Factory as the primary ingestion engine from their source data to their Raw/Bronze environment as it can handle private connections to both on-premise and Azure VNets better than Databricks and Fabric and has the ability to share its Self Hosted Integration Runtime across multiple environments.

This poses the question of "how do you orchestrate all of these environments?"

Since both Azure Databricks and Microsoft fabric have the ability to trigger workflows via their rest APIs, we can use Azure Data Factory as that orchestration engine.   

Below are links for the following scenarios with instructions on how to use the Azure Data Factory Templates that I have provided as well.

- [Run an Azure Databricks Job](DatabricksJob.md)
- [Run an Azure Databricks DLT Pipeline](DatabricksDLTPipeline.md)
- Run a Microsoft Fabric Data Pipeline
- Run a Microsoft Fabric Data Notebook

Below is a Diagram of this approach.  Hope this helps.

![ADF Orchestrate](img/ADF Orchestrate.png)


