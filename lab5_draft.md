

# Lab: Deploying compute workloads by using images and containers
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

----

### Exercise 1: Use the Azure Python libraries to provision a virtual machine

This example demonstrates how to use the Azure SDK management libraries in a Python script to create a resource group that contains a Linux virtual machine. 


## 1: Set up your local development environment

1. Open a command prompt using Windows **Start** and type **cmd**

1. Open a command prompt and check your installation of Python by running
    ``` cmd
    python --version
    ```
1. Change to the directory **F:/allfiles/labs/05/starter**
    ``` cmd
    cd F:/allfiles/labs/05/starter
    ```
1. Check the availabity of the Azure CLI by typing
    ```bash
    az --version
    ```
1. Login to your Azure account
    ```bash
    az login
    ```
   >**Note:** You will be prompted for your Azure credentials

## 2: Install the needed Azure library packages

1. Create a *requirements.txt* file that lists the management libraries used in this example:

    ```txt
    azure-mgmt-resource
    azure-mgmt-network
    azure-mgmt-compute
    azure-cli-core
    ```

1. In your terminal or command prompt with the virtual environment activated, install the management libraries listed in *requirements.txt*:

    ```cmd
    pip install -r requirements.txt
    ```

## 3: Write code to provision a virtual machine

1.  Create a Python file named *provision_vm.py* with the following code. 

    ```python
    # Import the needed management objects from the libraries. The azure.common library
    # is installed automatically with the other libraries.
    from azure.common.client_factory import get_client_from_cli_profile
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.network import NetworkManagementClient
    from azure.mgmt.compute import ComputeManagementClient
    ```
    >Note: This imports the libraries from the Azure SDK for Python

1.  Provision a resource group. 

    Add these lines to the Python file *provision_vm.py*

    ```python

    # Obtain the management object for resources, using the credentials from the CLI login.
    resource_client = get_client_from_cli_profile(ResourceManagementClient)

    RESOURCE_GROUP_NAME = "PythonAzureExample-VM-rg"
    LOCATION = "centralus"

    # Provision the resource group.
    rg_result = resource_client.resource_groups.create_or_update(RESOURCE_GROUP_NAME,
        {
            "location": LOCATION
        }
    )

    print(f"Provisioned resource group {rg_result.name} in the {rg_result.location} region")
    ```

    >Note: This is the equivalent of the Azure CLI command

    ```
    az group create -n PythonAzureExample-VM-rg -l centralus
    ```
1.  Provision a virtual network. 

    Add these lines to the Python file *provision_vm.py*

    ```python

    # Network and IP address names
    VNET_NAME = "python-example-vnet"
    SUBNET_NAME = "python-example-subnet"
    IP_NAME = "python-example-ip"
    IP_CONFIG_NAME = "python-example-ip-config"
    NIC_NAME = "python-example-nic"

    # Obtain the management object for networks
    network_client = get_client_from_cli_profile(NetworkManagementClient)

    # Provision the virtual network and wait for completion
    poller = network_client.virtual_networks.create_or_update(RESOURCE_GROUP_NAME,
        VNET_NAME,
       {
            "location": LOCATION,
            "address_space": {
                "address_prefixes": ["10.0.0.0/16"]
            }
        }
    )

    vnet_result = poller.result()

    print(f"Provisioned virtual network {vnet_result.name} with address prefixes {vnet_result.address_space.address_prefixes}")
    ```
1.  Provision the subnet and wait for completion

    Add these lines to the Python file *provision_vm.py*
    
    ```python
    poller = network_client.subnets.create_or_update(RESOURCE_GROUP_NAME, 
        VNET_NAME, SUBNET_NAME,
        { "address_prefix": "10.0.0.0/24" }
    )
    subnet_result = poller.result()

    print(f"Provisioned virtual subnet {subnet_result.name} with address prefix {subnet_result.address_prefix}")

    ```

