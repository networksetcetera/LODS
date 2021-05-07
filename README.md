# LODS
June contract

Module 05b (Python): Deploying Compute Workloads by using Images and Containers (Python)
https://labondemand.com/LabProfile/75239

Module 05a (CLI): Deploying Compute Workloads by using Images and Containers
https://labondemand.com/LabProfile/75231

Labs we are looking at

1, 2, 5, 11

Titles are:

#### 01 - Building a Web Application on Azure PaaS , M01/L02 demo – Create a Web App (3 demos)
#### 02 -Implement Task Processing Logic by using Azure Functions , M02/L02 demo – Create an HTTP trigger (1 demo)
#### 05 – Deploying Compute Workloads by using Images and Containers ,  M05/L01 demo – Create an Azure VM (2 demos)
#### 11 – Asynchronously processing messages by using Azure Storage , M11/L01 demo – Send and receive messages from a Service Bus queue (1 demo)

## POC links
https://docs.microsoft.com/en-us/azure/developer/python/

https://docs.microsoft.com/en-ca/learn/certifications/exams/az-204 (scroll)


Lab 01
https://docs.microsoft.com/en-ca/learn/modules/host-a-web-app-with-azure-app-service/6-exercise-deploy-your-code-to-app-service?pivots=python

https://docs.microsoft.com/en-ca/azure/app-service/containers/quickstart-python?tabs=bash


## June 10

Lab 05

https://github.com/MicrosoftLearning/AZ-204-DevelopingSolutionsforMicrosoftAzure/blob/master/Instructions/Labs/AZ-204_05_lab_ak.md

https://docs.microsoft.com/en-ca/azure/developer/python/azure-sdk-example-virtual-machines?tabs=cmd

https://docs.microsoft.com/en-ca/samples/azure-samples/resource-manager-python-template-deployment/resource-manager-python-template-deployment/

https://docs.microsoft.com/en-ca/samples/browse/?term=python%20deploy%20vm


See also Github notes here:

https://github.com/networksetcetera/LODS


## June  15
looking at M05
used instructor AZ-204
https://docs.microsoft.com/en-ca/azure/developer/python/configure-local-development-environment?tabs=cmd#required-components
installed Python with the latest download
*** be sure to select "add to path" option
used "terminal"
az --version ..ok
python --version ..ok
code .  ..ok

https://docs.microsoft.com/en-ca/azure/developer/python/

Specific to Lab 5
https://docs.microsoft.com/en-ca/azure/developer/python/azure-sdk-example-virtual-machines?tabs=cmd

tested
.. runs ok in "terminal"
.. did not run in VS Code (probably because azure module was not yet installed)

Other examples will be here
https://docs.microsoft.com/en-ca/python/api/overview/azure/containerinstance?view=azure-python

container sample
https://github.com/Azure-Samples/aci-docs-sample-python



full list of samples on Github
https://github.com/Azure/azure-sdk-for-python/tree/master/sdk

## June 21
For the rest of Lab 5 focus on Exercise 3 (less on Exercise 2)
.. deploy ane existing container image
.. use one from Docker Hub or as indicated by the code samples


## June 22-28 > AWS exam

## June 29

Looking at my fork of Lab 11

https://github.com/networksetcetera/AZ-204-DevelopingSolutionsforMicrosoftAzure/blob/master/Instructions/Labs/AZ-204_11_lab.md

looks like we should build it from scratch like CA video demo

This is more relevant to the "demo" which refers to **Service Bus Queues**
https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-python-how-to-use-queues

## June 30

added VS Code and got it to work as an editor and for running Python code
also got venv to work
https://code.visualstudio.com/docs/python/python-on-azure

https://code.visualstudio.com/docs/python/python-tutorial

**NOTE** the syntax is slightly different for Python in **Visual Studio Code** vs Python in the Azure **Cloud Shell**

https://code.visualstudio.com/docs/python/environments   

.venv   vs   venv  and also **conda**

Lab 11 is focussed on **Storage Queues** so this example is more relevant
https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-queue

start with 
pip install azure-storage-queue

writing to the queue works

reading and deleting is here:
https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_message.py

Demo 11
https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/servicebus/azure-servicebus/samples

You can get the **demo scripts** from AZ-203 here (taken from AZ-204 courseware)
https://github.com/networksetcetera/AZ203T00

You can list environment variables like this in Powershell (used for sample code)
Get-ChildItem Env:

## July 1

Lab 02 - Functions

Changed from .NET to Python
Platform has to be Linux

also installed node.js with chocolatey option

you can choose the shell in VS Code = terminal + choose default shell

See screenshots in **images** 

https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-python

other links
required for VS Code Python extension
https://dotnet.microsoft.com/download

https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions

link to my example
https://funclogicmoko.azurewebsites.net/api/HttpTrigger1?name=fred

## July 2

Consolidate labs, POC for demos

