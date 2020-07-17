# demo 4 - CosmosDB

# Demo: Working with Azure Cosmos DB by using .NET

In this demo you'll create a console app to perform the following operations in Azure Cosmos DB:

* Connect to an Azure Cosmos DB account
* Create a database
* Create a container

## Prerequisites

This demo is performed in the Visual Studio code on the virtual machine.

### Retrieve Azure Cosmos DB account keys

1.  Login to the Azure portal. [https://portal.azure.com](https://portal.azure.com)

1.  Navigate to the Azure Comsos DB account you created in the *Create Azure Cosmos DB resources by using the Azure Portal* demo.

1.  Select **Keys** in the left navigation panel. Leave the browser open so you can copy the needed information later in this demo.

### Set up the Python environment

1.  Open **Visual Studio Code**.

1.  Use the **File** menu to open the folder F:/Allfiles/Demos

1.  Using the left-navigation in **Visual Studio Code** select **New Folder** and name the folder "git-samples"

1.  Right-click on the folder **git-samples** and select **Open in Terminal**

1.  Type the following command to clone a demonstration application

    ```
    git clone https://github.com/Azure-Samples/azure-cosmos-db-python-getting-started.git
    ```
1.  Observe that a new folder was created called **azure-cosmos-db-python-getting-started**

1. [] In Visual Studio Code, open a **Terminal** and create a Virtual Environment for Python:

    ```
    python -m venv .venv
    ```
1.  [] Activate the Python virtual environment

    ```
    .venv\scripts\activate
    ```

1. [] In Visual Studio Code, create a new text file named +++**requirements.txt**+++ in the current folder. 

1. [] Add the following lines to the text file
    
    ```
    azure-storage-queue

    ```
1. [] Using the terminal, install the required Azure libraries for Python:

    ```
    pip install -r requirements.txt
    ```
1. [] Using the same terminal, sign into your Azure account as +++@lab.CloudPortalCredential(LabUser).Username+++ using +++@lab.CloudPortalCredential(LabUser).Password+++ as the password:

    ```
    az login
    ```

1. [] Select the **View** > **Command Palette** menu and enter +++**Python: Select Interpreter**+++
    
    >[!TIP] In the dropdown menu select the virtual environment **.venv** that you created earlier.  You can also select the Python interpreter by clicking in the bottom left corner of Visual Studio Code
## Build the console app

### Add packages and using statements



### Add code to connect to an Azure Cosmos DB account

1. Add these constants and variables into your `Program` class.


    The console displays the message: **End of demo, press any key to exit.** This message confirms that your application made a connection to Azure Cosmos DB. 

## Create a database

1. Copy and paste the `CreateDatabaseAsync` method below your `CosmosDemoAsync` method. `CreateDatabaseAsync` creates a new database with ID `az204Database` if it doesn't already exist, that has the ID specified from the `databaseId` field.

    ```csharp
    private async Task CreateDatabaseAsync()
    {
        // Create a new database
        this.database = await this.cosmosClient.CreateDatabaseIfNotExistsAsync(databaseId);
        Console.WriteLine("Created Database: {0}\n", this.database.Id);
    }
    ```

2. Copy and paste the code below where you instantiate the `CosmosClient` to call the **CreateDatabaseAsync** method you just added.

    ```csharp
        // Runs the CreateDatabaseAsync method
        await this.CreateDatabaseAsync();
    ```

3. Save your work and, in a terminal in VS Code, run the `dotnet run` command. The console displays the message: **Created Database: az204Database** 


## Create a container

1. Copy and paste the `CreateContainerAsync` method below your `CreateDatabaseAsync` method. 

    ```csharp
    private async Task CreateContainerAsync()
    {
        // Create a new container
        this.container = await this.database.CreateContainerIfNotExistsAsync(containerId, "/LastName");
        Console.WriteLine("Created Container: {0}\n", this.container.Id);
    }
    ```

2. Copy and paste the code below where you instantiated the `CosmosClient` to call the **CreateContainer** method you just added.

    ```csharp
    // Run the CreateContainerAsync method
    await this.CreateContainerAsync();
    ```

3. Save your work and, in a terminal in VS Code, run the `dotnet run` command. The console displays the message: **Created Container: az204Container** 
        
    >✔️ **Note:** You can verify the results by returning to your browser and selecting **Browse** in the **Containers** section in the left navigation. You may need to select **Refresh**.
    

## Wrapping up

You can now safely delete the *az204-cosmos-rg* resource group from your account.