1.  Provision an IP address and wait for completion

    Add these lines to the Python file *provision_vm.py*

    ```python

    poller = network_client.public_ip_addresses.create_or_update(RESOURCE_GROUP_NAME,
    IP_NAME,
    {
        "location": LOCATION,
        "sku": { "name": "Standard" },
        "public_ip_allocation_method": "Static",
        "public_ip_address_version" : "IPV4"
    }
    )

    ip_address_result = poller.result()

    print(f"Provisioned public IP address {ip_address_result.name} with address {ip_address_result.ip_address}")
    ```
1.  Provision the network interface client

    Add these lines to the Python file *provision_vm.py*
    
    ```python
    poller = network_client.network_interfaces.create_or_update(RESOURCE_GROUP_NAME,
    NIC_NAME, 
    {
        "location": LOCATION,
        "ip_configurations": [ {
            "name": IP_CONFIG_NAME,
            "subnet": { "id": subnet_result.id },
            "public_ip_address": {"id": ip_address_result.id }
        }]
    }
    )

    nic_result = poller.result()

    print(f"Provisioned network interface client {nic_result.name}")
    ```
1.  Provision the virtual machine

    Add these lines to the Python file *provision_vm.py*
    
    ```python
    # Obtain the management object for virtual machines
    compute_client = get_client_from_cli_profile(ComputeManagementClient)

    VM_NAME = "ExampleVM"
    USERNAME = "azureuser"
    PASSWORD = "ChangePa$$w0rd24"

    print(f"Provisioning virtual machine {VM_NAME}; this operation might take a few minutes.")

    # Provision the VM specifying only minimal arguments, which defaults to an Ubuntu 18.04 VM
    # on a Standard DS1 v2 plan with a public IP address and a default virtual network/subnet.

    poller = compute_client.virtual_machines.create_or_update(RESOURCE_GROUP_NAME, VM_NAME,
    {
        "location": LOCATION,
        "storage_profile": {
            "image_reference": {
                "publisher": 'Canonical',
                "offer": "UbuntuServer",
                "sku": "16.04.0-LTS",
                "version": "latest"
            }
        },
        "hardware_profile": {
            "vm_size": "Standard_DS1_v2"
        },
        "os_profile": {
            "computer_name": VM_NAME,
            "admin_username": USERNAME,
            "admin_password": PASSWORD
        },
        "network_profile": {
            "network_interfaces": [{
                "id": nic_result.id,
            }]
        }
    }
    )

    vm_result = poller.result()

    print(f"Provisioned virtual machine {vm_result.name}")
    ```

## 4. Run the script

Type this at the command prompt and press **enter** to run the Python script

```cmd
python provision_vm.py
```

The provisioning process takes a few minutes to complete.


## 5. Verify the resources

