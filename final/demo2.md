# Lab: Azure Functions (Python)
# Student Lab Manual

### Lab Scenario

Demonstrate Azure Functions using Python

### Objectives

After you complete this lab, you will be able to:

- Create a Storage Account

- Create an Azure Function 

- Test the Function locally

- Trigger the Function on a schedule

- Integrate the Function with other services

### Lab setup

- 45 minutes

## Before you start

### Sign in to the lab virtual machine

Log on to @lab.VirtualMachine(AZ-204T00A-SEA-DEV).SelectLink by pressing @lab.CtrlAltDelete and typing in the following credentials:
    
**Username**: +++@lab.VirtualMachine(AZ-204T00A-SEA-DEV).Username+++
    
**Password**: +++@lab.VirtualMachine(AZ-204T00A-SEA-DEV).Password+++

>[!TIP] Select the +++Type Text+++ icon to enter the associated text into the virtual machine.

>[!alert] If you are using the +++TypeText+++ feature to copy text to **VS Code**, you must first input the text into **Notepad** and the copy it into **VS Code** in order to maintain formatting.

>[!alert] **Do not** select nested Typetext icons. Doing so will produce the wrong output.
> 
>[!alert] !IMAGE[oftqjqbo.jpg](oftqjqbo.jpg)
---

## Download the lab files

@lab.Activity(DownloadFromGit)

---

