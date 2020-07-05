# LODS
June contract

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


Lab 02 - Functions

Changed from .NET to Python
Platform has to be Linux

also installed node.js with chocolatey option

you can choose the shell in VS Code = terminal + choose default shell

See screenshots in **images** 

https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-python

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

# July 4

The Azure CLI is written in Python
https://www.python.org/success-stories/building-an-open-source-and-cross-platform-azure-cli-with-python/
https://pypi.org/project/azure-cli/


# July 5

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

Lab01 - alternate (from PyCon 2020)
https://docs.microsoft.com/en-us/azure/developer/python/tutorial-deploy-app-service-on-linux-01

Lab05 - alternate
https://docs.microsoft.com/en-us/azure/developer/python/tutorial-deploy-containers-01

Lab02 - alternate
https://docs.microsoft.com/en-us/azure/developer/python/tutorial-vs-code-serverless-python-01