Looked over Labs and demos
- some don't need modification
- some need alot (the rabbit holes)

Add Martin, Tom (perhaps Bill) as private collaborators to repo
https://help.github.jp/enterprise/2.11/user/articles/inviting-collaborators-to-a-personal-repository/

Consider Python example for Demo 1
https://docs.microsoft.com/en-ca/azure/app-service/containers/quickstart-python?tabs=bash


## July 3

Mount working labs in IDLx - done
Try screen shot feature to improve clarity - done

AWS POCs - created a lab series

Key to LODS labs
- One Lab Series = AZ-204-MultiMan-Demo
- Three Lab Profiles
- AZ-204 Multi-Man (Demo) = toggle based on demo1a, 1b and 1c
- Module 05a (CLI) = raw instructions from MS/Github (screenshots)
- Module 05b (Python) = Python version , currently Exercise 1

- todo: create lab masters for other labs - done

Typical link to demo
https://raw.githubusercontent.com/JeffKoMS/AZ203T00/master/demos/M01/L02/01-Demo_Create_a_WebApp_by_using_the_Azure_Portal.md

Lab 11
- saved the workspace
- Fixed the create_queue() issue
- todo: write a step-by-step guide to setting up Python in **Visual Studio Code** or todo: include this in the lab setup

Lab 5
- now works in **Visual Studio Code**
- saved the workspace
- todo: break this down into chunks

Lab 2
- todo: timer trigger - done, see funclogicxx2, Recurring

- use httprepl with the Python Function endpoint. httprepl is .Net Core, simpler to work with direct URL endpoint
https://ciwchris.github.io/blog/20-using-the-dotnet-httprepl-tool.html
https://devblogs.microsoft.com/aspnet/httprepl-a-command-line-tool-for-interacting-with-restful-http-services/
https://docs.microsoft.com/en-us/aspnet/core/web-api/http-repl?view=aspnetcore-3.1&tabs=windows

- try CURL instead of HTTPREPL for local Python Function testing

- todo: integration, can leave this as .Net or find an integration example
- this includes an integration example (queues)
https://docs.microsoft.com/en-us/azure/developer/python/tutorial-vs-code-serverless-python-01
- here is a good blog article about local testing
https://www.scalyr.com/blog/azure-functions-in-python-a-simple-introduction/
- main MS documentation
https://docs.microsoft.com/en-us/azure/azure-functions/functions-reference-python
- Python Function sample
https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-storage-blob
- Python Django (web app framework)
https://code.visualstudio.com/docs/python/tutorial-django
- Python Flask (web app framework)
https://code.visualstudio.com/docs/python/tutorial-flask

- integration test successful. See funclogicxx3, GetSettingInfo and the run log in Azure. 3 successful calls



## July 4

The Azure CLI is written in Python
https://www.python.org/success-stories/building-an-open-source-and-cross-platform-azure-cli-with-python/
https://pypi.org/project/azure-cli/


Azure SDK for Python
https://docs.microsoft.com/en-us/azure/developer/python/azure-sdk-install?view=azure-python

Package index
https://docs.microsoft.com/en-us/azure/developer/python/azure-sdk-library-package-index

Examples and Tutorials
https://docs.microsoft.com/en-us/azure/developer/python/azure-sdk-example-resource-group
- Provision a resource group
- Provision Azure storage
- Use Azure storage
- Provision & deploy a web app
- Provision & query a database
- Provision a virtual machine
- Explore specific Azure services
- Scenario-specific tutorials
- How-to guides
- Samples

Recommended Environment
https://docs.microsoft.com/en-us/azure/developer/python/configure-local-development-environment?tabs=cmd
- VS code, with recommended extensions
- CMD prompt
- venv

## July 5

Lab01 - alternate (from PyCon 2020)
https://docs.microsoft.com/en-us/azure/developer/python/tutorial-deploy-app-service-on-linux-01

Lab05 - alternate
https://docs.microsoft.com/en-us/azure/developer/python/tutorial-deploy-containers-01

Lab02 - alternate
https://docs.microsoft.com/en-us/azure/developer/python/tutorial-vs-code-serverless-python-01

See Martin's suggestions
https://github.com/networksetcetera/LODS/issues/1#issuecomment-653840527

Look at demos for Module 5.  There are 8 demos (not 2)
- create an Azure VM (2, Portal, Powershell) .. suggest Python option
- create an ARM template (2, Portal, VS Code) 
- containers 
- Demo: Retrieve and deploy existing Docker image locally
- Demo: Create a container image by using Docker
- Demo: Deploy an image to ACR by using Azure CLI .. suggest a Python option
- Demo: Run Azure Container Instances by using the Cloud Shell

Consider _Python in a container_
https://code.visualstudio.com/docs/containers/quickstart-python

Intially, you would just create a Python web app using _Flask_ then you can put the entire web app in a _Docker container_