!instructions[](https://raw.githubusercontent.com/LODSContent/All-MOC/master/MOC/CloudShellPrep)

===

### Exercise 1: Create Azure resources

#### Task 1: Open the Azure portal

1. [] Sign into the Azure portal (+++https://portal.azure.com+++) as +++@lab.CloudPortalCredential(LabUser).Username+++ using +++@lab.CloudPortalCredential(LabUser).Password+++ as the password.

    >[!NOTE] If this is your first time signing in to the Azure portal, a dialog box will display offering a tour of the portal. Select **Get Started** to skip the tour.

#### Task 2: Create an Azure Storage account

1. [] Create a new storage account with the following details:
    
    -   Resource group: **RG1-@lab.LabInstance.Id**

    -   Name: +++funcstor-@lab.LabInstance.Id+++

    -   Location: **(US) East US**

    -   Performance: **Standard**

    -   Account kind: **StorageV2 (general purpose v2)**

    -   Replication: **Locally-redundant storage (LRS)**

    -   Access tier: **Hot**

    >[!NOTE] Wait for Azure to finish creating the storage account before you move forward with the lab. You'll receive a notification when the account is created.

#### Task 3: Create an Azure Functions app

1. [] Create a new function app with the following details:

    -   Existing resource group: **RG1-@lab.LabInstance.Id**

    -   App name: +++funclogicpy-@lab.LabInstance.Id+++

    -   Publish: **Code**

    -   Runtime stack: **Python**

    -   Version: **3.7**

    -   Region: **East US**

    -   Storage account: **funcstor-@lab.LabInstance.Id**

    -   Operating system: **Linux**

    -   Plan: **Consumption**

    -   Enable Application Insights: **Yes**

    >[!NOTE] Wait for Azure to finish creating the function app before you move forward with the lab. You'll receive a notification when the app is created.

#### Review

In this exercise, you created the Azure resources that you'll use for this lab.

===

### Exercise 2: Create a function that's triggered by an HTTP request using Visual Studio Code

>[!NOTE] The interface for Azure Functions has been updated. (Classic version and current branch). Also, as of this writing, authoring and editing Python Functions is supported only in Visual Studio Code. You can then upload your Functions to Azure and view them from the Azure Portal. 

#### Task 1: Create an HTTP-triggered function using Visual Studio Code and Python

1. [] Open **Visual Studio Code** and install the **Azure Functions** extension.

1. [] In the left navigation, select the **Azure** icon.

1. [] Expand **Functions**.

1. [] Select **Sign in to Azure**.

    >[!NOTE] You should now see the new Function you created earlier.
    
1. [] In Visual Studio Code, select **Create new project** and open **F:\Lab05\starter**.

1. [] Select **Python** as the language.

1. [] Select the Python interpreter.

    >[!NOTE] This should already be installed. For example **Python 3.8.3**.

1. [] Select **HTTP Trigger**.

1. [] Rename the **HttpTrigger1** function to +++**Echo**+++.

1. [] In the **Authorization** box, select **Anonymous**.

1. [] Select **Open in Current Window**.

    >[!NOTE] A new Python function will be created.

#### Task 2: Test the function code locally

1. [] In Visual Studio Code blade, on the menu, select **Debug** > **Start Debugging**.

    >[!NOTE] You may be asked to install **Azure Functions Core Tools**.
    
1. [] With the function is running, return to the **Functions** tool.

1. [] Right-click the function named **Echo** and select **Copy Function URL**.

1. [] Open a browser and paste the function URL.  
For example **localhost:7071/api/Echo**.

1. [] The function will return a string **Please pass a name on the query string or in the request body**.

1. [] Add the following query to the end of the URL:
    ```
    ?name=Azure
    ```
1. [] The local Function will return a response:
    ```
    Hello Azure
    ```
    >[!NOTE] You can substitute any name or string for **Azure**.
    
1. [] Select **Disconnect** to stop the function.

#### Task 3: Deploy and test the function in Azure

1. [] In Visual Studio Code, return to the **Functions** tool under **Azure** and then select the **Echo HTTP** function.

1. [] Select **Deploy to Function app..**.

1. [] Select the Azure Function you created earlier.

    >[!NOTE] You will get a message **Deploying to funclogicpy-@lab.LabInstance.Id**.

1. [] Wait until you get a confirmation message that the deployment is complete. Select **View Output**.

#### Task 4: Get a base function URL

1. [] In the Azure portal's navigation pane, select the **Resource groups** link.

1. [] On the **Resource groups** blade, find and then select the **RG1-@lab.LabInstance.Id** resource group that you created earlier in this lab.

1. [] On the **RG1-@lab.LabInstance.Id** blade, select the **funclogicpy-@lab.LabInstance.Id** function app that you created earlier in this lab.

1. [] In the left navigation of the Azure Portal select **Functions**.

1. [] Select the **Echo** function.

1. [] Select **Get Function URL** and then select **Copy to clipboard**.

    >[!NOTE] You can also inspect the code for the Function using the left navigation in the Azure Portal.  This should reflect the code you uloaded from **Visual Studio Code**.

#### Task 5: Test function run by using a browser and curl

1. [] Open a new browser tab and paste the URL for the function then press Enter.  
Notice the response from the function.

1. [] Add the following query to the end of the URL:
    ```
    ?name=Azure
    ```
1. [] The Azure Function will return a response:
    ```
    Hello Azure
    ```
1. [] Open a terminal using the icon on the taskbar of the lab machine.

1. [] Enter the following command to ensure that **curl** is installed.
    ```
    curl --version
    ```
1. [] Copy the URL for the Azure Function from the Azure Portal as you did in **Task 4**.

1. [] In the terminal window enter the following command:
    ```
    curl <URL>
    ```
    **Note:** Paste the value for the URL you copied earlier in <URL>.  You can do this by right-clicking after the **curl** command and then selecting **Paste**.

1. [] You will get a similar response compared to the local test.

1. [] In the terminal window type the following command, by appending the query after the URL:
    ```
    curl <URL>?name=Azure
    ```
1. [] You should now get a response from the Azure function:
    ```
    Hello Azure
    ```
    
    >[!NOTE] You can observe the execution of the Azure function in the Azure portal.  
    >In the Overview blade for the funtion you will see **Total Execution count**.

#### Review

In this exercise, you created a basic function that echoes the content sent via an HTTP POST request.

===

### Exercise 3: Create a function that triggers on a schedule

#### Task 1: Create a schedule-triggered function

1. [] Return to **Visual Studio Code**.

1. [] In the left navigation, select the Azure icon.

1. [] Expand **Functions** > **Local Project** and then select **Functions**.

1. [] Select **Create Functions..**.

1. [] Select **Timer trigger**.

1. [] Rename the function **TimerTrigger1** to +++**Recurring**+++.

1. [] Leave the default value of the **cron** expression and press Enter.

1. [] In the **Schedule** text box, change the value to **\*/30 \* \* \* \* \***

1. [] You should now see sample Python code for a scheduled function.

1. [] In the left-hand navigation of Visual Studio Code, select the new function.

1. [] Select **Deploy to Function app..**.

1. [] Select the Azure Function you created earlier.

    >[!NOTE] You will get a message **Deploying to AzureFunction[yourname]**.

1. [] Wait until you get a confirmation message that the deployment is complete.


#### Task 2: Observe function runs

1. [] Return to the **Azure Portal** and return to the function funclogicpy-@lab.LabInstance.Id.

1. [] Select **Functions** in the left-hand navigation.

1. [] Select **Recurring** then select **Code and Test**.

    >[!NOTE] editing is not available in the Azure Portal. However, you can run the Function and observe the logs.

1. [] In the function editor, select **Logs**.

1. [] Observe the function run that occurs about every 30 seconds. Each function run should render a simple message to the log.

    >[!NOTE] You can also observe the Successful runs of the Function by selecting **Monitor** in the left-hand navigation. 

#### Review

In this exercise, you created a function that runs automatically based on a fixed schedule.

===

### Exercise 4: Create a function that integrates with other services

#### Task 1: Create an Azure Blob Storage Trigger function

1. [] In Visual Studio Code, under **Local Project**, select **Functions**.

1. [] Select **Create Functions..**.

1. [] Select **Azure Blob Storage Trigger**.

1. [] Rename the Function from the default of _BlobTrigger1_ to **GetSettingInfo**.

1. [] For the Blob Storage trigger, select **AzureWebJobsStorage**.

1. [] Accept the default location _samples-workitems/{name}_ and press Enter.

    >[!NOTE] You should now see the sample Python code for the function.

#### Task 2: Upload the function to Azure

1. [] In the left navigation of Visual Studio Code, select **GetSettingInfo**.

1. [] Select **Deploy to Function App..**.

1. [] Select the function **funclogicpy-@lab.LabInstance.Id**.

1. [] Your local function should now deploy to Azure.

#### Task 4: Observe the running function code

1. [] Return to the Azure Portal.

1. [] Select **funclogicpy-@lab.LabInstance.Id** and observe the Metrics under the **Overview** section.

1. [] Select **Functions**.

1. [] Select the **GetSettingInfo** function.

1. [] Under the Developer section, select **Monitor**.  
We will return to the **Invocations** tab and the **Logs** tab later in the exercise.

#### Task 3: Upload sample content

1. [] In the Azure portal's navigation pane, select the **Resource groups** link.

1. [] In the **Resource groups** blade, select **RG1-@lab.LabInstance.Id** resource group that you created earlier in this lab.

1. [] In the **RG1-@lab.LabInstance.Id** blade, select the **funcstor-@lab.LabInstance.Id** storage account that you created earlier in this lab.

1. [] In the **Storage account** blade, in the **Blob service** section, select **Containers**.

1. [] In the **Containers** section, select **+ Container**.

1. [] In the **New container** pop-up window, perform the following actions:
    
    1. In the **Name** text box, enter **samples-workitems**.
    
    1. In the **Public access level** drop-down list, select **Private (no anonymous access)**.
    
    1. Select **OK**.

1. [] Back in the **Containers** section, select the recently created **samples-workitems** container.

1.  In the **Container** blade, select **Upload**.

1.  In the **Upload blob** window, perform the following actions:

    1. In the **Files** section, select the **Folder** icon.

    1. In the **File Explorer** window, browse to **Allfiles (F):\\Allfiles\\Labs\\02\\Starter**, select the **settings.json** file, and then select **Open**.

    1. Ensure that the **Overwrite if files already exist** check box is selected, and then select **Upload**. 
    
    >[!NOTE] Wait for the blob to upload before you continue with this lab.

#### Task 5: Test function run

1. [] Return to the Azure Portal.  
You should be in the **Monitor** blade.

1. [] Observe the **Success Count** under the **Invocations** tab.  You should see a count of 1. 

    >[!NOTE] This may take a few minutes to appear.
    
1. [] Under **Date(UTC)** next to the success event, select the timestamp.  
You should see details of the blob file upload event.

1. [] Select the **Logs** tab and then select **Open in Live Metrics**.  
You should see incoming requests and a trace record polling for blob events.

#### Review

In this exercise, you created a function that integrates with Blob Storage.

---
### Congratulations!
You have successfully completed this exercise. 



