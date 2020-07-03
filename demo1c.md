# Demo: Create a Web App with a local Git deployment source

This demo shows you how to deploy your app to Azure App Service from a local Git repository by using the Kudu build server and the Azure CLI. 


## Download sample code and launch Azure CLI

1. To download a sample repository, run the following command in your Git Bash window:

    ```bash
    git clone git clone https://github.com/Azure-Samples/python-docs-hello-world
    ```

    Later in the demo you'll be entering more commands in the Git Bash window so be sure to leave it open.
    
2.  Go to the folder and inspect the files

    ```bash
    cd python-docs-hello-world
    ```
    
    The repository contains an application.py file, which tells App Service that the code contains a Flask app. 

3. Launch the Azure Cloud Shell and be sure to select the **Bash** environment.
    * You can either launch the Cloud Shell through the portal (https://portal.azure.com),
or by launching the shell directly (https://shell.azure.com).


## Create the web app 

In the Cloud Shell run the following commands to create the web app and the necessary resources:

1. Create a resource group:

    ```bash
    az group create --location <MyLocation> --name <MyResourceGroup>
    ```

2. Create an app service plan:

    ```bash
    az appservice plan create --name <MyPlan> --resource-group <MyResourceGroup>
    ```

3. Create the web app:

    ```bash
    az webapp create --name <MyUniqueApp> --resource-group <MyResourceGroup> --plan <MyPlan>  --deployment-local-git
    ```
   >**Note:** You should record the name that you used to create the web app <MyUniqueApp> as it will be used later for deployment of the code.
    
4.  Use the Azure portal to confirm that the web app was created.  Open the Azure Portal in a browser at _portal.azure.com_ and use the top search bar with the keywords _app service_.  You should now see the web app that you created in the previous step. Click on the **App Service** to see the Overview.


## Deploy with az webapp up

We'll be configuring and using the Flask server for deployments in this demo. Local Git can deploy to an Azure web app by using **az webapp up**. 

   >**Note:** Return to **Git Bash** to perform these steps

1. Configure a web application and run it locally using **Flask**.
    
    ```bash
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    export FLASK_APP=application.py
    flask run
    ```
2. Open a web browser, and go to the sample app at http://localhost:5000/. The app displays the message Hello World!.

   >**Note:** This web page is being served by the local Flask web app server.  We will deploy the same code to Azure later in the demonstration.

3. While in the **Git Bash** shell, login to your Azure account.

    ```bash
    az login
    ```
    
    >**Note:**  A new browser tab will open in Internet Edge.  You will be asked to login using your Azure account credentials.

4. Return to the **Git Bash** prompt. Make sure that you are in the _python-docs-hello-world_ folder. Now you can use **az webapp up** to deploy your Python code to the Azure web app:

    ```bash
    az webapp up --sku F1 -n <MyUniqueApp>
    ```

5. Observe the running web app.  Browse to the deployed application in your web browser at the URL http://<MyUniqueApp>.azurewebsites.net.
The Python sample code is running a Linux container in the App Service using a built-in image.
    
    >**Note:** You can also use the Azure Portal to inspect your running web app.  Go back to the Azure Portal and select **Properties** in the left navigation for the web app.  Click on the URL for the running web app.  A new browser tab should open showing the running web app
    
    >**Note:** The web app code uses HTTP whereas the web app **browse** link under **overview** will go to HTTPS


    

### What happens to my app during deployment?

The az webapp up command does the following actions:
-Create a default resource group.
-Create a default app service plan.
-Create an app with the specified name.
-Zip deploy files from the current working directory to the app.

## Verify results

In the Azure Portal navigate to the web app you created above:

1. In the **Overview** section select the **URL** to verify the app was deployed successfully. 
2. Select **Deployment Center** to view deployment information.

From here you can make change to the code in the local repository and push the change to the web app.

## Clean up resources

In the Cloud Shell use the following command to delete the resource group and the resources it contains. The `--no-wait` portion of the command will return you to the Bash prompt quickly without showing you the results of the command. You can confirm the resource group was deleted in the Azure Portal

```bash
az group delete --name <MyResourceGroup> --no-wait
```
