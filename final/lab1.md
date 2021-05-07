# Lab: Building a web application on Azure platform as a service offerings  (Python)
# Student lab manual

## Lab scenario

You're the owner of a startup organization and have been building an image gallery application for people to share great images of food. To get your product to market as quickly as possible, you decided to use Microsoft Azure App Service to host your web apps and APIs.

## Objectives

After you complete this lab, you will be able to:

-   Create various apps by using App Service.

-   Configure application settings for an app.

-   Deploy apps by using Kudu, the Azure Command-Line Interface (CLI), and zip file deployment.

## Lab setup

-   Estimated time: **45 minutes**

## Before you start

### Sign in to the lab virtual machine

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

===

### Exercise 1: Build a Python Web Apps using the Azure Portal

#### Task 1: Open the Azure portal

1. [] Sign in to the Azure portal (+++https://portal.azure.com+++) as +++@lab.CloudPortalCredential(LabUser).Username+++ using +++@lab.CloudPortalCredential(LabUser).Password+++ as the password.

    >[!NOTE] If this is your first time signing in to the Azure portal, a dialog box will display offering a tour of the portal. Select **Get Started** to skip the tour.

#### Task 2: Create a Web App

1.  [] Create a new web app with the following details:

    -   Existing resource group: **@lab.CloudResourceGroup(RG1).Name**
    
    -   Web App name: +++imgapipy@lab.LabInstance.Id+++

    -   Publish: **Code**

    -   Runtime stack: **Python 3.8**

    -   Operating System: **Linux**

    -   Region: **East US**

    -   New App Service plan: **Linux Plan**
    
    -   SKU and size: **F1**

    -   Application Insights: **Disabled**

1. [] Wait for Azure to finish creating the web app before you move forward with the lab. You'll receive a notification when the app is created.

#### Task 3: Configure the Web App

1. [] In the Azure portal's navigation pane, select **Resource groups**.

1. [] In the Resource groups blade, select **@lab.CloudResourceGroup(RG1).Name**.

1. [] In the @lab.CloudResourceGroup(RG1).Name blade, select **imgapipy@lab.LabInstance.Id**.

1. [] In the imgapipy@lab.LabInstance.Id blade, in the **Overview** section, select **Browse**.  
This will open a new browser tab. The placeholder page that loads indicates your web app is up and running and ready to receive deployment of your app's code.

1. [] In the imgapipy@lab.LabInstance.Id blade, in the **Settings** section, select **Properties**.

1. [] In the **Properties** section, copy the value of the **URL** text box. You'll use this value later in the lab.

#### Task 4: Write code to implement a web application in Python

1. [] Open a **Cloud Shell**. Ensure that **Bash** mode is selected

<!--- Yes, just add the Cloud Shell boilerplate ---!>

1. [] Run the following commands to set up a virtual environment and install Flask in your profile:

    ```bash
    python3 -m venv venv
    source venv/bin/activate
    pip install flask
    ```
1. [] Run these commands in the Cloud Shell to create the directory for your new web app:

    ```bash
    mkdir ~/BestBikeApp
    cd ~/BestBikeApp
    ```
1. [] Open the web-based Visual Studio Code editor to create and edit the application.py for your web app:

    ```bash
    code application.py
    ```
1. [] Copy and paste the following Python code to create the main web app functionality:
 
    ```python
    from flask import Flask
    app = Flask(__name__)

    @app.route("/")
    def hello():
        return "<html><body><h1>Hello Best Bike App!</h1></body></html>\n"
    ```
<!--- indent added  ---!>
1. [] Save the file and exit the editor. You can save the file and exit the editor through the "..." menu on the top right

1. [] In order to deploy your application to Azure, you will need to save your list of application requirements in a requirements.txt file. To do so, run the following command:

    ```bash
    pip freeze > requirements.txt
    ```

#### Task 5: Test the web application using a Flask web server in the Cloud Shell

1. [] From your **Cloud Shell** session, run the following commands to start your web application:
 
    ```bash
    cd ~/BestBikeApp
    export FLASK_APP=application.py
    flask run
    ```
1. [] Open a second **Cloud Shell** in a new browser tab. Use **shell.azure.com** and sign in with the same credentials you used for the Azure Portal.

1. [] In the second **Cloud Shell** type the following command to test the web application:

    ```bash
    curl http://127.0.0.1:5000/
    ```
1. [] You should see the following displayed:

    ```html
    <html><body><h1>Hello Best Bike App!</h1></body></html>
    ```
1. [] Exit the second **Cloud Shell** by typing +++exit+++ and selecting **quit**.

1. [] Return to the first **Cloud Shell** in the Azure Portal. Stop the Flask web server by using **CTRL-C** in the **Cloud Shell** session 

#### Task 6: Deploy a Python web application to Web Apps

1. [] In the Azure Portal open a **Cloud Shell** 

1. [] We will gather some information about our web app resource. Enter the following commands to store the information:

    ```bash
    APPNAME=$(az webapp list --query [0].name --output tsv)
    APPRG=$(az webapp list --query [0].resourceGroup --output tsv)
    APPPLAN=$(az appservice plan list --query [0].name --output tsv)
    APPSKU=$(az appservice plan list --query [0].sku.name --output tsv)
    APPLOCATION=$(az appservice plan list --query [0].location --output tsv)
    ```
    >[!NOTE] You can check the stored values by using the **echo** command in Bash.
    
1. [] Deploy the Python code to the Web App using **az webapp up**:

    ```bash
    cd ~/BestBikeApp
    az webapp up --name $APPNAME --resource-group $APPRG --plan $APPPLAN --sku $APPSKU --location "$APPLOCATION"
    ```

1. [] In the Azure Portal select **Properties** in the left navigation for the Web App.

1. [] Click on the URL for the Web App.

    >[!NOTE] you need to use http for the response, not https. You may need to refresh the browser to see the new Web App

1. [] Observe the html page for the running Web App. 

#### Review

In this exercise, you created a web app in Azure and then deployed your Python web application to Web Apps by using **az webapp up**.

---
### Congratulations!
You have successfully completed this exercise. 