Compare ACR, ECR and Docker Hub
https://www.g2.com/compare/azure-container-registry-vs-docker-hub-vs-amazon-elastic-container-registry-ecr

## July 6

Released 
- Lab 1
- Demo 1, with Python option
- Lab 5, Exercise 1 only

## July 7

todo: Functions Lab and Functions Demo

todo: follow up on invoice

Demo 5

- mcr.microsoft.com cannot be browsed.  Instead use Docker Hub to list images published by MS

- Consider Python in the container (See note in issues)
https://hub.docker.com/_/python

- Consider Flask in a container
https://docs.microsoft.com/en-us/azure/developer/python/tutorial-deploy-containers-01

- easy test, Using **Module 01b (Python)**
https://hub.docker.com/_/hello-world

## July 8 to 9

Scope change

## July 10 

New list of Labs and Demos
- Lab 1
- Lab 2
- Lab 5
- Lab 11
- Demo 4
- Demo 6 (counts as 2)
- Demo 11

References for Demo 6 (identity and authentication)

https://docs.microsoft.com/en-us/azure/developer/python/quickstarts-identity-security

https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-v2-python-daemon

https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-v2-python-webapp

References for Demo 4 (Cosmos DB)

https://docs.microsoft.com/en-us/azure/developer/python/quickstarts-data-solutions

## July 13

Got confirmation of the new ask

Sent Lab11 for review 

## July 14

Demo11 - working (Service Bus, See files in Onedrive)

Demo04 - found a better example (https://docs.microsoft.com/en-us/azure/cosmos-db/create-sql-api-python)

Code version is here (https://docs.microsoft.com/en-us/azure/cosmos-db/create-sql-api-python#review-the-code)

Demo06b - daemon example MSAL (https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-v2-python-daemon#more-information) 

Demo06c - full example (https://github.com/Azure-Samples/ms-identity-python-daemon/tree/master/1-Call-MsGraph-WithSecret)
- acquire token
- use the token to get user profile from MS Graph API

**Note:** The subscription will end in 4 days

Move resources to another subscription (https://medium.com/@calloncampbell/moving-your-azure-resources-to-another-subscription-or-resource-group-1644f43d2e07)
(https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/move-resource-group-and-subscription)

## July 15

Lab02 - procedure for Azure Functions in Python using VS Code (https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-python)

- download node.js (64-bit), select **chocolatey** option
- This requires a restart of the Lab VM
- Check installation.  Az, Python, npm, choco
- Debug locally. This will ask to install _Azure Functions Core Tools_

Lab02 - complete, files in Onedrive

Demo06 - Graph API (https://docs.microsoft.com/en-US/graph/api/user-get?view=graph-rest-1.0&tabs=http)

Demo11 - complete, files in Onedrive

Suggest this ink for installing Python (https://docs.microsoft.com/en-us/azure/developer/python/configure-local-development-environment?tabs=cmd)


## July 16

Demo06b - another example (https://docs.microsoft.com/en-us/azure/active-directory/develop/scenario-desktop-acquire-token?tabs=python)

MSAL Python doesn't provide an interactive acquire token method directly. Instead, it requires the application to send an authorization request in its implementation of the user interaction flow to obtain an authorization code. This code can then be passed to the acquire_token_by_authorization_code method to get the token.

Lab05 - Python program to display host name and IP address

(https://www.geeksforgeeks.org/display-hostname-ip-address-python/)

(https://www.geeksforgeeks.org/python-program-find-ip-address/)

Access Python and AzureRM from the Cloud Shell
(https://msftstack.wordpress.com/2017/07/30/azure-cloud-shell-python-and-container-instances/)

>Note: The latest CLI versions now also include a command to display the current authentication token. Try:
    ```
    az account get-access-token
    ```

Suggested order:
- Lab05
- Demo04

Wait for feedback on Demo06 - here it is!

First question (ref Module 01a (Python): fine to go with a single/simple app. Goal of the lab doesn’t require complexity.

Second question (ref demo for Module 6 = MSAL and Graph API): Feel free to use the 2nd technique, just want something that will work.

Good ref for Lab05 (https://www.wintellect.com/containerize-python-app-5-minutes/)

Lab05 - This worked (https://runnable.com/docker/python/dockerize-your-python-application)

## July 17

Got Demo04 working based on (https://docs.microsoft.com/en-us/azure/cosmos-db/create-sql-api-python#review-the-code)

(https://github.com/Azure-Samples/azure-cosmos-db-python-getting-started/blob/master/cosmos_get_started.py)

Reminder - all samples for Azure and Python (https://github.com/Azure-Samples?q=&type=&language=python)

Demo06 based on (https://github.com/Azure-Samples/ms-identity-python-daemon)

## May 6, 2021

List of Azure Functions written in Python
(https://github.com/Azure-Samples/functions-docs-python)
