# Demo: Create an HTTP trigger function by using the Azure portal

In this demo, you'll learn how to use Functions to create a "hello world" function in the Azure portal. This demo has two main steps:

1.  Create a Function app to host the function
2.  Create and test the HTTP trigger function

To begin, sign in to the Azure portal at [https://portal.azure.com](https://portal.azure.com) with your account.

## Create a function app

You must have a function app to host the execution of your functions. A function app lets you group functions as a logic unit for easier management, deployment, and sharing of resources.

1.  From the Azure portal menu, select **Create a resource**.

2. In the **New** page, select **Compute** > **Function App**.

3. Use the function app settings as specified in the table below.

    <table>
    <thead>
    <tr>
    <th>Setting</th>
    <th>Suggested value</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td><p><strong>Subscription</strong></p></td>
    <td><p>Your subscription</p></td>
    <td><p>The subscription under which this new function app is created.</p></td>
    </tr>
    <tr>
    <td><p><strong>Resource Group</strong></p></td>
    <td><p><em>myResourceGroup</em></p></td>
    <td><p>Name for the new resource group in which to create your function app.</p></td>
    </tr>
    <tr>
    <td><p><strong>Function App name</strong></p></td>
    <td><p>Globally unique name</p></td>
    <td><p>Name that identifies your new function app. Valid characters are <code>a-z</code> (case insensitive), <code>0-9</code>, and <code>-</code>.</p></td>
    </tr>
    <tr>
    <td><p><strong>Publish</strong></p></td>
    <td><p>Code</p></td>
    <td><p>Option to publish code files or a Docker container.</p></td>
    </tr>
    <tr>
    <td><p><strong>Runtime stack</strong></p></td>
    <td><p>Preferred language</p></td>
    <td><p>Choose a runtime that supports your favorite function programming language. Choose <strong>.NET</strong> for C# and F# functions.</p></td>
    </tr>
    <tr>
    <td><p><strong>Region</strong></p></td>
    <td><p>Preferred region</p></td>
    <td><p>Choose a region near you or near other services your functions access.</p></td>
    </tr>
    </tbody>
    </table>

<!-- 
Setting | Suggested value | Description
- | - |  -
**Subscription** | Your subscription | The subscription under which this new function app is created.
**Resource Group** | *myResourceGroup* | Name for the new resource group in which to create your function app.
**Function App name** | Globally unique name | Name that identifies your new function app. Valid characters are `a-z` (case insensitive), `0-9`, and `-`.
**Publish** | Code | Option to publish code files or a Docker container.
**Runtime stack** | Preferred language | Choose a runtime that supports your favorite function programming language. Choose **.NET** for C# and F# functions.
**Region** | Preferred region | Choose a region near you or near other services your functions access. 
-->

4. Select the **Next : Hosting >** button and enter the following settings for hosting.

    Setting | Suggested value | Description
    - | - | -
    **Storage account** | Globally unique name | Create a storage account used by your function app. You can accept the account name generated for you, or create one with a different name.
    **Operating system** | Preferred operating system | An operating system is pre-selected for you based on your runtime stack selection, but you can change the setting if necessary.
    **Plan** | Consumption plan | Hosting plan that defines how resources are allocated to your function app. In the default **Consumption Plan**, resources are added dynamically as required by your functions.

5. Select **Review + Create** to review the app configuration selections.

6. Select **Create** to provision and deploy the function app. When the deployment is complete select **Go to resource** to view your new function app.

Next, you'll create a function in the new function app.

## Create and test the HTTP triggered function

1. Expand your new function app, then select the **+** button next to **Functions**.

    ![Image showing where to select to expand the function app and add a new button](../../linked_image_files/http-trigger-function-1.png)

2. Select the **In-portal** development environment, and select **Continue**.

3. Choose **WebHook + API** and then select **Create**.

A function is created using a language-specific template for an HTTP triggered function.

### Test the function

1. In your new function, click **</> Get function URL** at the top right. 

    ![Image showing the location of the **Get function URL** button, to the right of the **Run** button.](../../linked_image_files/http-triger-copyURL.png)

2. In the dialog box that appears select **default (Function key)**, and then click **Copy**.

3. Paste the function URL into your browser's address bar. Add the query string value `&name=<yourname>` to the end of this URL and press the `Enter` key on your keyboard to execute the request. You should see the response returned by the function displayed in the browser.

4. When your function runs, trace information is written to the logs. To see the trace output from the previous execution, return to your function in the portal and click the arrow at the bottom of the screen to expand the **Logs**.

## Clean up resources

You can clean up the resources created in this demo simply by deleting the resource group that was created early in the demo.
