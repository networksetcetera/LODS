# Demo: Use Python to send and receive messages from a Service Bus queue

In this demo you will learn how to:

* Create a Service Bus namespace using the Azure Portal.
* Create a Python application to create a queue and send a set of messages to the queue.
* Create a Python application to receive those messages from the queue.

## Prerequisites

This demo is performed in the Azure Portal, and in Visual Studio Code. The code examples below rely on the **Azure-ServiceBus** PIP package.

>**Note:** In Python, the spacing is important for the correct interpretation of the code. Be careful to keep the indentations shown in the example code when copying and pasting to the lab machine.

### Login to Azure

1.  Login in to the Azure Portal: [https://portal.azure.com](https://portal.azure.com) and launch the Cloud Shell. Be sure to select **Bash** as the shell.

1.  Use the Resource Group that has been assigned and created by Cloud Slice 

## Create the Service Bus namespace

1.  Create a Service Bus messaging namespace. 
   
   1. In the **Azure Portal** search for +++Service Bus+++
   1. Select **Service Bus**
   1. Select **Add**
   1. Select the **RG1-@lab.LabInstance.Id** resouce group
   1. In the **Namespace name** textbox, enter +++az204svcbus@lab.LabInstance.Id+++ 
   1. Select the region
   1. Use the **Standard** pricing tier

1.  Get the Connection String for the namespace.

   1. In the **Azure Portal** go to the new resource that was created
   1. Select **Shared Access Policies** under the Settings section in the left-hand navigation
   1. Select the **Root ManageSharedAccessKey** link
   1. Copy the value of the **Primary Connection String**
   1. Paste the Connection String to a temporary location such as Notepad. You will need it in the next step.

===

## Step 1: Write code to create the queue

## Create Python application to send messages to the queue

1.  Open Windows Explorer and create a new folder named +++az204svcbus+++.

1.  Open the new folder in **Visual Studio Code**

1.  Ensure that Python is installed. Open a terminal and enter:

    ```
    python --version
    ```

1.  Ensure that the Python Extension is installed under **extensions**.

1.  In the **View** menu, open a terminal using the **Command Palette**.

1.  Upgrade the PIP package manager (optional).
 
    ```
    python -m pip install --upgrade pip
    ```
1.  Create a virtual environment for Python.
 
    ```
    python -m venv .venv
    ```
1.  Activate the virtual environment.

    ```
    .venv\scripts\activate
    ```
    >[!NOTE] You should see a new command prompt that includes the virtual environment.

1.  Create a new text file called **requirements.txt** and add the following lines.
 
    ```
    azure
    azure-servicebus
    ```
    >[!NOTE] we will use this file to install the required libraries.
    
1.  Create a new Python file called *CreateQueue.py*

1.  In **CreateQueue.py**, enter the following:

    ```
    from azure.servicebus import ServiceBusClient

    sb_client = ServiceBusClient.from_connection_string('<Connection_string>')

    sb_client.create_queue("taskqueue")
    ```
    >[!NOTE] You will need to substitute your value for the **Primary Connection_string** that you saved earlier. 
    
1.  Login to your Azure account.
    ```
    az login
    ```
    >[!NOTE] You will see a login screen in your browser. Use your Azure credentials to log in.  You should get a confirmation at the command-line.

1.  Install the required libraries for Azure. Enter the following command and the press Enter:
 
    ```
    pip install -r requirements.txt
    ```

    >[!NOTE] If you get an error **No module named azure** then you can manually install the modules using these commands

    ```
    pip install azure
    pip install azure-servicebus
    
1.  In the left navigation, select **CreateQueue.py** and run the Python program by pressing the green **run** button in the top right.

1.  Switch to the Azure Portal and confirm that a new Service Bus queue has been created name **taskqueue**.

===

## Step 2: Write code to send messages to the queue

1.  Create a new Python file named +++SendMessages.py+++

1.  In SendMessages.py, enter the following:

    ```
    from azure.servicebus import QueueClient, Message

    # Create the QueueClient
    queue_client = QueueClient.from_connection_string('<Connection_string>',"taskqueue")

    # Send a test message to the queue
    msg = Message(b'Test Message')
    queue_client.send(msg)

    print("Messages sent")

    ```
    >[!NOTE] You will need to substitute your value for the **Primary Connection_string** that you saved earlier.
    
1.  In the left navigation, select **SendMessages.py** and run the Python program by pressing the green **run** button in the top right.

1.  Login to the Azure portal and browse to the **az204-queue** you created earlier and then select **Overview** to show the Essentials screen. 

Notice that the Active Message Count value for the queue is now 1. 

1.  Find the line of code with the message value.

    ```
    msg = Message(b'Test Message')
    ```
1.  Change the message by replacing the line above with this code.

    ```
    msg = Message(b'Test Message 2')
    ```
1.  In the left navigation, select **SendMessages.py** and re-run the Python program by pressing the green **run** button in the top right.

1.  Refresh the Azure Portal. 

Notice that the Active Message Count value for the queue is now 2. Each time you run the sender application without retrieving the messages (as described in the next section), this value increases by 1. 

===

## Step 3: Write code to receive messages to the queue

1.  Create a new Python file named +++ReceiveMessages.py+++

1.  In ReceiveMessages.py, enter the following:

    ```
    from azure.servicebus import QueueClient, Message

    # Create the QueueClient
    queue_client = QueueClient.from_connection_string("<Connection_string>", "taskqueue")

    # Receive the message from the queue
    with queue_client.get_receiver() as queue_receiver:
        messages = queue_receiver.fetch_next(timeout=3)
        for message in messages:
            print(message)
            message.complete()

    print("Messages received")
    ```
    >[!NOTE] You will need to substitute your value for the **Primary Connection_string** that you saved earlier.  

1.  In the left navigation, select **ReceiveMessages.py** in the left navigation and run the Python program by pressing the green **run** button in the top right.

1.  Review the output showing the 2 messages that were received.  
These messages have now been removed from the Service Bus queue. 

1.  Check the portal again.  
Notice that the **Active Message Count** value is now 0. You may need to refresh the portal page.

## End of demonstration