Open the [Azure portal](https://portal.azure.com), navigate to the "PythonAzureExample-VM-rg" resource group, and note the virtual machine, virtual disk, network security group, public IP address, network interface, and virtual network:


----


### Exercise 2: Create a Docker container image and deploy it to Azure Container Registry

#### Task 1: Open the Cloud Shell and editor

1.  In the Azure portal's navigation pane, select the **Cloud Shell** icon to open a new shell instance.  

    > **Note**: Wait for Cloud Shell to finish connecting to an instance before moving on with the lab.

1.  At the **Cloud Shell** command prompt in the portal, enter the following command, and then select Enter to move from the root directory to the **\~/clouddrive** directory:

    ```
    cd ~/clouddrive
    ```

1.  Enter the following command, and then select Enter to create a new directory named **ipcheck** in the **\~/clouddrive** directory:

    ```
    mkdir ipcheck
    ```

1.  Enter the following command, and then select Enter to change the active directory from **\~/clouddrive** to **\~/clouddrive/ipcheck**:

    ```
    cd ~/clouddrive/ipcheck
    ```

1.  Enter the following command, and then select Enter to create a new .NET console application in the current directory:

    ```
    dotnet new console --output . --name ipcheck
    ```

1.  Enter the following command, and then select Enter to create a new file in the **\~/clouddrive/ipcheck** directory named **Dockerfile**:

    ```
    touch Dockerfile
    ```

1.  Enter the following command, and then select Enter to open the embedded graphical editor in the context of the current directory:

    ```
    code .
    ```

#### Task 2: Create and test a .NET application

1.  In the graphical editor, find the FILES pane, and then open the **Program.cs** file to open it in the editor.

1.  Delete the entire contents of the **Program.cs** file.

1.  Copy and paste the following code into the **Program.cs** file:

    ```
    public class Program
    {
        public static void Main(string[] args)
        {        
            // Check if network is available
            if (System.Net.NetworkInformation.NetworkInterface.GetIsNetworkAvailable())
            {
                System.Console.WriteLine("Current IP Addresses:");

                // Get host entry for current hostname
                string hostname = System.Net.Dns.GetHostName();
                System.Net.IPHostEntry host = System.Net.Dns.GetHostEntry(hostname);
                
                // Iterate over each IP address and render their values
                foreach(System.Net.IPAddress address in host.AddressList)
                {
                    System.Console.WriteLine($"\t{address}");
                }
            }
            else
            {
                System.Console.WriteLine("No Network Connection");
            }
        }
    }
    ```

1.  Save the **Program.cs** file by using the menu in the graphical editor or the Ctrl+S keyboard shortcut.  Don't close the graphical editor.

1.  Back at the command prompt, enter the following command, and then select Enter to run the application:

    ```
    dotnet run
    ```

1.  Find the results of the run. At least one IP address should be listed for the Cloud Shell instance.

1.  In the graphical editor, find the FILES pane of the editor, and then open the **Dockerfile** file to open it in the editor.

1.  Copy and paste the following code into the **Dockerfile** file:

    ```
    # Start using the .NET Core 2.2 SDK container image
    FROM mcr.microsoft.com/dotnet/core/sdk:2.2-alpine AS build

    # Change current working directory
    WORKDIR /app

    # Copy existing files from host machine
    COPY . ./

    # Publish application to the "out" folder
    RUN dotnet publish --configuration Release --output out

    # Start container by running application DLL
    ENTRYPOINT ["dotnet", "out/ipcheck.dll"]
    ```

1.  Save the **Dockerfile** file by using the menu in the graphical editor or by using the Ctrl+S keyboard shortcut.

1.  Close the Cloud Shell pane in the portal.

#### Task 3: Create a Container Registry resource

1.  In the Azure portal's navigation pane, select the **Create a resource** link.

1.  From the **New** blade, find the **Search the Marketplace** text box above the list of featured services.

1.  In the search box, enter **Container Registry**, and then select Enter.

1.  From the **Marketplace** search results blade, select the **Container Registry** result.

1.  From the **Container Registry** blade, select **Create**.

1.  From the **Create container registry** blade, perform the following actions:

    1.  In the **Registry name** text box, give your registry a globally unique name.

        >  **Note**: The blade will automatically check the name for uniqueness and inform you if you're required to choose a different name.

    1.  Leave the **Subscription** text box set to its default value.

    1.  In the **Resource group** drop-down list, select the existing **ContainerCompute** option.

    1.  In the **Location** text box, select **East US**.

    1.  In the **Admin user** section, select **Disable**.

    1.  In the **SKU** drop-down list, select **Basic**.

    1.  Select **Create**.  

    > **Note**: Wait for the creation task to complete before moving forward with this lab.

#### Task 4: Open Azure Cloud Shell and store Container Registry metadata

1.  In the portal, select the **Cloud Shell** icon to open a new shell instance.  

    > **Note**: Wait for Cloud Shell to finish connecting to an instance before moving forward with the lab.

1.  At the **Cloud Shell** command prompt in the portal, enter the following command, and then select Enter to get a list of all container registries in your subscription:

    ```
    az acr list
    ```

1.  Enter the following command, and then select Enter:

    ```
    az acr list --query "max_by([], &creationDate).name" --output tsv
    ```

1.  Enter the following command, and then select Enter:

    ```
    acrName=$(az acr list --query "max_by([], &creationDate).name" --output tsv)
    ```

1.  Enter the following command, and then select Enter:

    ```
    echo $acrName
    ```

#### Task 5: Deploy a Docker container image to Container Registry

1.  Enter the following command, and then select Enter to change the active directory from **\~/** to **\~/clouddrive/ipcheck**:

    ```
    cd ~/clouddrive/ipcheck
    ```

1.  Enter the following command, and then select Enter to get the contents of the current directory:

    ```
    dir
    ```

1.  Enter the following command, and then select Enter to upload the source code to your container registry and build the container image as a Container Registry task:

    ```
    az acr build --registry $acrName --image ipcheck:latest .
    ```

    > **Note**: Wait for the build task to complete before moving forward with this lab.

1.  Close the Cloud Shell pane in the portal.

#### Task 6: Validate your container image in Container Registry

1.  In the Azure portal's navigation pane, select the **Resource groups** link.

1.  From the **Resource groups** blade, find and then select the **ContainerCompute** resource group that you created earlier in this lab.

1.  From the **ContainerCompute** blade, select the container registry that you created earlier in this lab.

1.  From the **Container Registry** blade, find the **Services** section, and then select the **Repositories** link.

1.  In the **Repositories** section, select the **ipcheck** container image repository.

1.  From the **Repository** blade, select the **latest** tag.

1.  Find the metadata for the version of your container image with the **latest** tag.

    > **Note**: You can also select the **Run ID** link to find metadata about the build task.

#### Review

In this exercise, you created a .NET console application to display a machine’s current IP address. You then added the **Dockerfile** file to the application so that it could be converted into a Docker container image. Finally, you deployed the container image to Container Registry.

----

### Exercise 3: Deploy an Azure container instance

#### Task 1: Enable the admin user in Container Registry

1.  In the Azure portal's navigation pane, select the **Resource groups** link.

1.  From the **Resource groups** blade, find and then select the **ContainerCompute** resource group that you created earlier in this lab.

1.  From the **ContainerCompute** blade, select the container registry that you created earlier in this lab.

1.  From the **Container Registry** blade, select **Update**.

1.  From the **Update container registry** blade, perform the following actions:
    
    1.   In the **Admin user** section, select **Enable**.
    
    1.   Select **Save**.

    1.   Close the blade.
    
1.  Close the **Update container registry** blade.

#### Task 2: Automatically deploy a container image to an Azure container instance

1.  From the **Container Registry** blade, find the **Services** section, and then select the **Repositories** link.

1.  In the **Repositories** section, select the **ipcheck** container image repository.

1.  From the **Repository** blade, select the ellipsis menu associated with the **latest** tag entry.

1.  In the pop-up menu, select the **Run instance** link.

1.  From the **Create container instance** blade, perform the following actions:
    
    1.  In the **Container name** text box, enter **managedcompute**.
    
    1.  Leave the **Container image** text box set to its default value.
    
    1.  In the **OS type** section, select **Linux**.
    
    1.  Leave the **Subscription** text box set to its default value.
    
    1.  In the **Resource group** drop-down list, select **ContainerCompute**.
    
    1.  In the **Location** drop-down list, select **East US**.
    
    1.  In the **Number of cores** drop-down list, select **2**.
    
    1.  In the **Memory (GB)** text box, enter **4**.
    
    1.  In the **Public IP address** section, select **No**.
    
    1.  Select **OK**.

    > **Note**: Wait for the creation task to complete before moving forward with this lab.

#### Task 3: Manually deploy a container image to Container Instances

1.  In the Azure portal's navigation pane, select the **Resource groups** link.

1.  From the **Resource groups** blade, find and then select the **ContainerCompute** resource group that you created earlier in this lab.

1.  From the **ContainerCompute** blade, select the container registry that you created earlier in this lab.

1.  From the **Container Registry** blade, find the **Settings** section, and then select the **Access keys** link.

1.  In the **Access Keys** section, record the values for the following fields:
    
    1.  **Login server**
    
    1.  **Username**
    
    1.  **Password**

    > **Note**: You'll use these values later in this lab when you create another container instance.

1.  In the Azure portal's navigation pane, select the **Create a resource** link.

1.  From the **New** blade, find the **Search the Marketplace** text box above the list of featured services.

1.  In the search box, enter **Container instance**, and then select Enter.

1.  From the **Marketplace** search results blade, select the **Container Instances** result.

1.  From the **Container Instances** blade, select **Create**.

1.  Find the tabs from the **Create Container Instances** blade, such as **Basics**, **Networking**, and **Advanced**.

    > **Note**: Each tab represents a step in the workflow to create a new container instance.

1.  From the **Basics** tab, perform the following actions:

    1.  Leave the **Subscription** text box set to its default value.

    1.  In the **Resource group** drop-down list, select **ContainerCompute**.
    
    1.  In the **Container name** text box, enter **manualcompute**.

    1.  In the **Region** drop-down list, select **(US) East US**.
    
    1.  In the **Image source** section, select **Azure Container Registry**.
    
    1.  In the **Registry** drop-down list, select the **Azure Container Registry** resource that you created earlier in this lab.
    
    1.  In the **Image** drop-down list, select **ipcheck**.
    
    1.  In the **Image tag** drop-down list, select **latest**.

    1.  Select **Review + Create**.

1.  From the **Review + Create** tab, review the selected options.

1.  Select **Create** to create the container instance by using your specified configuration.  

    > **Note**: Wait for the creation task to complete before moving forward with this lab.

#### Task 4: Validate that the container instance ran successfully

1.  In the Azure portal's navigation pane, select the **Resource groups** link.

1.  From the **Resource groups** blade, find and then select the **ContainerCompute** resource group that you created earlier in this lab.

1.  From the **ContainerCompute** blade, select the **manualcompute** container instance that you created earlier in this lab.

1.  From the **Container Instance** blade, find the **Settings** section, and then select the **Containers** link.

1.  In the **Containers** section, find the list of **Events**.

1.  Select the **Logs** tab, and then find the text logs from the container instance.

> **Note**: You can also optionally find the **Events** and **Logs** from the **managedcompute** container instance.

> **Note**: After the application finishes running, the container terminates because it has completed its work. For the manually created container instance, you indicated that a successful exit was acceptable, so the container ran once. The automatically created instance didn't offer this option, and it assumes the container should always be running, so you'll notice repeated restarts of the container.

#### Review

In this exercise, you used multiple methods to deploy a container image to an Azure container instance. By using the manual method, you were also able to customize the deployment further and to run task-based applications as part of a container run.

### Exercise 4: Clean up your subscription 

#### Task 1: Open Azure Cloud Shell and list resource groups

1.  In the portal, select the **Cloud Shell** icon to open a new shell instance.

1.  At the **Cloud Shell** command prompt in the portal, enter the following command, and then select Enter to list all resource groups in the subscription:

    ```
    az group list
    ```

1.  Enter the following command, and then select Enter to find a list of possible commands to delete a resource group:

    ```
    az group delete --help
    ```

#### Task 2: Delete resource groups

1.  Enter the following command, and then select Enter to delete the **ContainerCompute** resource group:

    ```
    az group delete --name ContainerCompute --no-wait --yes
    ```

1.  Close the Cloud Shell pane in the portal.

#### Task 3: Close the active applications

-   Close the currently running Microsoft Edge application.

#### Review

In this exercise, you cleaned up your subscription by removing the resource groups used in this lab.
