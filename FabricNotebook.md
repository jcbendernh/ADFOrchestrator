# Run a Microsoft Fabric Notebook
## Overview

**Work in Progress**

These instructions allow you to execute a Fabric Notebook from Azure Data Factory.  The result of this is included in the [Execute Fabric Notebook using SPN](https://github.com/jcbendernh/ADFOrchestrator/blob/main/files/Execute%20Fabric%20Notebook%20using%20SPN.zip) Azure Data Factory Template within this repo. Thus, you do not need to build this pipeline from scratch, all the steps are already included in the template. You only need to follow the steps below.

Here is an overview of the Data Factory Pipeline<br>&nbsp;<br>
<img src="img/ADFFabricNotebookOverview.png" alt="Pipeline Overview" width="800">


The template utilizes the the following Fabric REST API call: [Job Scheduler - Run On Demand Item Job](https://learn.microsoft.com/en-us/rest/api/fabric/core/job-scheduler/run-on-demand-item-job?tabs=HTTP)

## Prerequisites
1. <b>Azure Service Principal</b> - We can use an Azure Service Principal (SPN) to make REST calls to the Fabric API. To do so, follow these steps.
    1. [Register an application with Microsoft Entra ID and create a service principal](https://learn.microsoft.com/en-us/entra/identity-platform/howto-create-service-principal-portal#register-an-application-with-microsoft-entra-id-and-create-a-service-principal) - Once finished, make sure to copy the following values from the Overview tab.
        1. Application (client) ID
        2. Directory (tenant) ID
    2. [Set up authentication - Option 3: Create a new client secret](https://learn.microsoft.com/en-us/entra/identity-platform/howto-create-service-principal-portal#option-3-create-a-new-client-secret) - When creating the client secret on the SPN, make sure to copy the secret value, <b>this is only displayed upton creation and cannot be copied again</b>.
2. <b>SPN roles in the target Fabric Workspace</b> - Add your newly created SPN to your workspace with at least the Contributor role.  For more information on Fabric Workspace Roles check out [Roles in workspaces in Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/fundamentals/roles-workspaces).
3. <b>Azure Key Vault</b> - We need to create an Azure Key Vault and add 3 secrets to be utilized by the ADF Rest call to Fabric.  To do so, follow these steps.
    1. Create a Key Vault using the following article: [Quickstart: Create a key vault using the Azure portal](https://learn.microsoft.com/en-us/azure/key-vault/general/quick-create-portal)
    2. Add yourself as a <b>Key Vault Administrator</b> and <b>Key Vault Secrets Officer</b> to the RBAC permissions of the newly created Key Vault using the following article: [Provide access to Key Vault keys, certificates, and secrets with Azure role-based access control](https://learn.microsoft.com/en-us/azure/key-vault/general/rbac-guide?tabs=azure-portal)
    3. Add the following secrets to your Key Vault instance using the following article: [Quickstart: Set and retrieve a secret from Azure Key Vault using the Azure portal](https://learn.microsoft.com/en-us/azure/key-vault/secrets/quick-create-portal)
        1. SPADF-TenantID = Directory (tenant) ID from Step 1 above
        2. SPADF-ClientID = Application (client) ID from Step 1 above
        3. SPADF-Secret = Client secret value from Step 2 above
4. <b>Key Vault Linked Service in Azure Data Factory</b> - Create the Linked Service using the following article: [Store credentials in Azure Key Vault](https://learn.microsoft.com/en-us/azure/data-factory/store-credentials-in-key-vault)<br>NOTE: For step 2 of the article, follow the <b>Access control</b> option. 
5. <b>Enable SPN authentication for Fabric API</b> - Enable SPN access to Fabric via the Admin Portal using the following article: [Enable service principal authentication for admin APIs](https://learn.microsoft.com/en-us/fabric/admin/enable-service-principal-admin-apis)
