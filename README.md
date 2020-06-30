# LODS
June contract

Labs we are looking at

1, 2, 5, 11

Titles are:

01 – Building a Web Application on Azure PaaS
M01/L02 demo – Create a Web App
02 – Implement Task Processing Logic by using Azure Functions
M02/L02 demo – Create an HTTP trigger
05 – Deploying Compute Workloads by using Images and Containers
M05/L01 demo – Create an Azure VM
11 – Asynchronously processing messages by using Azure Storage
M11/L01 demo – Send and receive messages from a Service Bus queue..

jun 15
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

June 21
For the rest of Lab 5 focus on Exercise 3 (less on Exercise 2)
.. deploy ane existing container image
.. use one from Docker Hub or as indicated by the code samples


June 22-28 > AWS exam

June 29

Looking at my fork of Lab 11

https://github.com/networksetcetera/AZ-204-DevelopingSolutionsforMicrosoftAzure/blob/master/Instructions/Labs/AZ-204_11_lab.md

looks like we should build it from scratch like CA video demo

This is more relevant to the "demo" which refers to **Service Bus Queues**
https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-python-how-to-use-queues

added VS Code and got it to work as an editor and for running Python code
also got venv to work
https://code.visualstudio.com/docs/python/python-on-azure

Lab 11 is focussed on **Storage Queues** so this example is more relevant
https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-queue

start with 
pip install azure-storage-queue

writing to the queue works

reading and deleting is here:
https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_message.py

Demo 11
https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/servicebus/azure-servicebus/samples

