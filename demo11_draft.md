
# Demo: Use Python to send and receive messages from a Service Bus queue

In this demo you will learn how to:

* Create a Service Bus namespace using the Azure Portal.
* Create a Python application to create a queue and send a set of messages to the queue.
* Create a Python application to receive those messages from the queue.


## Prerequisites

This demo is performed in the Azure Portal, and in Visual Studio Code. The code examples below rely on the **Azure-ServiceBus** PIP package.


### Login to Azure

1. Login in to the Azure Portal: [https://portal.azure.com](https://portal.azure.com) and launch the Cloud Shell. Be sure to select **Bash** as the shell.

1. Create a resource group, replace `<myRegion>` with a location that makes sense for you. Copy the first line by itself and edit the value.


## Create the Service Bus namespace

1. Create a Service Bus messaging namespace with a unique name, for example _az204svcbus[yourname]_ 
   
   1. In the **Azure Portal** search for _Service Bus_ 
   1. Select the **Service Bus** option
   1. Click **Add**
   1. Select the **Resource Group** you created earlier
   1. Type in the **Namespace name** of _az204svcbus[yourname]_ 
   1. Select the region
   1. Use the **Standard** pricing tier

1. Get the connection string for the namespace

   1. In the **Azure Portal** go to the new resource that was created
   1. Select **Shared Access Policies** under the Settings section in the left-hand navigation
   1. Click the _Root ManageSharedAccessKey_ link
   1. Copy the value of the **Primary Connect String**.  
   1. Paste the connection string to a temporary location such as Notepad. You will need it in the next step.

## Step 1: Write code to create the queue

## Create Python application to send messages to the queue

1. Open Windows Explorer and create a new folder named *az204svcbus*.

1. Open the new folder in **Visual Studio Code**

1. Ensure that Python is installed

1. Ensure that the Python Extension is installed under **extensions** 

1. Open a terminal by using the **Command Palette** under the **View** menu

1. Upgrade the PIP package manager 
    ```
    python -m pip install --upgrade pip
    ```
1. Create a virtual environment for Python
    ```
    python -m venv .venv
    ```
1.  Activate the virtual environment
    ```
    .venv\scripts\activate
    ```
    >Note: You should see a new command prompt that includes the virtual environment

1. Create a new text file called **requirements.txt** and add the following lines
    ```
    azure
    azure-servicebus
    ```
    >Note: we will use this file to install the required libraries
    
1. Create a new Python file called *CreateQueue.py*

1. In *CreateQueue.py*, type the following:

    ```
    from azure.servicebus import ServiceBusClient

    sb_client = ServiceBusClient.from_connection_string('<Connection_string>')

    sb_client.create_queue("taskqueue")
    ```
    >Note: You will need to substitute your vale for <Connection_string> 
    
1. Login to your Azure account
    ```
    az login
    ```
    >Note: You will see a login screen in your browser. Use your Azure credentials to log in.  You should get a confirmation at the command-line
    
1.  Select _CreateQueue.py_ in the left navigation and run the Python program by pressing the green **run** button in the top right

1.  Go to the Azure Portal and confirm that a new Sevice Bus queue has been created called _taskqueue_


## Step 2: Write code to send messages to the queue

1. Login to the Azure Portal and navigate to the *az204-queue* you created earlier and select **Overview** to show the Essentials screen. 

Notice that the Active Message Count value for the queue is now 10. Each time you run the sender application without retrieving the messages (as described in the next section), this value increases by 10. 


## Step 3: Write code to receive messages to the queue

1. Set up the new console app
    * Create a new folder named *az204svcbusRec*.
    * Open a terminal in the new folder and run `dotnet new console`
    * Run the `dotnet add package Microsoft.Azure.ServiceBus` command to ensure you have the packages you need.
    * Launch Visual Studio Code and open the new folder.



1. Check the portal again. Notice that the **Active Message Count** value is now 0. You may need to refresh the portal page.


