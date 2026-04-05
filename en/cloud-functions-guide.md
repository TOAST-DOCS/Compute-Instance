## Compute > Cloud Functions > Console Guide
This document describes how to create and manage functions in the Cloud Functions console.

## Function Management
You can create, modify, delete, and copy functions.

### Create Function
After configuring function settings, writing code, and building it, click the **Create** button to create a function with the last built package.

![console-guide-07](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_functions/2025-07-29/console-guide-07.png)
![console-guide-08](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_functions/2025-07-29/console-guide-08.png)

#### Function Settings
<table class="it">
    <tr>
        <th>Category</th>
        <th>No.</th>
        <th>Item</th>
        <th>Description</th>
    </tr>
    <tr>
        <td rowspan="2">Basic Information</td>
        <td>1.</td>
        <td>Name</td>
        <td>Name of the function<br> Duplicate function names are not allowed.<br> Used as the function endpoint URL.</td>
    </tr>
    <tr>
        <td>2.</td>
        <td>Description</td>
        <td>Description of the function, up to 250 characters</td>
    </tr>
    <tr>
        <td rowspan="7">Function Settings</td>
        <td>3.</td>
        <td>Endpoint URL</td>
        <td>HTTP Endpoint URL for function invocation<br>Automatically updated when the function name is entered.</td>
    </tr>
    <tr>
        <td>4.</td>
        <td>Type</td>
        <td>Pool Manager<br> New Deployment</td>
    </tr>
    <tr>
        <td>5.</td>
        <td>Resource</td>
        <td>Select the resource for the function execution environment<br>For Pool Manager type, only designated resources can be selected<br>For New Deployment type, custom resources can be entered</td>
    </tr>
    <tr>
        <td>6.</td>
        <td>Execution timeout</td>
        <td>Specifies the function execution time (timeout). If the function exceeds the specified time after execution, it is forcefully terminated and returns an error response</td>
    </tr>
    <tr>
        <td>7.</td>
        <td>(Pool Manager) Concurrent execution settings</td>
        <td>Specifies the number of concurrent executions for the function. When the function is called concurrently, instances are executed simultaneously for each invocation, and the maximum number of instances is limited.</td>
    </tr>
    <tr>
        <td>8.</td>
        <td>(New Deployment) Minimum number of instances</td>
        <td>Specifies the minimum number of instances to maintain.</td>
    </tr>
    <tr>
        <td>9.</td>
        <td>(New Deployment) Maximum number of instances</td>
        <td>Specifies the maximum number of instances that can be automatically scaled.</td>
    </tr>
    <tr>
        <td>Log Settings</td>
        <td>10.</td>
        <td>Log service integration</td>
        <td>Select whether to integrate using the Log & Crash Search service<br>Function logs can be checked directly in the Log & Crash Search service.<br>Logs are sent in units of 10 lines.</td>
    </tr>
</table>

> **[Note]** <br>
> **(Pool Manager) Concurrent execution settings**


![console-guide-05](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_functions/2025-07-29/console-guide-05.png)

<br>

> **[Note]** <br>
> **(New Deployment) Number of instances** - Depending on resource usage, the specified number of instances may not be created.

<br>


#### Writing Code

![console-guide-09](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_functions/2025-07-29/console-guide-09.png)
![console-guide-10](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_functions/2025-07-29/console-guide-10.png)
![console-guide-11](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_functions/2025-07-29/console-guide-11.png)

<table class="it">
    <tr>
        <th>Category</th>
        <th>No.</th>
        <th>Item</th>
        <th>Description</th>
    </tr>
    <tr>
        <td rowspan="2">Source Code</td>
        <td>1.</td>
        <td>Runtime environment</td>
        <td>Select the runtime environment for the function</td>
    </tr>
    <tr>
        <td>2.</td>
        <td>Entry Point</td>
        <td>Specifies the entry point of the function. Example: function name <br>Automatically completed according to the runtime environment template.<br>If modified manually, it must match the Entry Point of the written source code.<br>If entered incorrectly, <strong>it cannot be verified through building</strong>, but <strong>can be verified through logs during testing</strong>.</td>
    </tr>
    <tr>
        <td rowspan="3">Code</td>
        <td>3.</td>
        <td>Code editor</td>
        <td>The runtime environment template files are loaded, and you can write the function by modifying those files<br>Adding directories in the code editor is not possible. To edit the directory structure, download the template files, modify them locally, and then upload them as a ZIP file.</td>
    </tr>
    <tr>
        <td>4.</td>
        <td>User local environment</td>
        <td>Write function code in your local environment and upload it as a ZIP file</td>
    </tr>
    <tr>
        <td>5.</td>
        <td>Build</td>
        <td>Builds the code written or uploaded by the user to create a package.<br>The built package is used for testing, and the last built package is linked as the function version when the function is created/modified.</td>
    </tr>
    <tr>
        <td rowspan="2">Test</td>
        <td>6.</td>
        <td>Test event</td>
        <td>Write the JSON Body to be passed to the function</td>
    </tr>
    <tr>
        <td>7.</td>
        <td>Test</td>
        <td>Tests the function by passing the JSON Body written in the event. (Build must be performed first.) <br>You can verify function behavior through logs. <br>Invoked with the GET method.</td>
    </tr>
    <tr>
        <td></td>
        <td>8.</td>
        <td>Create</td>
        <td>Creates a function according to the function settings and written code using the Create button</td>
    </tr>
</table>

<br>

