
# Lab: Asynchronously processing messages by using Azure Storage Queues
# Student lab manual

## Lab scenario

You're studying various ways to communicate between isolated service components in Microsoft Azure, and you have decided to evaluate the Azure Storage service and its Queue service offering. As part of this evaluation, you'll build a prototype application in Python that can send and receive messages so that you can measure the complexity involved in using this service. To help you with your evaluation, you've also decided to use Azure Storage Explorer as the queue message producer/consumer throughout your tests.

## Objectives

After you complete this lab, you will be able to:
 
-   Add **Azure.Storage** libraries.
 
-   Create a queue.
 
-   Produce a new message in the queue by using Python.
 
-   Consume a message from the queue by using Python.
 
-   Manage a queue by using Storage Explorer.

## Lab setup

-   Estimated time: **45 minutes**

## Instructions

### Before you start

#### Sign in to the lab virtual machine

Ensure that you're signed in to your Windows 10 virtual machine by using the following credentials:
    
-   Username: **Admin**

-   Password: **Pa55w.rd**

#### Review the installed applications

Find the taskbar on your Windows 10 desktop. The taskbar contains the icons for the applications that you'll use in this lab:

-   Microsoft Edge

-   Visual Studio Code

-   Storage Explorer

### Exercise 1: Create Azure resources

#### Task 1: Open the Azure portal

