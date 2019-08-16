# Build and deploy the Azure DevOps Pipeline Agent on AKS

### A] Create PAT token and Agent Pool in Azure DevOps Services
1. Login to Azure DevOps Services portal with your credentials

2. Create a *PAT* token
   Click on your profile name (top right corner) and then click on **Security**.  Click on **+ New Token**. See screenshot below.

   ![alt tag](./images/A-01.PNG)

   In the *Create a personal access token* window, specify a name for the PAT Token, set the **Expiration** field to **Custom defined** and select an expiry date.  Set the **Scopes** field to **Full access**.  Then click on **Create**.  See screenshot below.

   ![alt tag](./images/A-02.PNG)

   Copy and save the PAT token.  The token won't be accessible once the window is closed.

   ![alt tag](./images/A-03.PNG)

2. Create an *Agent Pool*
   Open up the Azure DevOps organization tab and then click on **Organization settings** (lower left corner).  See screenshot below.   

   ![alt tag](./images/A-04.PNG)

   Then click on **Agent pools* as shown in the screenshot below.

   ![alt tag](./images/A-05.PNG)

   Click on **Add pool**, specify a name for the agent pool and check the box for **Grant access permission to all pipelines**.  Click on **Create**.  See screenshots below.

   ![alt tag](./images/A-06.PNG)

   ![alt tag](./images/A-07.PNG)