> **[Note]** <br> The package generated through the Build button is linked as the function version with the last built package when the function is created/modified. At least one build must be performed before creating a function.

### Modify Function
Click the **Modify** button to modify the function settings and code of an existing function.
#### Non-modifiable Items
- Name, Runtime environment
    - All items except these can be modified.
#### Source Code
- When using the code editor, the existing code is loaded.
- If the function was created by uploading a ZIP file from the user local environment, switching to the code editor will not display the ZIP file and will load the default template code instead.

### Delete Function
![console-guide-14](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_functions/2025-07-29/console-guide-14.png)
Select and delete existing functions. Multiple functions can be deleted at once.

### Copy Function
![console-guide-13](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_functions/2025-07-29/console-guide-13.png)
Copies an existing function to create an identical one. Since duplicate names are not allowed, you can enter a new name before copying.
- Triggers are not copied. (HTTP trigger is provided by default)
- Only the currently applied version is copied.

## Function Information
### Function List
![console-guide-01](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_functions/2025-07-29/console-guide-01.png)
- You can view the list of functions created by the user.
- The build status displays the build status of the current version.
### Function Basic Information
![console-guide-02](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_functions/2025-07-29/console-guide-02.png)
- You can view the basic information of the function.
- In the log management item, click the **Log & Crash Search** button to navigate to the Log & Crash Search service and check logs.

### Function Version Management
![console-guide-15](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_functions/2026-01-27/console-guide-15.png)
- You can manage the versions of a function.
- You can view the history of all created versions and roll back to a previous version.

#### Version Management Overview
Clicking the **Build** button on the function creation/modification screen builds the code and generates a package.
- The last built package is linked as the function version when the function is created/modified.
- Each version is managed independently, and a tested version can be applied to the function as-is.
- At least one build must be performed before a function can be created.

#### Version Information
<table class="it">
    <tr>
        <th>No.</th>
        <th>Item</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>1.</td>
        <td>Version list</td>
        <td>You can view the complete version history of the function.<br>Information such as version name, runtime, source code, and build status is displayed.</td>
    </tr>
    <tr>
        <td>2.</td>
        <td>Currently applied version</td>
        <td>The version currently applied to the function is displayed.</td>
    </tr>
    <tr>
        <td>3.</td>
        <td>Source code download</td>
        <td>You can download the source code of the selected version as a ZIP file.</td>
    </tr>
    <tr>
        <td>4.</td>
        <td>Check logs</td>
        <td>You can view the build log of the selected version.<br>This is the same as the build log check feature in the Basic Information tab.</td>
    </tr>
</table>

#### Version Deployment
- Select the version to deploy from the version list and click the **Deploy Version** button to update the function to that version.
- The currently applied version cannot be selected.
- Multiple versions cannot be selected simultaneously; only one version can be deployed at a time.
- Versions with failed builds can also be deployed.

> **[Note]**
> <br>A confirmation popup is displayed when deploying a version, and upon success, the version list is automatically refreshed.

#### Delete Version
- Select the version to delete from the version list and click the **Delete Version** button to delete that version.
- The currently applied version cannot be deleted.
- Multiple versions can be selected and deleted at once.
- When a version is deleted, the source code of that version is also deleted.

> **[Note]**
> <br>A confirmation popup is displayed when deleting a version, and deleted versions cannot be recovered.

#### Constraints
- Packages built on the function creation/modification screen are not linked as function versions if the creation/modification is canceled.
- When a function is deleted, all versions linked to that function are also deleted.
- When copying a function, only the currently applied version of the original function is copied; the entire version history is not copied.
- When creating a function, you can build with multiple runtimes, but when modifying a function, you can only build with the runtime selected at the time of creation.

### Function Trigger Management
- You can manage triggers that execute a function.
- An HTTP trigger is provided by default when a function is created.
    - You can configure whether to use it by enabling/disabling it.

![console-guid-12](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_functions/2025-07-29/console-guide-12.png)

- You can execute the created function through the provided HTTP trigger.
    - Example: `https://{userdomain}/{function name}`
    - Method: GET, POST

#### Create/Modify Trigger
- Timer
    - Value: Enter the cycle as a Cron string.
- API Gateway
    - You can add an HTTP Endpoint using the API Gateway service.

#### Delete Trigger
- Multiple triggers can be selected and deleted. The default HTTP trigger cannot be deleted.

### Function Monitoring
![console-guide-06](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_functions/2025-07-29/console-guide-06.png)
- You can view the usage of a function.
- Provides metrics for function invocation count, rejected invocation count, error occurrence count, success rate, and function execution time.
- Provides metrics within the configured time range.
- If logs are integrated, you can navigate to the Log & Crash Search service using the Log & Crash Search button.

| Item | Description |
| --- | --- |
| Function invocation count | Total number of function invocations (per second) |
| Rejected invocation count | Number of failed function invocations due to instances not being created (per second) |
| Error occurrence count | Number of responses with a response code other than 200 (per second) |
| Success rate | Ratio of successful invocations to total function invocations |
| Function execution time | Response time for function invocations |

> **[Note]**
> <br>Rejected invocation count applies only to the Pool Manager type.
> <br>Function invocation count, rejected invocation count, and error occurrence count are displayed as the average count per second within the metric step range. For example, if the step is 15 seconds and 1 event occurred during that period, it is displayed as 0.0667 (1÷15).
> <br>The average of function execution time is the overall cumulative average, and the maximum value is the maximum execution time during that period.