1.  Sign in to the Azure portal (<https://portal.azure.com>).

1.  If this is your first time signing in to the Azure portal, you'll notice a dialog box offering a tour of the portal. Select **Get Started** to skip the tour.

#### Task 2: Create a Storage account

1.  Create a new storage account with the following details:
    
    -   New resource group: **AsyncProcessor**

    -   Name: **asyncstor[yourname]**

    -   Location: **(US) East US**

    -   Performance: **Standard**

    -   Account kind: **StorageV2 (general purpose v2)**

    -   Replication: **Locally-redundant storage (LRS)**

    -   Access tier: **Hot**

    > **Note**: Wait for Azure to finish creating the storage account before you move forward with the lab. You'll receive a notification when the account is created.

1.  Find the **Access Keys** blade of your newly created storage account instance.

1.  Record the value of the **Connection string** text box. You'll use this value later in this lab.


#### Review

In this exercise, you created a new Storage Account that you'll use through the remainder of the lab.

### Exercise 2: Configure the Azure Storage libraries in a Python project 

#### Task 1: Create a Python project

>Note: You can use this link for guidance (https://code.visualstudio.com/docs/python/python-tutorial)

1.  Install the Python extention in **Visual Studio Code**.

1.  Using **Visual Studio Code**, open the **Allfiles (F):\\Allfiles\\Labs\\11\\Starter\\MessageProcessor** folder.

1.  Open a Terminal in **Visual Studio Code**. Create a Virtual Environment for Python by running this command:
    ```
    python -m venv .venv
    ```
1.  Activate the virtual environment for Python by runnng this command:

    ```
    .venv\scripts\activate
    ```

1.  Using **Visual Studio Code**, create a new text file named **requirements.txt** in the current folder. 
    Add the following lines to the text file
    
    ```text
    
    azure-storage-queue

    ```
1.  Install the required Azure libraries for Python by running this command:
    ```
    pip install -r requirements.txt
    ```
    >**Note:** You may need to update the version of PIP
    
1.  Sign into your Azure account using a terminal in **Visual Studio Code**.

1.  Choose your Python interpreter in **Visual Studio Code** by selecting the **command palette** under the _view_ menu. Type 
    ```
    Python: Select Interpreter
    ```
    >Note: In the dropdown menu select the virtual environment **.venv** that you created earlier
    
1.  Using **Visual Studio Code**, create a new Python file named **QueueCreate.py** in the current folder.

    
#### Task 2: Write code to access Azure Storage

1.  Add the following **from** directives for libraries that will be referenced by the application:
    ```
    from azure.storage.queue import QueueClient
    ```

1.  Create a new **Queue Client** called **queue** with two constant string properties named **connection_string** and **messagequeue**, and then create an asynchronous **Main** entry point method:

    ```
    queue = QueueClient.from_connection_string(conn_str="<connection_string>", queue_name="messagequeue")
    queue.create_queue()
    ```

1.  Update the **<connection_string>** string constant by setting its value to the **Connection string** of the storage account that you recorded earlier in this lab.

1.  Run the Python script by clicking the green **run** button with the caption _Run python File in Terminal_

#### Task 3: Validate Azure Storage access

1.  Open **Storage Explorer** from the icon on the Windows Taskbar.

1.  Within **Storage Explorer** Sign in to your Azure account.

1.  Expand your Azure subscription 

1.  Expand **Storage Accounts** 

1.  Expand the **asyncstor[yourname]** Storage account Storage Account
 
1.  Expand Queues

1.  You should see a new Queue called "messagequeue"

#### Review

In this exercise, you configured your Python project to access the Storage service and manipulate a queue made available through the service.

### Exercise 3: Add messages to the queue 

#### Task 1: Use **Storage Explorer** to add test messages

1.  Using the **Azure Portal** find the **asyncstor[yourname]** Storage account that you created earlier in this lab.

1.  In the **Overview** section of the blade, select **Open in Explorer** to open the Storage account by using Storage Explorer.

    >**Note:** You can also open **Storage Explorer** using the icon in the windows taskbar

1.  In the **Azure Storage Explorer** application, sign in to your Azure account.

1.  From the **Azure Storage Explorer** application, in the **EXPLORER** pane, find and expand the **asyncstor[yourname]** storage account that you created earlier in this lab.

1.  Within the **asyncstor*[yourname]*** node for the Storage account that you created earlier in this lab, find and open the **messagequeue** queue.

1.  Add a new message to the queue with the following properties:

    -   Message text: **Hello World**

    -   Expires in: **12 Hours**

    -   Encode message body in Base 64: **No**


#### Task 2: Write code to access queue messages

1.  Using **Visual Studio Code** create a new Python file called** MessageReader.py** in the current folder

    1.  Add the following block of code to create the Queue Client:

        ```
        from azure.storage.queue import QueueClient

        queue = QueueClient.from_connection_string(conn_str="<connection_string>", queue_name="messagequeue")
        ```

    1.  Render a header by using the **print** command:

        ```
        print("---Existing Messages---");
        ```
    
    1.  Add the following block of code to retrieve a batch of messages asynchronously from the queue service and iterate over the messages:
        ```
        messages = queue.receive_messages()
        for msg in messages:
            print(msg.content)
        ```

1.  Save the **MessageReader.py** file.

1.  Run the Python script by clicking the green **run** button with the caption _Run python File in Terminal_

    >**Note:** The output of the Python script should echo the message text that you added the the queue earlier.

#### Task 3: Delete queued messages


1.  Using **Visual Studio Code** create a new Python file called** MessageDelete.py** in the current folder

    1.  Add the following block of code to create the Queue Client:

        ```
        from azure.storage.queue import QueueClient

        queue = QueueClient.from_connection_string(conn_str="<connection_string>", queue_name="messagequeue")
        ```

    1.  Render a header by using the **print** command:

        ```
        print("---Deleting Messages---");
        ```
    
    1.  Add the following block of code to retrieve a batch of messages asynchronously from the queue service and iterate over the messages:
        ```
        queue.clear_messages()

        ```

1.  Save the **MessageDelete.py** file.

1.  Run the Python script by clicking the green **run** button with the caption _Run python File in Terminal_

1.  In the **Azure Storage Explorer** window, within the **asyncstor[yourname]** node for the Storage account that you created earlier in this lab, find and open the **messagequeue** queue.

1.  Observe the empty list of messages in the queue.

    > **Note**: You might need to refresh the queue.

#### Review

In this exercise, you read and deleted existing messages from the Storage queue by using the Azure library for Storage Queues in Python.


### Exercise 4: Queue new messages by using .NET

1.  Using **Visual Studio Code** create a new Python file called **MessageAdd.py** in the current folder

    1.  Add the following block of code to create the Queue Client:

        ```
        from azure.storage.queue import QueueClient

        queue = QueueClient.from_connection_string(conn_str="<connection_string>", queue_name="messagequeue")
        ```

    1.  Render a header by using the **print** command:

        ```
        print("---Adding Messages---");
        ```
    
    1.  Add the following block of code to retrieve a batch of messages asynchronously from the queue service and iterate over the messages:
        ```
        queue.send_message("Hi Developer!")
        
        ```

1.  Save the **MessageAdd.py** file.

1.  Run the Python script by clicking the green **run** button with the caption _Run python File in Terminal_


#### Task 2: View queued messages by using Storage Explorer

1.  In the **Azure Storage Explorer** window, within the **asyncstor[yourname]** node for the Storage account that you created earlier in this lab, find and open the **messagequeue** queue.

1.  Observe the single new message in the list of messages in the queue.

    > **Note**: You might need to refresh the queue.

#### Review

In this exercise, you created new messages in the queue by using the .NET library for Storage queues.

### Exercise 5: Clean up your subscription 

#### Task 1: Open Azure Cloud Shell and list resource groups

1.  In the portal, select the **Cloud Shell** icon to open a new shell instance.

1.  If Cloud Shell isn't already configured, configure the shell for Bash by using the default settings.

1.  At the **Cloud Shell** command prompt in the portal, enter the following command, and then select Enter to list all resource groups in the subscription:

    ```
    az group list
    ```

1.  Enter the following command, and then select Enter to get a list of possible commands to delete a resource group:

    ```
    az group delete --help
    ```

#### Task 2: Delete a resource group

1.  Enter the following command, and then select Enter to delete the **AsyncProcessor** resource group:

    ```
    az group delete --name AsyncProcessor --no-wait --yes
    ```
    
1.  Close the Cloud Shell pane in the portal.

#### Task 3: Close the active application

1.  Close the currently running Microsoft Edge application.

1.  Close the currently running Visual Studio Code application.

1.  Close the currently running Azure Storage Explorer application.

#### Review

In this exercise, you cleaned up your subscription by removing the resource group that was used in this lab.
