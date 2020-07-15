
# Lab: Implement task processing logic by using Azure Functions
# Student lab answer key

## Microsoft Azure user interface

Given the dynamic nature of Microsoft cloud tools, you might experience Azure UI changes after the development of this training content. These changes might cause the lab instructions and lab steps to not match up.

Microsoft updates this training course when the community brings needed changes to our attention; however, because cloud updates occur frequently, you might encounter UI changes before this training content updates. **If this occurs, adapt to the changes, and then work through them in the labs as needed.**

## Instructions

### Before you start

#### Sign in to the lab virtual machine

Sign in to your Windows 10 virtual machine (VM) by using the following credentials:
    
-   Username: **Admin**

-   Password: **Pa55w.rd**

> **Note**: Instructions to connect to the virtual lab environment will be provided by your instructor.

#### Review the installed applications

Find the taskbar on your Windows 10 desktop. The taskbar contains the icons for the applications that you'll use in this lab:
    
-   Microsoft Edge

-   File Explorer

-   Visual Studio Code

### Exercise 1: Create Azure resources

#### Task 1: Open the Azure portal

1.  On the taskbar, select the **Microsoft Edge** icon.

1.  In the open browser window, go to the Azure portal (<https://portal.azure.com>).

1.  Enter the email address for your Microsoft account, and then select **Next**.

1.  Enter the password for your Microsoft account, and then select **Sign in**.

    > **Note**: If this is your first time signing in to the Azure portal, you'll be offered a tour of the portal. Select **Get Started** to skip the tour and begin using the portal.

#### Task 2: Create an Azure Storage account

1.  In the Azure portal's navigation pane, select **All services**.

1.  On the **All services** blade, select **Storage Accounts**.

1.  On the **Storage accounts** blade, get your list of storage account instances.

1.  On the **Storage accounts** blade, select **Add**.

1.  On the **Create storage account** blade, observe the tabs on the blade, such as **Basics**, **Tags**, and **Review + Create**.

    > **Note**: Each tab represents a step in the workflow to create a new storage account. You can select **Review + Create** at any time to skip the remaining tabs.

1.  Select the **Basics** tab, and then in the tab area, perform the following actions:
    
    1.  Leave the **Subscription** text box set to its default value.
    
    1.  In the **Resource group** section, select **Create new**, enter **Serverless**, and then select **OK**.
    
    1.  In the **Storage account name** text box, enter _funcstor[yourname]_.
    
    1.  In the **Location** list, select the **(US) East US** region.
    
    1.  In the **Performance** section, select **Standard**.
    
    1.  In the **Account kind** list, select **StorageV2 (general purpose v2)**.
    
    1.  In the **Replication** list, select **Locally-redundant storage (LRS)**.
    
    1.  In the **Access tier (default)** section, ensure that **Hot** is selected.
    
    1.  Select **Review + Create**.

1.  On the **Review + Create** tab, review the options that you specified in the previous steps.

1.  Select **Create** to create the storage account by using your specified configuration.

    > **Note**: On the **Deployment** blade, wait for the creation task to complete before moving forward with this lab.

#### Task 3: Create an Azure Functions app

1.  In the Azure portal's navigation pane, select the **Create a resource** link.

1.  From the **New** blade, find the **Search the Marketplace** text box.

1.  In the search box, enter **Function**, and then select Enter.

1.  On the **Everything** search results blade, select the **Function App** result.

1.  On the **Function App** blade, select **Create**.

1.  Find the tabs on the **Function App** blade, such as **Basics**.

    > **Note**: Each tab represents a step in the workflow to create a new function app. You can select **Review + Create** at any time to skip the remaining tabs.

1.  On the **Basics** tab, perform the following actions:
    
    1.  Leave the **Subscription** text box set to its default value.
    
    1.  In the **Resource group** section, select **Use existing**, and then select **Serverless** in the list.
    
    1.  In the **Function app name** text box, enter _funclogic[yourname]_.

    1.  In the **Publish** section, select **Code**.

    1.  In the **Runtime stack** drop-down list, select **Python**.

    1.  In the **Version** drop-down list, select **3.7**.

    1.  In the **Region** drop-down list, select the **East US** region.
    
    1.  Select **Next: Hosting**.

1.  On the **Hosting** tab, perform the following actions:

    1.  In the **Storage account** drop-down list, select the _funcstor_[yourname]_ storage account that you created earlier in this lab.

    1.  In the **Operating System** section, select **Linux**. (notice that the **Windows** option is not available)

    1.  In the **Plan type** drop-down list, select the **Consumption** option.

    1.  Select **Review + Create**.

1.  On the **Review + Create** tab, review the options that you selected during the previous steps.

1.  Select **Create** to create the function app by using your specified configuration. 

    > **Note**: Wait for the creation task to complete before you move forward with this lab.
    
    > **Hint:** The Azure Function will use the Storage Account for _AzureWebJobStorage_.  You can see this setting under **Application Settings** for your empty Azure Function

#### Review

In this exercise, you created the Azure resources that you'll use for this lab.

### Exercise 2: Create a function that's triggered by an HTTP request using Visual Studio Code

> **Note**: The interface for Azure Functions has been updated. (Classic version and current branch). Also, as of this writing, authoring and editing Python Functions are supported only in Visual Studio Code. You can then upload your Functions to Azure and view them from the Azure Portal. You may use this link as a guide (https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-python)

![Functions extension](https://github.com/networksetcetera/LODS/blob/master/images/lab2/Screen%20Shot%202020-06-30%20at%2010.37.02%20AM.png)

#### Task 1: Create an HTTP-triggered function using Visual Studio Code and Python

1.  Open **Visual Studio Code** and install the _Azure Functions_ extension
    ![extension installed](https://github.com/networksetcetera/LODS/blob/master/images/lab2/Screen%20Shot%202020-06-30%20at%2010.29.49%20AM.png)

1.  Select the Azure Icon in the left navigation 

1.  Expand **Functions**

1.  Select **Sign in to Azure**

    >**Note:** You should now see the new Function you created earlier
    
1.  In **Visual Studio Code** click **Create new project** and open the folder on the F: drive for Lab05/starter.

1.  Select **Python** as the language

1.  Select the Python interpreter

    >**Note:** This should already be installed.  For example _Python 3.8.3_

1.  Select **HTTP Trigger**.

1.  Rename the function from _HttpTrigger1_ to **Echo** and press **enter**.

1.  Within the **Authorization** text box select **Anonymous**.

1.  You will see 3 options. Select **Open in Current Window**.

    >**Note:** A new Python function will be created

#### Task 2: Test the function code locally

1.  In **Visual Studio Code** blade, select **Start Debugging** from the **Debug** menu.

    >**Note:** You may be asked to install _Azure Functions Core Tools_
    
1.  Now that the Function is running, return to the _Functions_ tool in **Visual Studio Code**

1.  Right-click on the Function name **Echo** and select **Copy Function URL** 

1.  Open a browser and paste the Function URL, for example _localhost:7071/api/Echo_

1.  The Function will return a string _Please pass a name on the query string or in the request body_

1.  Add the following query to the end of the URL
    ```
    ?name=Azure
    ```
1.  The local Function will return a response
    ```
    Hello Azure
    ```
    >**Note:** You can substitute any name or string for _Azure_
    
1.  Click **Disconnect** to stop the Function


#### Task 3: Deploy the Function to Azure and test function running in the portal

1.  In **Visual Studio Code** return to the **Functions** tool under **Azure** and select the _Echo HTTP_ Function

1.  Click **Deploy to Function app..**

1.  Select the Azure Function you created earlier

    >**Note:** You will get a message _Deploying to FuncLogic[yourname]_

1.  Wait until you get a confirmation message that the deployment is complete. Click **View Output**



#### Task 4: Get a base function URL

1.  In the Azure portal's navigation pane, select the **Resource groups** link.

1.  On the **Resource groups** blade, find and then select the **Serverless** resource group that you created earlier in this lab.

1.  On the **Serverless** blade, select the **funclogic[yourname]** function app that you created earlier in this lab.

1.  In the left navigation of the Azure Portal select **Functions**

1.  Click on the function called **Echo**

1.  Click on **Get Function URL** and select **Copy to clipboard**

    >Note: You can also inspect the code for the Function using the left navigation in the Azure Portal.  This should reflect the code you uloaded from **Visual Studio Code**



#### Task 5: Test function run by using a browser and curl

1.  Open a new browser tab and paste the URL for the Function then hit **enter**

1.  Notice the response from the function

1.  Add the following query to the end of the URL
    ```
    ?name=Azure
    ```
1.  The Azure Function will return a response
    ```
    Hello Azure
    ```
1.  Open a terminal using the icon on the taskbar of the lab machine

1.  Type the following command to ensure that **curl** is installed
    ```
    curl --version
    ```
1.  Copy the URL for the Azure Function from the Azure Portal as you did in **Task 4**

1.  In the terminal window type the following command
    ```
    curl <URL>
    ```
    **Note:** Paste the value for the URL you copied earlier in <URL>.  You can do this by right-clicking after the **curl** command and selecting **Paste**

1.  You will get a similar response compared to the local test

1.  In the terminal window type the following command, by appending the query after the URL
    ```
    curl <URL>?name=Azure
    ```
1.  You should now get a response from the Azure Function
    ```
    Hello Azure
    ```
    
    >**Note:** You can observe the execution of the Azure Function in the Azure Portal.  In the Overview blade for the funtion you will see **Total Execution count**

#### Review

In this exercise, you created a basic function that echoes the content sent via an HTTP POST request.


### Exercise 3: Create a function that triggers on a schedule

#### Task 1: Create a schedule-triggered function

1.  Return to **Visual Studio Code**.

1.  Select the Azure Icon in the left navigation 

1.  Expand **Functions**
    
1.  Expand **Local Project**.

1.  Click on **Functions**

1.  Click the **Create Functions..** icon

1.  Select **Timer trigger**

1.  Rename the function from _TimerTrigger1_ to **Recurring** and press **enter**.

1.  Leave the default value of the _cron_ expression and press **enter**

1.  In the **Schedule** text box, change the value to **\*/30 \* \* \* \* \***.

1.  You should now see some sample Python code for a scheduled Function

1.  Select the new function in the left-hand navigation of **Visual Studio Code**

1.  Click **Deploy to Function app..**

1.  Select the Azure Function you created earlier

    >**Note:** You will get a message _Deploying to AzureFunction[yourname]_

1.  Wait until you get a confirmation message that the deployment is complete.



#### Task 2: Observe function runs

1.  Return to the **Azure Portal** and return to the Function _Funclogic[yourname]_

1.  Select **Functions** in the left-hand navigation.

1.  Click on **Recurring** then click **Code and Test**

    >**Note:** editing is not available in the Azure Portal. However, you can run the Function and observe the logs

1.  In the function editor, select **Logs**.

1.  Observe the function run that occurs about every 30 seconds. Each function run should render a simple message to the log.

    >**Note:** You can also observe the Successful runs of the Function by selecting **Monitor** in the left-hand navigation 

#### Review

In this exercise, you created a function that runs automatically based on a fixed schedule.


### Exercise 4: Create a function that integrates with other services

#### Task 1: Create an Azure Blob Storage Trigger function

1.  In **Visual Studio Code** click on **Functions** under **Local Project**

1.  Click the **Create Functions..** icon

1.  Select **Azure Blob Storage Trigger**

1.  Rename the Function from the default of _BlobTrigger1_ to **GetSettingInfo**.

1.  Select **AzureWebJobsStorage** for the Blob Storage trigger

1.  Accept the default location _samples-workitems/{name}_ and press **Enter**

    >**Note:** You should now see the sample Python code for the Function


#### Task 2: Upload the Function to Azure

1.  Select **GetSettingInfo** in the left-hand navigation of **Visual Studio Code**

1.  Click **Deploy to Function App..**

1.  Select the Function you created earlier called _FuncLogic[yourname]_

1.  Your local Function should now deploy to Azure

#### Task 4: Observe the running Function code

1.  Return to the Azure Portal 

1.  Select _funclogic[yourname]_ and observe the Metrics under the **Overview** section

1.  Click on **Functions**

1.  Select the **GetSettingInfo** Function

1.  Under the Developer section, select **Monitor**.  We will return to the **Invocations** tab and the **Logs** tab later in the exercise

#### Task 3: Upload sample content

1.  In the Azure portal's navigation pane, select the **Resource groups** link.

1.  On the **Resource groups** blade, find and then select the **Serverless** resource group that you created earlier in this lab.

1.  On the **Serverless** blade, select the **funcstor*[yourname]*** storage account that you created earlier in this lab.

1.  On the **Storage account** blade, select the **Containers** link in the **Blob service** section.

1.  In the **Containers** section, select **+ Container**.

1.  In the **New container** pop-up window, perform the following actions:
    
    1.  In the **Name** text box, enter **samples-workitems**.
    
    1.  In the **Public access level** drop-down list, select **Private (no anonymous access)**.
    
    1.  Select **OK**.

1.  Back in the **Containers** section, select the recently created **samples-workitems** container.

1.	On the **Container** blade, select **Upload**.

1.	In the **Upload blob** window, perform the following actions:

    1.  In the **Files** section, select the **Folder** icon.

    1.  In the **File Explorer** window, browse to **Allfiles (F):\\Allfiles\\Labs\\02\\Starter**, select the **settings.json** file, and then select **Open**.

    1.  Ensure that the **Overwrite if files already exist** check box is selected, and then select **Upload**. 
    
    > **Note**: Wait for the blob to upload before you continue with this lab.


#### Task 5: Test function run

1.  Return to the Azure Portal.  You should be in the **Monitor** blade 

1.  Observe the **Success Count** under the **Invocations** tab.  You should see a count of 1.  

    >**Note:** This may take a few minutes to appear
    
1.  Click on the timestamp under **Date(UTC)** next to the success event.  You should see details of the blob file upload event

1.  Select the **Logs** tab and click **Open in Live Metrics**.  You should see incoming requests and a trace record polling for blob events


#### Review

In this exercise, you created a function that integrates with Blob Storage.


### Exercise 5: Clean up your subscription 

#### Task 1: Open Azure Cloud Shell and list resource groups

1.  In the Azure portal's navigation pane, select the **Cloud Shell** icon to open a new shell instance.

    > **Note**: The **Cloud Shell** icon is represented by a greater than sign (\>) and underscore character (\_).

1.  If this is your first time opening Cloud Shell using your subscription, you can use the **Welcome to Azure Cloud Shell Wizard** to configure Cloud Shell for first-time usage. Perform the following actions in the wizard:
    
    1.  A dialog box prompts you to create a new storage account to begin using the shell. Accept the default settings, and then select **Create storage**. 

    > **Note**: Wait for Cloud Shell to finish its initial setup procedures before moving forward with the lab. If you don't notice Cloud Shell configuration options, this is most likely because you're using an existing subscription with this course's labs. The labs are written with the presumption that you're using a new subscription.

#### Task 2: Delete a resource group

1.  At the command prompt, enter the following command, and then select Enter to delete the **Serverless** resource group:

    ```
    az group delete --name Serverless --no-wait --yes
    ```
    
1.  Close the Cloud Shell pane in the portal.

#### Task 3: Close the active application

1.     the currently running Microsoft Edge application.

#### Review

In this exercise, you cleaned up your subscription by removing the resource group that was used in this lab.
