

# Lab: Building a web application on Azure platform as a service offerings
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

### Exercise 1: Build a Python Web Apps using the Azure Portal

#### Task 1: Open the Azure portal

1.  On the taskbar, select the **Microsoft Edge** icon.

1.  In the open browser window, go to the Azure portal (<https://portal.azure.com>).

1.  At the sign-in page, enter the email address for your Microsoft account, and then select **Next**.

1.  Enter the password for your Microsoft account, and then select **Sign in**.

    > **Note**: If this is your first time signing in to the Azure portal, a dialog box will display offering a tour of the portal. Select **Get Started** to skip the tour and begin using the portal.

#### Task 2: Create a Web App

1.  In the Azure portal's navigation pane, select **All services**.

1.  From the **All services** blade, select **Web**.

1.  Select **App Services** from the list.

1.  Select **Add** to create a new Web App.

1.  From the **Create Web App** blade, observe the tabs from the blade, such as **Basics**, **Monitoring**, **Tags**, and **Review + Create**.

    > **Note**: Each tab represents a step in the workflow to create a new Web App. At any time, you can select **Review + Create** to skip the remaining tabs.

1.  Select the **Basics** tab, and in the tab area, perform the following actions:
    
    1.  Leave the **Subscription** text box set to its default value.
    
    1.  In the **Resource group** section, select **Create new**, enter **ManagedPlatform**, and then select **OK**.
    
    1.  In the **Instance Details** section and the **Name** text box, enter a _unique name_ for your Web App.  _This name will be part of the app's URL: appname.azurewebsites.net. The name you choose must be unique among all Azure web apps._
        
    1.  In the **Publish** field, confirm the default selction of **Code**.
    
    1.  In the **Runtime stack** dropdown list, select **Python 3.8**.
    
    1.  For the **Operating System** selection, note that **Linux** is the only allowed option.
    
    1.  In the **Location** list, select the **(US) East US** region.
    
    1.  In the **App Service Plan** section, leave the default name for **Linux Plan**.
    
    1.  In the **Sku and size** field, select **F1**.  _To select the F1 tier, select Change size to open the Spec Picker wizard. On the Dev / Test tab, select F1 from the list, then select Apply._
    
    1.  Select **Review + Create**.
    


1.  From the **Review + Create** tab, review the options that you specified in the previous steps.

1.  Select **Create** to create the Web App by using your specified configuration.

1.  From the **Deployment** blade, wait for the creation task to complete before moving forward with this lab.

1.	Select the **Go to resource** button from the **Deployment** blade to go to the newly created Web App.

    > **Note**: This is just an empty slot for your Web Application.  You will still need to add some sample code in other lab steps.


#### Task 3: Configure the Web App

1.  In the Azure portal's navigation pane, select **Resource groups**.

1.  From the **Resource groups** blade, select the **ManagedPlatform** resource group that you created earlier in this lab.

1.  From the **ManagedPlatform** blade, select the web app that you created earlier in this lab.

1.  From the **Web App** blade, in the **Overview** section, select the **Browse** link. This will open a new browser tab. The placeholder page that loads indicates that your web app is up and running and ready to receive deployment of your app's code.![default page](https://github.com/networksetcetera/LODS/blob/master/images/lab1/Screen%20Shot%202020-07-02%20at%206.01.40%20PM.png)

1.  From the **Web App** blade in the **Settings** section, select the **Properties** link.

1.  In the **Properties** section, copy the value of the **URL** text box. You'll use this value later in the lab.


#### Task 4: Write code to implement a web application in Python

1.  Open a **Cloud Shell**. Ensure that **Bash** mode is selected

1.  Run the following commands to set up a virtual environment and install Flask in your profile:
    ```bash
    python3 -m venv venv
    source venv/bin/activate
    pip install flask
    ```
1.  Run these commands in the Cloud Shell to create the directory for your new web app:
    ```bash
    mkdir ~/BestBikeApp
    cd ~/BestBikeApp
    ```
1.  Open the web-based Visual Studio Code editor to create and edit the application.py for your web app:
    ```bash
    code application.py
    ```
1.  Copy and paste the following Python code to create the main web app functionality:
    ```python
    from flask import Flask
    app = Flask(__name__)

    @app.route("/")
    def hello():
    return "<html><body><h1>Hello Best Bike App!</h1></body></html>\n"
    ```
1.  Save the file and exit the editor. You can save the file and exit the editor through the "..." menu on the top right

1.  In order to deploy your application to Azure, you will need to save your list of application requirements in a requirements.txt file. To do so, run the following command
    ```bash
    pip freeze > requirements.txt
    ```

#### Task 5: Test the web application using a Flask web server in the Cloud Shell

1.  From your **Cloud Shell** session, run the following commands to start your web application:
    ```bash
    cd ~/BestBikeApp
    export FLASK_APP=application.py
    flask run
    ```
1.  Open a second **Cloud Shell** in a new browser tab. Use _shell.azure.com_ and sign in with the same credentials you used for the Azure Portal.

1.  In the second **Cloud Shell** type the following command to test the web application
    ```bash
    curl http://127.0.0.1:5000/
    ```
1.  You should see the following displayed:
    ```html
    <html><body><h1>Hello Best Bike App!</h1></body></html>
    ```
1.  Exit the second **Cloud Shell** by typing _exit_ and selecting _quit_

1.  Return to the first **Cloud Shell** in the Azure Portal. Stop the Flask web server by using **CTRL-C** in the **Cloud Shell** session 


#### Task 6: Deploy a Python web application to Web Apps

1.  In the Azure Portal open a **Cloud Shell** 

1.  We will gather some information about our web app resource. Enter the following commands to store the information 
    ```bash
    APPNAME=$(az webapp list --query [0].name --output tsv)
    APPRG=$(az webapp list --query [0].resourceGroup --output tsv)
    APPPLAN=$(az appservice plan list --query [0].name --output tsv)
    APPSKU=$(az appservice plan list --query [0].sku.name --output tsv)
    APPLOCATION=$(az appservice plan list --query [0].location --output tsv)
    ```
    >**Note:** you can check the stored values by using the _echo_ command in Bash
    
1.  Deploy the Python code to the Web App using **az webapp up**.
    ```bash
    cd ~/BestBikeApp
    az webapp up --name $APPNAME --resource-group $APPRG --plan $APPPLAN --sku $APPSKU --location "$APPLOCATION"
    ```

1.  In the Azure Portal select **Properties** in the left navigation for the Web App.

1.  Click on the URL for the Web App.

    >**Note:** you need to use http for the response, not https

1.  Observe the html page for the running Web App. ![running web app](https://github.com/networksetcetera/LODS/blob/master/images/lab1/Screen%20Shot%202020-07-02%20at%2010.37.44%20PM.png)


#### Review

In this exercise, you created a web app in Azure and then deployed your Python web application to Web Apps by using **az webapp up**.


### Exercise 2: Clean up your subscription 

#### Task 1: Open Azure Cloud Shell

1.  In the portal, select the **Cloud Shell** icon to open a new shell instance.

    > **Note**: The **Cloud Shell** icon is represented by a greater than sign (\>) and underscore character (\_).

1.  If this is your first time opening Cloud Shell using your subscription, you can use the **Welcome to Azure Cloud Shell Wizard** to configure Cloud Shell for first-time usage. Perform the following actions in the wizard:
    
    1.  A dialog box prompts you to create a new storage account to begin using the shell. Accept the default settings, and then select **Create storage**.
    
    1.  Wait for Cloud Shell to finish its initial setup procedures before moving forward with the lab.

    > **Note**: If you don't notice the Cloud Shell configuration options, this is most likely because you're using an existing subscription with this course's labs. The labs are written with the presumption that you're using a new subscription.

#### Task 2: Delete resource groups

1.  Enter the following command, and then select Enter to delete the **ManagedPlatform** resource group:

    ```
    az group delete --name ManagedPlatform --no-wait --yes
    ```

1.  Close the Cloud Shell pane in the portal.

#### Task 3: Close the active applications

-   Close the currently running Microsoft Edge application.

#### Review

In this exercise, you cleaned up your subscription by removing the resource groups used in this lab.
