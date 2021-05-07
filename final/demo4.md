# Demo: Working with Azure Cosmos DB by using Python

In this demo you'll create a Python app to perform the following operations in Azure Cosmos DB:

* Connect to an Azure Cosmos DB account
* Create a database
* Create a (CosmosDB) container

## Prerequisites

This demo is performed in the Visual Studio code on the virtual machine.

    >[!Note] Ensure that the you have downloaded the Allfiles folder to the F: drive

### Retrieve Azure Cosmos DB account keys

1. [] Login to the Azure portal. [https://portal.azure.com](https://portal.azure.com)

1. [] Navigate to the Azure Comsos DB account you created in the *Create Azure Cosmos DB resources by using the Azure Portal* demo.

1. [] Select **Keys** in the left navigation panel. Leave the browser open so you can copy the needed information later in this demo.

### Set up the Python environment

1. [] Open **Visual Studio Code**.

1. [] Install the Python extension. Select **extensions** in the left navigation and search for _Python_ then click **install**

1. [] Use the **File** menu to open the folder F:/Allfiles/Demos

1. [] Using the left-navigation in **Visual Studio Code** select **New Folder** and name the folder "git-samples"

1. [] Right-click on the folder **git-samples** and select **Open in Terminal**

1. [] Type the following command to clone a demonstration application

    ```
    git clone https://github.com/Azure-Samples/azure-cosmos-db-python-getting-started.git
    ```
1. [] Observe that a new folder was created called **azure-cosmos-db-python-getting-started**

1. [] In Visual Studio Code, open a **Terminal** and create a Virtual Environment for Python:

    ```
    python -m venv .venv
    ```
1.  [] Activate the Python virtual environment

    ```
    .venv\scripts\activate
    ```
    
1. [] Using the same terminal, sign into your Azure account as +++@lab.CloudPortalCredential(LabUser).Username+++ using +++@lab.CloudPortalCredential(LabUser).Password+++ as the password:

    ```
    az login
    ```

1. [] Select the **View** > **Command Palette** menu and enter +++**Python: Select Interpreter**+++
    
    >[!TIP] In the dropdown menu select the virtual environment **.venv** that you created earlier.  You can also select the Python interpreter by clicking in the bottom left corner of Visual Studio Code

## Use the CosmosDB account keys in the sample Python application

### Observe the sample Python program

1. [] In the left-hand navigation pane of **Visual Studio Code** expand the folder **azure-cosmos-db-python-getting-started**

1. [] Click on **cosmos_get_started.py** and observe the sections of the Python program

- Initialize the Cosmos Client
- Create a Database
- Create a Container

### Modify code to connect to an Azure Cosmos DB account

1. [] Find the code under the section _# Initialize the Cosmos Client_

    ```
    # Initialize the Cosmos client
    endpoint = "endpoint"
    key = 'primary_key'
    ```

1. [] Using the **Azure Portal** copy the value _URI_ and substitute the value in the code

    ```
    endpoint = '<paste-your-value>'
    ```
1. [] Using the **Azure Portal** copy the value _PRIMARY KEY_ and substitute the value in the code

    ```
    key = '<paste-your-value>'
    ```

## Observe the code to connect to an Azure Cosmos DB account

1. [] Find the code under the section _# Create Cosmos Client_

    ```
    # <create_cosmos_client>
    client = CosmosClient(endpoint, key)
    ```
## Observe the code to create a Database

1. [] Find the code under the section _# Create a database_

    ```
    # Create a database
    database_name = 'AzureSampleFamilyDatabase'
    database = client.create_database_if_not_exists(id=database_name)
    ```

## Observe the code to create a container

1. [] Find the code under the section _# Create a container_

    ```
    # Create a container
    container_name = 'FamilyContainer'
    container = database.create_container_if_not_exists(
        id=container_name, 
        partition_key=PartitionKey(path="/lastName"),
        offer_throughput=400
    )
    ```

### Add packages to Python using PIP

1. [] At the Python command line type the following command and press **enter**:

    ```
    pip install --pre azure-cosmos
    ```
1. [] Run the Python program by clicking the green **run Python file in terminal** button in the top right
        
    >✔️ **Note:** You can verify the results by returning to your browser and selecting **Browse** in the **Containers** section in the left navigation. You may need to select **Refresh**. You should see 4 new documents added the the families container
    

## Wrapping up

You can now safely delete the *az204-cosmos-rg* resource group from your account.



