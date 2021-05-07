# Lab: Deploying compute workloads by using images and containers (Python version)

<!---Martin G Removed the following disclaimer

!Instructions[](https://raw.githubusercontent.com/LODSContent/All-MOC/master/MOC/MSLearningDisclaimer)

--->

# Student Lab Manual

### Lab Scenario

<!---The scenario needs to be rewritten to reframe this demo/lab as a Python-oriented task--->

<!---Derrick:  Left the first exercise as CLI, changed the .NET code to Python and deployed to ACR or ACI--->

Your organization is seeking a way to virtualize or containerize a Python application. You're tasked with evaluating multiple compute services in Microsoft Azure and determining which service can help you automatically create VMs and install Python applications on those machines. As a proof of concept, you have decided to try creating VMs from built-in images and container images so that you can compare the two solutions. To keep your proof of concept simple, you'll create a special “IP check” application written in Python that you'll automatically deploy to your machines. Your proof of concept will evaluate the Azure Container Instances and Azure Virtual Machines services.

### Objectives

After you complete this lab, you will be able to:

- Create a VM by using the Azure Command-Line Interface (CLI).

- Deploy a Docker container image to Azure Container Registry.

- Deploy a container from a container image in Container Registry by using Container Instances.

### Estimated time

- 45 minutes

---

## Before you start

### Sign into the lab virtual machine

Log on to @lab.VirtualMachine(AZ-204T00A-SEA-DEV).SelectLink by pressing @lab.CtrlAltDelete and typing in the following credentials:
    
**Username**: +++@lab.VirtualMachine(AZ-204T00A-SEA-DEV).Username+++
    
**Password**: +++@lab.VirtualMachine(AZ-204T00A-SEA-DEV).Password+++

>[!TIP] Select the +++Type Text+++ icon to enter the associated text into the virtual machine.

>[!alert] If you are using the +++TypeText+++ feature to copy text to **VS Code**, you must first input the text into **Notepad** and the copy it into **VS Code** in order to maintain formatting.

>[!alert] **Do not** click nested Typetext icons. Doing so will produce the wrong output.
> 
>[!alert] !IMAGE[oftqjqbo.jpg](oftqjqbo.jpg)
---

## Download the lab files

@lab.Activity(DownloadFromGit)

---
 
!instructions[](https://raw.githubusercontent.com/LODSContent/All-MOC/master/MOC/CloudShellPrep)

===

### Exercise 1: Create a VM by using the Azure CLI 

#### Task 1: Open the Azure portal

1. [] Sign into the Azure portal (+++https://portal.azure.com+++) as +++@lab.CloudPortalCredential(LabUser).Username+++ using +++@lab.CloudPortalCredential(LabUser).Password+++ as the password.

    >[!NOTE] If this is your first time signing in to the Azure portal, a dialog box will display offering a tour of the portal. Select **Get Started** to skip the tour.

#### Task 2: Open Azure Cloud Shell

1. [] Open a new Cloud Shell instance in the Azure portal.

1. [] If the Cloud Shell is not already configured, configure the shell for Bash using the settings in the **Prepare cloud shell for later use** found at the beginning of this lab.

#### Task 3: Use the Azure CLI commands

1. [] Use the **az** command with the **\-\-help** flag to find a list of subgroups and commands at the root level of the CLI.

1. [] Use the **az vm** command with the **\-\-help** flag to find a list of subgroups and commands for Azure Virtual Machines:

1. [] Use the **az vm create** command with the **\-\-help** flag to find a list of arguments and examples for the **Create Virtual Machine** command:

1. [] Use the **az vm create** command to create a new VM with the following settings:
    
    -   Resource group: ** @lab.CloudResourceGroup(RG1).Name**

    -   Name: **quickvm**

    -   Image: **Debian**

    -   Username: **student**

    -   Password: **StudentPa55w.rd** 

    >[!NOTE] Wait for the VM creation process to complete. After the process completes, the command will return a JavaScript Object Notation (JSON) file with details about the machine.

1. [] Use the **az vm show** command to find a more detailed JSON file that contains various metadata about the newly created VM.

1. [] Use the **az vm list-ip-addresses** command to list all the IP addresses associated with the VM:

    ```
    az vm list-ip-addresses --resource-group RG1-@lab.LabInstance.Id --name quickvm
    ```

1. [] Use the **az vm list-ip-addresses** command and the **\-\-query** argument to filter the output to only return the first IP address value:

    ```
    az vm list-ip-addresses --resource-group RG1-@lab.LabInstance.Id --name quickvm --query '[].{ip:virtualMachine.network.publicIpAddresses[0].ipAddress}' --output tsv
    ```

1. [] Use the following script to store the results of the previous command in a new Bash shell variable named *ipAddress*:

    ```
    ipAddress=$(az vm list-ip-addresses --resource-group RG1-@lab.LabInstance.Id --name quickvm --query '[].{ip:virtualMachine.network.publicIpAddresses[0].ipAddress}' --output tsv)
    ```
    >[!Note] Ensure that you are using the **Bash** Cloud Shell. You can switch from Powershell to Bash before running this command.

1. [] Use the following script to render the value of the Bash shell variable **ipAddress**:

    ```
    echo $ipAddress
    ```

1. [] Use the following script to connect to the VM that you created earlier in this lab by using the Secure Shell (SSH) tool and the IP address stored in the Bash shell variable **ipAddress**:

    ```
    ssh student@$ipAddress
    ```

1. [] During the connection process, you'll receive a warning that the authenticity of the host can't be verified. Continue connecting to the host. Finally, use the password +++**StudentPa55w.rd**+++ when prompted for credentials

1. [] After connecting to the VM, use the following command to check the version of Python that is installed:

    ```
    python --version
    ```

1. [] Use the **exit** command to end your SSH session:

    ```
    exit
    ```

1. [] Close the Cloud Shell pane.

#### Review

In this exercise, you used Cloud Shell to create a VM as part of an automated script.

===
### Exercise 2: Create a Docker container image and deploy it to Azure Container Registry

#### Task 1: Open the Cloud Shell and editor

1. [] In the Azure portal's navigation pane, select **Cloud Shell** to open a new shell instance.  

1. [] At the **Cloud Shell** command prompt in the portal, enter the following command and then press Enter:

    ```
    cd ~/clouddrive
    ```

1. [] Enter the following command to create a new directory named **ipcheck** in the **\~/clouddrive** directory:

    ```
    mkdir ipcheck
    ```

1. [] Enter the following command to change the active directory from **\~/clouddrive** to **\~/clouddrive/ipcheck**:

    ```
    cd ~/clouddrive/ipcheck
    ```

1. [] Enter the following command to create a new file in the **\~/clouddrive/ipcheck** directory named **Dockerfile**:

    ```
    touch Dockerfile
    ```

#### Task 2: Create and test a Python application

1. [] At the **Cloud Shell** enter the following command to create a new Python script:

    ```
    code ipcheck.py
    ```
    
1. [] In the graphical editor, enter the following:

    ```
    # Python Program to Get IP Address 
    import socket    
    hostname = socket.gethostname()    
    IPAddr = socket.gethostbyname(hostname)    
    print("Your Computer Name is:" + hostname)    
    print("Your Computer IP Address is:" + IPAddr)
    ```

1. [] Using the menu in the graphical editor or press Ctrl+S to save the **ipcheck.py** file.  

1. [] At the command prompt, enter the following command to run the application:

    ```
    python ipcheck.py
    ```

1. [] Find the results of the run. The name of the computer and at least one IP address should be listed for the Cloud Shell instance.

1. [] In the graphical editor, find the FILES pane and then open the **Dockerfile** file.

1. [] Enter the following code into the **Dockerfile** file:

    ```
    FROM python:alpine3.7
    ADD ipcheck.py /
    CMD python ./ipcheck.py
    ```

1. [] Save the **Dockerfile** file.

1. [] In the portal, close the Cloud Shell pane.

#### Task 3: Create a Container Registry resource

1. [] In the Azure portal's navigation pane, select **Create a resource**.

1. [] In the **Search the Marketplace** box, enter +++**Container Registry**+++ and then press Enter.

1. [] In the **Marketplace** search results, select **Container Registry**.

1. [] On the **Container Registry** blade, select **Create**.

1. [] On the **Create container registry** blade, perform the following actions:

    1. In the **Registry name** box, enter +++**reg@lab.LabInstance.Id**+++.

        >[!NOTE] The service will automatically check the name for uniqueness and inform you if you're required to choose a different name.

    1. Leave the **Subscription** box set to its default value.

    1. Select the **Resource group** menu and then select **RG1-@lab.LabInstance.Id**.

    1. In the **Location** box, select **East US**.


    1. Select the **SKU** menu and then select **Basic**.

    1. Select **Create**.  

    >[!NOTE] Wait for the creation task to complete before moving forward with this lab.

#### Task 4: Open Azure Cloud Shell and store Container Registry metadata

1. [] In the portal, select the **Cloud Shell** icon to open a new shell instance.  

    >[!NOTE] Wait for Cloud Shell to finish connecting to an instance before moving forward with the lab.

1. [] At the **Cloud Shell** command prompt, enter the following command to see a list of all container registries in your subscription:

    ```
    az acr list
    ```

1. [] Enter the following command and then press Enter:

    ```
    az acr list --query "max_by([], &creationDate).name" --output tsv
    ```

1. [] Enter the following command and then press Enter:

    ```
    acrName=$(az acr list --query "max_by([], &creationDate).name" --output tsv)
    ```

1. [] Enter the following command and then press Enter:

    ```
    echo $acrName
    ```

#### Task 5: Deploy a Docker container image to Container Registry

1. [] Enter the following command to change the active directory from **\~/** to **\~/clouddrive/ipcheck**:

    ```
    cd ~/clouddrive/ipcheck
    ```

1. [] Enter the following command to get the contents of the current directory:

    ```
    dir
    ```

1. [] Enter the following command to upload the source code to your container registry and build the container image as a Container Registry task:

    ```
    az acr build --registry $acrName --image ipcheck-python:latest .
    ```

    >[!NOTE] Wait for the build task to complete before moving forward with this lab.

1. [] Close the Cloud Shell pane.

#### Task 6: Validate your container image in Container Registry

1. [] In the Azure portal's navigation pane, select **Resource groups**.

1. [] In the **Resource groups** blade, find and then select the **RG1-@lab.LabInstance.Id** resource group.

1. [] In the **RG1-@lab.LabInstance.Id** blade, select **reg@lab.LabInstance.Id**.

1. [] In the **Container Registry** blade, find the **Services** section and then select the **Repositories** link.

1. [] In the **Repositories** section, select **ipcheck-python**.

1. [] In the **Repository** blade, select **latest**.

1. [] Find the metadata for the version of your container image with the **latest** tag.

    >[!NOTE] You can also **Run ID** to find metadata about the build task.

#### Review

In this exercise, you created a .NET console application to display a machine’s current IP address. You then added the **Dockerfile** file to the application so that it could be converted into a Docker container image. Finally, you deployed the container image to Container Registry.

----

### Exercise 3: Deploy an Azure container instance

#### Task 1: Enable the admin user in Container Registry

1. [] In the Azure portal's navigation pane, select **Resource groups**.

1. [] In the **Resource groups** blade, find and then select the **RG1-@lab.LabInstance.Id** resource group that you created earlier in this lab.

1. [] In the **RG1-@lab.LabInstance.Id** blade, select **reg@lab.LabInstance.Id**.

1. [] In the **Container Registry** blade, select **Update**.

1. [] In the **Update container registry** blade, perform the following actions:
    
    1. In the **Admin user** section, select **Enable**.
    
    1. Select **Save**.

    1. Close the blade.
    
1. [] Close the **Update container registry** blade.

#### Task 2: Automatically deploy a container image to an Azure container instance

1. [] In the **Container Registry** blade, find the **Services** section and then select **Repositories**.

1. [] In the **Repositories** section, select **ipcheck-python**.

1. [] In the **Repository** blade, select the ellipsis menu associated with the **latest** tag entry.

1. [] In the dialog box, select **Run instance**.

1. [] In the **Create container instance** blade, perform the following actions:
    
    1. In the **Container name** box, enter +++**managedcompute**+++.
    
    1. Leave the **Container image** box set to its default value.
    
    1. In the **OS type** section, select **Linux**.
    
    1. Leave the **Subscription** box set to its default value.
    
    1. Select the **Resource group** menu and then select **RG1-@lab.LabInstance.Id**.
    
    1. Select the **Location** menu and then select **East US**.
    
    1. Select the **Number of cores** and then select **2**.
    
    1. In the **Memory (GB)** box, enter +++**4**+++.
    
    1. In the **Public IP address** section, select **No**.
    
    1. Select **OK**.

    >[!NOTE] Wait for the creation task to complete before moving forward with this lab.

#### Task 3: Manually deploy a container image to Container Instances

1. [] In the Azure portal's navigation pane, select **Resource groups**.

1. [] In the **Resource groups** blade, select the **RG1-@lab.LabInstance.Id** resource group that you created earlier in this lab.

1. [] In the **RG1-@lab.LabInstance.Id** blade, select **reg@lab.LabInstance.Id**.

1. [] In the **Container Registry** blade, in the **Settings** section, select **Access keys**.

1. [] In the **Access Keys** section, use Notepad to record the values for the following fields:
    
    1. **Login server**
    
    1. **Username**
    
    1. **Password**

    >[!NOTE] You'll use these values later in this lab when you create another container instance.

1. [] In the Azure portal's navigation pane, select **Create a resource**.

1. [] In the **New** blade, in the **Search the Marketplace** box, enter +++**Container instance**+++ and then press Enter.

1. [] In the **Marketplace** search results, select **Container Instances**.

1. [] In the **Container Instances** blade, select **Create**.

1. [] From the **Basics** tab, perform the following actions:

    1. Leave the **Subscription** text box set to its default value.

    1. In the **Resource group** drop-down list, select **RG1-@lab.LabInstance.Id**.
    
    1. In the **Container name** text box, enter **manualcompute**.

    1. In the **Region** drop-down list, select **(US) East US**.
    
    1. In the **Image source** section, select **Azure Container Registry**.
    
    1. In the **Registry** drop-down list, select the **Azure Container Registry** resource that you created earlier in this lab.
    
    1. In the **Image** drop-down list, select **ipcheck-python**.
    
    1. In the **Image tag** drop-down list, select **latest**.

    1. Select **Review + Create**.

1. [] In the **Review + Create** tab, review the selected options.

1. [] Select **Create** to create the container instance by using your specified configuration.  

    >[!NOTE] Wait for the creation task to complete before moving forward with this lab.

#### Task 4: Validate that the container instance ran successfully

1. [] In the Azure portal's navigation pane, select the **Resource groups** link.

1. [] In the **Resource groups** blade, find and then select the **RG1-@lab.LabInstance.Id** resource group that you created earlier in this lab.

1. [] In the **RG1-@lab.LabInstance.Id** blade, select the **manualcompute** container.

1. [] In the **Container Instance** blade, in the **Settings** section, select **Containers**.

1. [] In the **Containers** section, find the list of **Events**.

1. [] Select the **Logs** tab and then find the text logs from the container instance.

1. [] Review the output showing the compute instance name and the IP address.  
This should be similar to the output you saw in the **Cloud Shell**.

>[!NOTE] Optionally, you can find the **Events** and **Logs** from the **managedcompute** container instance.

#### Review

In this exercise, you containerized a Python script and used multiple methods to deploy a container image to an Azure container instance. By using the manual method, you were also able to customize the deployment further and to run task-based applications as part of a container run.

---
### Congratulations!
You have successfully completed this exercise. 


