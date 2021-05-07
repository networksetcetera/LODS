# Module 6 Demos

This is an omnibus demo that comprises 2 separate demos. Each subsequent demo requires the preceding demo
to be completed. It is recommended to combine demos 1 and 2

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

## Download the demonstration files from GitHub

1.  Open **Visual Studio Code**.

1.  Use the **File** menu and open **F:\Allfiles\Demos**.

1.  Select **New Folder** and name the folder +++git-samples+++.

1.  Right-click **git-samples** and then select **Open in Terminal**.

1.  Type the following command to clone a demonstration application

    ```
    git clone https://github.com/Azure-Samples/ms-identity-python-daemon.git
    ```
1.  Verify a new folder named **azure-msidentity-python-daemon** was created.

===
# Demo 1: Register an app with the Microsoft Identity platform

In this demo you'll learn how to perform the following actions:

* Register an application with the Microsoft identity platform

## Prerequisites

This demo is performed in the Azure Portal.

### Login to the Azure Portal

1.  Login to the portal: [https://portal.azure.com](https://portal.azure.com) 

## Register a new application

1. In the Azure portal, open **Azure Active Directory**. 

1. In the **Active Directory** blade, select **App registrations** and then select **New registration**.

1. In the **Register an application** blade, enter your application's registration information:

    <table>
    <thead>
    <tr>
    <th>Field</th>
    <th>Value</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td><p><strong>Name</strong></p></td>
    <td><p><code>az204appregdemo</code></p></td>
    </tr>
    <tr>
    <td><p><strong>Supported account types</strong></p></td>
    <td><p>Select <strong>Accounts in this organizational directory</strong></p></td>
    </tr>
    <tr>
    <td><p><strong>Redirect URI (optional)</strong></p></td>
    <td><p>Select <strong>Public client/native (mobile &amp; desktop)</strong> and enter <code>http://localhost</code> in the box to the right.</p></td>
    </tr>
    </tbody>
    </table>

<!--
    Field | Value 
    - | - 
    **Name** | `az204appregdemo` | Enter a meaningful application name that will be displayed to users of the app.
    **Supported account types** | Select **Accounts in this organizational directory** 
    **Redirect URI (optional)** | Select **Public client/native (mobile & desktop)** and enter `http://localhost` in the box to the right.

    Below are more details on the **Supported account types**.

    Account type | Scope
    - | -
    **Accounts in this organizational directory only** | This option maps to Azure AD only single-tenant (only people in your Azure AD directory).
    **Accounts in any organizational directory** | This option maps to an Azure AD only multi-tenant. Select this option if you would like to target anyone from any Azure AD directory.
    **Accounts in any organizational directory and personal Microsoft accounts** | This option maps to Azure AD multi-tenant and personal Microsoft accounts. 

-->

1. Select **Register**.

Azure AD assigns a unique application (client) ID to your app, and you're taken to your application's **Overview** page. 

>✔️ **Note:** Leave the app registration in place, we'll be using it in future demos in this module.

>[!ALERT] If you decide to pause the demo at this point, please ensure you save the lab instance. If you close and exit the lab instance without saving it, 
you will lose the work you have done previously.

===

# Demo 2: A Python daemon app calling MS Graph by using the MSAL library and a client secret

This sample application shows how to use the Microsoft identity platform endpoint to access the data of Microsoft business customers .

It uses the _OAuth 2 client credentials grant_ to acquire an access token, which can be used to call the Microsoft Graph and access organizational data.

The app is Python Console application. It gets the list of users in an Azure AD tenant by using Microsoft Authentication Library (MSAL) for Python to acquire a token.

## Prerequisites

This demo is performed in Visual Studio Code. We'll be using information from the app we registered in the *Register an app with the Microsoft Identity platform* demo.

### Login to the Azure Portal and configure the **Registered Application**

1.  Login to the portal: [https://portal.azure.com](https://portal.azure.com) 

1.  Open **Azure Active Directory**.

1.  In the **Overview** section copy the name of your **Primary Domain**.

1. Navigate to the app registered in the previous demo.

    * Search for **App Registrations** in the top search bar of the portal.
    * Select the **az204appregdemo** app your registered earlier in this module.
    * Make sure you select **Overview** in the left navigation.

1. On the app **Overview** page, find the **Application (client) ID** value and record it for later.  
You'll need it to configure the Visual Studio configuration file for this project.

1. On the **Certificates & secrets** page, in the **Client secrets** section, select **New client secret**:

   - Type a key description (of instance `app secret`).
   - Select a key duration of either **In 1 year**, **In 2 years**, or **Never Expires**.
   - When you press the **Add** button, the key value will be displayed, copy, and save the value in a safe location.
   - You'll need this key later to configure the project in Visual Studio. This key value will not be displayed again, nor retrievable by any other means, so record it as soon as it is visible from the Azure portal.

1. In the list of pages for the app, select **API permissions**
   - Select **Add a permission**
   - Ensure that the **Microsoft APIs** tab is selected
   - In the **Commonly used Microsoft APIs** section, select **Microsoft Graph**
   - In the **Application permissions** section, ensure that the right permissions are selected: **User.Read.All**
   - Select **Add permissions**

1. At this stage permissions are assigned correctly but the client app does not allow interaction.
   No consent can be presented via a UI and accepted to use the service app.

1. Select **Grant/revoke admin consent for {tenant}** and then select **Yes** when you are asked if you want to grant consent for the requested permissions for all account in the tenant.

    >[!NOTE] you should now see a green check mark under the _status_ column for both API permissions
 
### Set up the Python environment

1. [] In Visual Studio Code, open a **Terminal** and create a virtual environment for Python:

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

### Set up the Python application and Configure the client project

1. In the left navigation pane of **Visual Studio Code** expand the **ms-identity-python-daemon** folder

1. Select **parameters.json**.

1. Find the string key **organizations** in the **authority** variable and replace the existing value with your Azure AD tenant name.

1. Find the string key **your_client_id** and replace the existing value with the **application ID (clientId)** of the registered application copied from the Azure portal.

1. Find the string key **The secret generated by AAD during your confidential app registration** and replace the existing value with the key you saved during the creation of the registered application, in the Azure portal.

### Observe the code sections

1. Create the MSAL confidential client application.

    ```Python
    app = msal.ConfidentialClientApplication(
        config["client_id"], authority=config["authority"],
        client_credential=config["secret"],
        )
    ```
1. Define the scopes.

    ```JSon
    "scope": [ "https://graph.microsoft.com/.default" ],
    ```

1. Acquire the token

    ```Python
    # The pattern to acquire a token 
    result = None

    # Firstly, looks up a token from cache
    # Since we are looking for token for the current app, NOT for an end user
    result = app.acquire_token_silent(config["scope"], account=None)

    if not result:
    logging.info("No suitable token exists in cache. Let's get a new one from AAD.")
    result = app.acquire_token_for_client(scopes=config["scope"])
    ```

1. Call the API

    In that case calling "https://graph.microsoft.com/v1.0/users" with the access token as a bearer token.

    ```Python
    if "access_token" in result:
        # Calling graph using the access token
        graph_data = requests.get(  # Use token to call downstream service
        config["endpoint"],
        headers={'Authorization': 'Bearer ' + result['access_token']}, ).json()
    print("Users from graph: " + str(graph_data))
    else:
        print(result.get("error"))
        print(result.get("error_description"))
        print(result.get("correlation_id"))  # You may need this when reporting a bug
    ```

## Build the Python app

### Add required packages using PIP

1.  Return to **Visual Studio Code** and the terminal for Python.

1.  Install the dependencies using pip as follows:
  
    ```
    pip install -r requirements.txt
    ```

## Run the application

1.  Start the application, it will display a JSON string 
    ```Shell
    python confidential_client_secret_sample.py parameters.json
    ```

1. You should see a JSON response in the console containing the users in the tenant.

    ```
    Business Phone:
    Display Name:  
    ..
    ..
    Id: 
    ```


