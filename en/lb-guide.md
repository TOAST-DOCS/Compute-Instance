## Network > Load Balancer > Console Guide

<a id='manage-loadbalancers'></a>

## Manage Load Balancers

<a id='create-loadbalancers'></a>
### Create Load Balancers
You can easily create a load balancer by simply entering the load balancer settings in the NHN Cloud Console. Depending on your purpose, you can select either L4 Routing or L7 Routing mode when creating a load balancer. <br>
The mode only refers to a template and does not determine the actual type of the load balancer. You can create a load balancer in L4 Routing mode and add L7 rules later.

* L4 Routing: A load balancer that performs load balancing based on IP and port. After creation, you can add L7 rules to convert it into a Layer7 load balancer.
* L7 Routing: A load balancer that performs load balancing based on L7 data.

#### Load Balancer Settings
Configure the basic information for the load balancer. The required items are as follows.

* Name: Enter the name of the load balancer.
* Description: Enter a description of the load balancer.
* Type: You can select either General or Dedicated.
* Network (Subnet): Specify the subnet of the VPC to which the load balancer will be connected. If you select **Auto Assign**, an available private IP within the subnet range will be assigned to the load balancer. You can select **Specify** to assign a desired private IP to the load balancer.
* Subnet Static Route: Select whether to apply the static route settings configured on the subnet to which the load balancer belongs.


#### Set up Listeners

Define the properties of the traffic that the load balancer will handle. Load balancers in NHN Cloud can have one or more listeners.

* Name: Enter the name of the listener.
* Description: Enter a description of the listener.
* Protocol: Specify the protocol of the traffic that the load balancer will handle. Select one of TCP/HTTP/HTTPS/TERMINATED_HTTPS.
* Load Balancer Port: Specify the port on which the default listener will receive traffic.
* Default Member Group: Specify the member group to which traffic will be distributed by default when received. You can set the listener's default member group to **Disabled**. If there are no L7 rules, or if existing L7 rules do not match the conditions and load balancing is determined as **Disabled**, the request will return 503.
* Connection limit: Specify the number of TCP sessions that the default listener can maintain simultaneously. A general load balancer can be set up to a maximum of 60,000, and a dedicated load balancer can be set up to a maximum of 480,000.
* Keep-Alive timeout: Specify the session keep-alive time with clients and servers in seconds. The load balancer maintains the session for this duration as long as the other party maintains the session. It is recommended to set the Keep-Alive timeout value configured on the server here. The default value is set to 300 seconds.
* Proxy Protocol: You can enable the load balancer to support the proxy protocol. This value should only be enabled when the server is configured to use the proxy protocol to obtain the client's IP. This is only available when using TCP and HTTPS protocols.
* Block invalid requests: If set to **Disabled**, requests containing invalid characters in the HTTP request header will be blocked. This is only available when using HTTP and TERMINATED_HTTPS protocols.

* SSL Certificate: When TERMINATED_HTTPS is selected as the protocol, register the certificate to use.

> [Caution] The load balancer port and protocol cannot be changed after the listener is created.

> [Caution] You can set the listener's default member group to **Disabled**. If there are no L7 rules, or if existing L7 rules do not match the conditions and load balancing is determined as **Disabled**, the request will return 503.

> [Note] The load balancer port accepts values between 1 and 65535.

> [Note] Health checks are performed only when a member group is assigned as a listener's default member group or designated as the action target of an L7 rule. Otherwise, health checks are not performed for that member group.

> [Note] How to register TERMINATED_HTTPS certificates
>
> When the listener protocol of a load balancer is set to TERMINATED_HTTPS, the button to register an SSL certificate is enabled.
>
> The files to register are 'Certificate' and 'Private Key'. The 'Private Key' refers to the private key paired with the public key embedded in the server certificate.
>
> The 'Certificate' follows the x.509 PEM format as follows.
>
>     -----BEGIN CERTIFICATE-----
>     (Content omitted)
>     -----END CERTIFICATE-----
>
>
> When you need to register the server certificate along with a chain certificate (Chain Certificate, Intermediate Certificate), you must combine the server certificate and chain certificate into a single file for registration.
>
> When creating a single certificate file, the server certificate must be placed at the top of the file, followed by the chain certificates below. Chain certificates can be listed in any order.
>
> When combining 1 server certificate and 2 chain certificates into a single certificate file, the format is as follows.
>
>
>      -----BEGIN CERTIFICATE-----
>      (Server certificate content omitted)
>      -----END CERTIFICATE-----
>      -----BEGIN CERTIFICATE-----
>      (Chain certificate #1 content omitted)
>      -----END CERTIFICATE-----
>      -----BEGIN CERTIFICATE-----
>      (Chain certificate #2 content omitted)
>      -----END CERTIFICATE-----
>
>
>
> The 'Private Key' is the key file corresponding to the public key included in the server certificate. The registered 'Private Key' must have its password removed to function properly.
>
> Files in PKCS#1 or PKCS#8 PEM format can be registered.
>
>
>
>      -----BEGIN RSA PRIVATE KEY-----
>      (Private key content omitted)
>      -----END RSA PRIVATE KEY-----
>
> or
>
>      -----BEGIN PRIVATE KEY-----
>      (Private key content omitted)
>      -----END PRIVATE KEY-----


##### Using Certificate Manager
When using the TERMINATED_HTTPS protocol for a listener, there are two methods to register a certificate: using a certificate registered in Certificate Manager, or registering it directly.

* By registering a certificate in the Certificate Manager service and linking it to the listener, you can receive certificate expiration alerts via email.
* If you register a certificate directly on the listener, there are no certificate expiration alerts. However, you can check the expiration date on the listener screen in the Console.
> [Caution]
> When a certificate is renewed in the Certificate Manager service, the certificates of affected listeners must also be renewed.
> To use a certificate registered in Certificate Manager for a listener, the password of the 'Private Key' must be removed, and it must be in PKCS#1 or PKCS#8 PEM format.


##### Set up L7 Rules
The load balancer can perform load balancing based on L7 data. When creating a load balancer by selecting the L7 Routing template, you can create a load balancer with L7 policies included. L7 policies only work properly when the listener's protocol is HTTP/TERMINATED_HTTPS. Even if you create a load balancer with the L4 template, you can add L7 rules later.

* Name: Enter the name of the L7 rule.
* Description: Enter a description of the L7 rule.
* Action Type: Specify the action to perform when an L7 rule matches.
  * Forward to Member Group: When an L7 rule matches, traffic is sent to the configured member group. This allows routing packets to a specific member group based on L7 data.
  * Redirect to URL: When an L7 rule matches, traffic is redirected to the configured URL. The redirection is performed using the Location in the HTTP header.
    * For **Redirect to URL**, you can configure the redirect URL in detail. You can retain or manually change values for each of the protocol, port, host, path, and query. For more details, see the [Note] below.
    * Status Code: The HTTP response code that the load balancer will respond with during redirection. 301 and 302 are supported.
  * Block: When an L7 rule matches, the request is blocked. A Forbidden (403) response is returned.
* Action Target: Configure the target for the action based on the action type. The input method varies depending on the action type.
* Action Priority: Set the priority of the L7 rule. The priority within the action type is determined based on the entered value, and if duplicate values are entered, the new rule takes precedence.
  * Rules are applied in the following order: **Block**, **Redirect to URL**, **Forward to Member Group**. Within the same action type, the priority entered by the user is applied.
  * The actual application order of rules can be easily identified through the **Rule Application Order** column in the L7 rules table.
  * Each time an L7 rule is added or modified, the action priorities entered by the user are internally re-sorted to determine the priority.
  * The action priority displayed in **Detail View** represents the relative value of the internally re-sorted priority, not the absolute value originally entered by the user.
  * When adding or modifying L7 rules later, priorities must be set based on the relative values of the internally re-sorted priorities.
* Conditions: Describe the conditions to apply to the L7 rule. Up to 10 conditions can be created per L7 rule.
  * Condition Type: Condition types support path, header, file type, cookie, and hostname.
    * Path: Inspects the value of the URL path.
    * Header: Inspects fields contained in the HTTP header. A header field name must be additionally entered.
    * File Type: Inspects the ending value of the URL path. This is useful for matching file extensions.
    * Cookie: Inspects the cookie field in the HTTP request header. A cookie key must be additionally entered.
    * Hostname: Inspects the host field in the HTTP request header.
  * Comparison Method: Depending on the condition type, you can select from CONTAINS/EQUAL_TO/STARTS_WITH/ENDS_WITH/REGEX.
    * CONTAINS: True if the condition type string contains the entered value.
    * EQUAL_TO: True if the condition type string matches the entered value exactly.
    * STARTS_WITH: True if the condition type string starts with the entered value.
    * ENDS_WITH: True if the condition type string ends with the entered value.
    * REGEX: True if the condition type string matches the entered regular expression syntax.
  * Value: Enter the string you want to match. If the condition type is header or cookie, a key must be additionally entered.

> [Caution] Among condition types, hostname is case-insensitive.

> [Note] If no configured L7 rules are matched, traffic is forwarded to the listener's default member group.

> [Note] Health checks are performed only when a member group is assigned as a listener's default member group or designated as the action target of an L7 rule. Otherwise, health checks are not performed for that member group.

> [Note] When a member group is deleted, L7 rules that had the member group as their action target will have their action type changed to Block.

> [Note] When setting **Redirect to URL** in an L7 rule, you can retain or directly specify some values of the URL to redirect to. If you want to retain the value of a specific field, enter it in the format `#{protocol}`, `#{port}`, `#{host}`, `#{path}`, `#{query}` in each URI part setting field. For example, if you want to change only the protocol and port number to HTTPS and port 443 for requests coming in over HTTP, enter `HTTPS` for protocol, `443` for port, `#{host}` for host, `#{path}` for path, and `#{query}` for query.

> [Caution] If the redirect URL is entered in an incorrect format, the redirect URL may be converted to a value different from the actual input.



#### Set up Member Groups
Configure the target member groups to which load-balanced traffic will be forwarded. Member groups can also be additionally created after the load balancer creation is complete.

* Name: Enter the name of the member group.
* Description: Enter a description of the member group.
* Protocol: Specify the protocol of the traffic that the member group will handle. Select one of HTTP/HTTPS/TCP.
* Member Port: Specify the port on which the member group will receive traffic.
* Load Balancing Method: Determine how the load balancer distributes traffic. Select one of ROUND_ROBIN/LEAST_CONNECTIONS/SOURCE_IP.
* Session Persistence: A setting that ensures responses to requests are handled by a specific instance to maintain the session. You can select one of No Session Persistence/APP_COOKIE/HTTP_COOKIE/SOURCE_IP.


> [Caution] The member port and protocol cannot be changed after the member group is created.

> [Note] The member port accepts values between 1 and 65535.


##### Health Check

Health check settings are also determined when creating a member group. Load balancers in NHN Cloud can define health check behavior for each member group. The required items are as follows.

* Health Check Protocol: Determine the protocol to use for health checks. Select one of TCP/HTTP/HTTPS.
* Health Check Port: Set the member port on which health checks will be attempted. If you select the member port, health checks are performed on the port number assigned to each member. If you select custom, health checks are performed uniformly on the custom port number regardless of each member's port number.
* HTTP Method: Select the HTTP method to use for health checks. This is enabled only when HTTP or HTTPS is selected as the health check protocol. Currently, only GET is supported.
* HTTP Status Code: Enter the HTTP status code to be considered a normal response during health checks. This is enabled only when HTTP or HTTPS is selected as the health check protocol. Currently, only GET is supported.
* URL: Specify the path of the member to attempt health checks on. This is enabled only when HTTP or HTTPS is selected as the health check protocol.
* Health Check Cycle: Enter the health check interval. The unit is seconds, and health checks are attempted at the specified interval.
* Maximum Wait Time for Response: Specify the maximum time to wait for a normal response after a health check. The unit is seconds, and if the specified wait time is exceeded, it is considered a failure.
* Maximum Number of Retries: Specify the maximum number of retry attempts for health checks. If the maximum number of retries is 2 or more, a failure is not immediately assumed just because a normal response to the health check was not received. If failures occur repeatedly up to the maximum number of retries, the instance is excluded from load balancing targets.
* Host Header: Enter the field value to use in the host header during health checks. This is enabled only when HTTP or HTTPS is selected as the health check protocol.

> [Caution] When multiple members with different port numbers exist within a member group, be cautious with the health check port setting. For example, assume there are 2 members: port 80 on 192.168.0.10 and port 8080 on 192.168.0.10. If the health check port is set to the member port, health checks are performed on both port 80 and port 8080 individually. However, if the health check port is set to custom and 80 is entered, port 80 is checked even for the member with port 8080. If port 80 on 192.168.0.10 is active, the member with port 8080 on 192.168.0.10 is also considered ACTIVE because it checks the status of port 80 on 192.168.0.10.

> [Note] Health checks are performed only when a member group is assigned as a listener's default member group or designated as the action target of an L7 rule. Otherwise, health checks are not performed for that member group.


##### Set up Members
Specify the instances or IPs to register as members when creating the load balancer. Members can also be registered after the load balancer is created. Members can be registered in two ways.

* Instance: Instances belonging to the VPC connected to the load balancer and VPCs peered with that VPC can be added as members. However, to register an instance on a different subnet from the load balancer as a member, both subnets must be registered in the routing table.
* IP Address: Members can be registered by directly entering an IP address. In this case, the communication path between the load balancer and the IP must be properly configured.

#### Delete Protection
Enabling delete protection prevents the load balancer from being accidentally deleted. The load balancer cannot be deleted until delete protection is disabled. A load balancer with delete protection enabled cannot have its listeners, member groups, or L7 rules deleted. A load balancer with delete protection enabled cannot have its health monitor deleted or modified.

#### IP Access Control Group
Specify the IP access control group to apply when creating the load balancer. You can select multiple groups with the same access control type from the IP access control groups. The applied IP access control group can be changed even after the load balancer is created.

<a id='view-loadbalancers'></a>
### Viewing Load Balancers
After creating the load balancer, you will be returned to the load balancer list screen. On the load balancer list screen, you can check the basic information of the created load balancers. The items displayed on the list screen are as follows.

* Name: The name of the load balancer specified during creation.
* Type: The load balancer type.
* IP Address: The private IP assigned from the VPC connected to the load balancer. Within the VPC, you can access the load balancer through this IP. To access the load balancer from outside the VPC, a floating IP must be associated.
* Network: The name of the VPC connected to the load balancer and the subnet CIDR.
* Status: Indicates the creation status of the load balancer. If it shows ACTIVE, it has been created successfully.
* Load Balancer Details: You can check the details of listeners and member groups connected to the load balancer.

> [Note] The creation status of a load balancer is determined as one of the following.

> | Status | Meaning |
> |--|--|
> | ACTIVE | Load balancer creation complete, operating normally |
> | PENDING_CREATE | Load balancer being created <br> If the status does not change to ACTIVE within one hour after creation, please contact the administrator.|
> | PENDING_UPDATE | Load balancer settings being modified<br> If the status does not change to ACTIVE within one hour after modification, please contact the administrator.|
> | PENDING_DELETE | Load balancer being deleted<br> If it does not disappear from the list within one hour after deletion, please contact the administrator.|
> | ERROR | Load balancer creation failed <br> Please contact the administrator. |
> | ERROR_MIGRATE | Load balancer migration failed <br> Please contact the administrator. |

<a id='modify-loadbalancers'></a>
### Modifying Load Balancers and Basic Information
When you select a load balancer from the load balancer list screen, additional information about the selected load balancer is displayed at the bottom of the screen. The detail screen is divided into two tabs. The description of each tab is as follows.

* Basic Information: Shows additional basic information about the selected load balancer. You can change the name, description, and subnet static route application setting of the selected load balancer.
* Statistics: You can view statistical information about the selected load balancer.

> [Note] The VPC and IP address connected to the load balancer cannot be changed.

<a id='change-listener'></a>
### Changing Listeners and Details
When you select the detail view of the desired load balancer from the load balancer main screen, you can check the listeners and member groups connected to the load balancer. Select the **Listener** tab to create, modify, or delete listeners.

#### Create Listener
Click **Create Listener** to create an additional listener. The items required for adding a listener are the same as those required for the default listener when creating a load balancer. When adding a listener, you cannot use load balancer ports that are already in use by existing listeners.

#### Modify Listener
Click **Modify Listener** on the listener you want to modify to change the listener settings.

> [Caution] The listener's port and protocol cannot be changed.

#### Certificate Management
For listeners using the TERMINATED_HTTPS protocol, you can manage multiple certificates in the **Certificate** tab on the listener detail screen.

##### Retrieve Certificates
1. Click the **Listener** tab on the load balancer detail screen.
2. Select the TERMINATED_HTTPS listener for which you want to manage certificates.
3. Click the **Certificate** tab on the listener detail screen.
4. You can check the following information in the registered certificate list.
   - **Name**: Certificate name
   - **Expiration date**: Certificate expiration date and number of days remaining until expiration

##### Add Certificate
1. Click the **+ Add Certificate** button in the **Certificate** tab on the listener detail screen.
2. Select whether to use Certificate Manager.
   - **Use**: You can select and add a certificate from the list of certificates registered in Certificate Manager.
   - **Disabled**: You can register by directly uploading a certificate file and private key file.
3. Review the warning message, select the checkbox, and click **Confirm**.

> [Caution] When adding a certificate, the load balancer will restart. During the restart, existing connected sessions are maintained, but new sessions cannot be processed (less than approximately 1 second). Therefore, it is recommended to perform this during a time that does not affect services.

##### Change Default Certificate
1. Click the **Change Default Certificate** button in the **Certificate** tab on the listener detail screen.
2. Select the certificate to use as the default certificate.
3. Review the warning message, select the checkbox, and click **Confirm**.

> [Caution] When changing the default certificate, the load balancer will restart. During the restart, existing connected sessions are maintained, but new sessions cannot be processed (less than approximately 1 second). Therefore, it is recommended to perform this during a time that does not affect services.

##### Delete Certificate
1. Select the certificate to delete in the **Certificate** tab on the listener detail screen.
2. Click the **Delete Certificate** button.
3. Review the warning message, select the checkbox, and click **Confirm**.

> [Caution] The default certificate cannot be deleted. To delete the default certificate, you must first change another certificate to the default certificate and then delete it.

> [Caution] When deleting a certificate, the load balancer will restart. During the restart, existing connected sessions are maintained, but new sessions cannot be processed (less than approximately 1 second). Therefore, it is recommended to perform this during a time that does not affect services.

<a id='custom-response-guide'></a>
### Custom Response Guide

You can configure custom responses on the load balancer's listener. By using custom responses, you can deliver a desired custom message or HTML content directly to users when a specific HTTP error code occurs.

#### Retrieve and Configure Custom Responses

1. Click the **Listener** tab on the load balancer detail screen.
2. Select the listener for which you want to configure custom responses.
3. Click **View/Modify Custom Response Settings** on the listener detail screen.
4. You can enter and review the following items.
   - **Response Code**: Select the HTTP status code to which the custom response will be applied. (Supported codes: 400, 403, 408, 500, 502, 503, 504)
   - **Content Type**: Select the Content-Type of the response to be delivered to the user. (Select from `text/html`, `text/plain`, `application/json`, `application/javascript`, `text/css`)
   - **Response Body**: Enter the body of the response to be shown to the user. (Maximum 1024 characters; the content can be freely written as HTML, text, etc. depending on the content type)
5. After entering each item, click **Confirm** to create the response. Created responses can be viewed in the list.

#### Delete Custom Responses

- Created custom responses can be deleted by selecting the desired item from the list and clicking the **Delete** button.
- When an error code corresponding to a deleted response occurs, the default system response will be displayed.

> [Note] Within the same listener, each error code can be registered as a custom response only once.

> [Caution] When adding, modifying, or deleting custom responses, the listener may briefly restart (less than 1 second). It is recommended to perform changes during a time with minimal service impact.

<a id='x-forwarded-header-guide'></a>
### X-Forwarded Header Settings Guide

You can view and modify the X-Forwarded header settings on the load balancer's listener. X-Forwarded headers are used to pass the client's original information (protocol, port, IP address) to backend servers.

#### Retrieve and Configure X-Forwarded Headers

1. Click the **Listener** tab on the load balancer detail screen.
2. Select the listener for which you want to configure X-Forwarded headers.
3. Click the **View/Modify Settings** button for the **X-Forwarded Header** item in the **Basic Information** tab on the listener detail screen.
4. You can configure the following items.
   - **X-Forwarded-Proto**: Configure whether to pass the protocol (http or https) used by the client to the backend server. Select either **Use** or **Disabled**.
   - **X-Forwarded-Port**: Configure whether to pass the port number connected by the client to the backend server. Select either **Use** or **Disabled**.
   - **X-Forwarded-for**: Configure whether to pass the client's original IP address to the backend server. Select either **Use** or **Disabled**.
5. After configuring each item, select the warning message checkbox and click **Confirm** to apply the settings.

> [Caution] Changing the X-Forwarded header settings will restart the load balancer. During the restart, existing connected sessions are maintained, but new sessions cannot be processed (less than approximately 1 second). Therefore, it is recommended to perform this during a time that does not affect services.

#### Delete Listener
Click **Delete Listener** on the listener you want to delete to remove it.

> [Caution] When creating/modifying/deleting a listener, the load balancer will restart. During the restart, existing connected sessions are maintained, but new sessions cannot be processed (less than approximately 1 second). Therefore, it is recommended to perform this during a time that does not affect services.

<a id='change-member-group'></a>
### Changing Member Groups and Details
When you select **Detail View** of the desired load balancer from the load balancer screen, you can check the listeners and member groups connected to the load balancer. Select the **Member Group** tab to create, modify, or delete member groups.

#### Create Member Group
Click **Create Member Group** to create an additional member group. The items required for creating a member group are the same as those required for the member group when creating a load balancer.

#### Modify Member Group
Click **Modify Member Group** to change the settings related to the member group.

> [Caution] The member port and protocol cannot be changed after the member group is created.

#### Delete Member Group
Select the member group to delete and click **Delete Member Group** to remove it.

> [Caution] When creating/modifying/deleting a member group, the load balancer will restart. During the restart, existing connected sessions are maintained, but new sessions cannot be processed (less than approximately 1 second). Therefore, it is recommended to perform this during a time that does not affect services.

> [Note] When a member group is deleted, L7 rules that had the member group as their action target will have their action type changed to Block.

<a id='change-member'></a>
### Changing Members and Details
On the load balancer **Detail View** screen, select the **Member Group** tab, then select the desired member group to check the member group's details and the status of members belonging to the member group.

#### Add Member
When a member group is selected, the **Basic Information**, **Member**, and **Health Check** tabs are displayed at the bottom of the screen. Select the **Member** tab to register the desired instances or IP addresses as members. Only instances belonging to the VPC connected to the load balancer and VPCs peered with that VPC can be added. You can directly specify the destination port number for each member, and load balancing is performed on that destination port number.

> [Caution] When multiple members with different port numbers exist within a member group, be cautious with the health check port setting. For example, assume there are 2 members: port 80 on 192.168.0.10 and port 8080 on 192.168.0.10. If the health check port is set to the member port, health checks are performed on both port 80 and port 8080 individually. However, if the health check port is set to custom and 80 is entered, port 80 is checked even for the member with port 8080. If port 80 on 192.168.0.10 is active, the member with port 8080 on 192.168.0.10 is also considered ACTIVE because it checks the status of port 80 on 192.168.0.10.

#### Disable Member
You can temporarily exclude a specific member from the service. Select the member to exclude, click the **Disable Member** button, and then click **Confirm**.
The excluded member's usage item will change to **X**, and the member status will change to **ONLINE**.

> [Note] The status of a member is determined as one of the following.
> 
> | Status | Meaning |
> |--|--|
> | ACTIVE | Member connection complete, operating normally |
> | INACTIVE | Health check is not being performed for the member |
> | ONLINE | Member is disabled|
> | OFFLINE | Member connection failed <br> Please contact the administrator.|

#### Delete Member
Members that are no longer in use can be deleted. Select the member to delete and click **Delete Member** to remove it. Even if deleted from the load balancer's members, the instance itself is not deleted.

> [Caution] When adding/disabling/deleting a member, the load balancer will restart. During the restart, existing connected sessions are maintained, but new sessions cannot be processed (less than approximately 1 second). Therefore, it is recommended to perform this during a time that does not affect services.

<a id='delete-loadbalancers'></a>
### Delete Load Balancers
Select the load balancer you want to delete from the load balancer list screen and click the delete button to delete it.


<a id='ip-acl-groups'></a>

## IP Access Control Group
For more details on the IP access control feature, see [IP Access Control](/Network/Load%20Balancer/en/overview/#ip).

<a id='create-ip-acl-groups'></a>
#### Create IP Access Control Group
To create an IP Access Control Group, click the [Create Access Control Group] button and enter the following values.

* Name: Enter the name of the access control group.
* Description: Enter a description for the access control group.
* IP Access Control Type: Select either Block or Allow.
* Add IP Access Control Target: Enter the IP address and description for the access control target. Click the "+" button on the right to add multiple access control targets at once. To make bulk input easier, you can select [Bulk Input]. In this case, enter the "IP address or CIDR" and "Description" on each line. You can enter up to 100 access control targets at a time.


Click [Confirm] to create the access control group and targets.

> [Note] Number of IP Access Control Groups and IP Access Control Targets
> 
> You can create up to 10 access control groups per project.
> You can create up to 1,000 access control targets per project.

<a id='change-ip-acl-groups'></a>
#### Modify IP Access Control Group
You can modify the properties of an IP Access Control Group. The modifiable properties are Name and Description. The "IP Access Control Type" property cannot be changed.

<a id='delete-ip-acl-groups'></a>
#### Delete IP Access Control Group
You can delete the selected IP Access Control Group. Deleting a group also deletes all access control targets belonging to the group.
When an IP Access Control Group is deleted, load balancers using this group will no longer apply the corresponding policy.

<a id='add-ip-acl-targets'></a>
#### Add IP Access Control Target
When you select an access control group, the access control target menu appears at the bottom.
When a target is added to an access control group, the policy for the added IP or CIDR is applied to all load balancers using this access control group.

<a id='change-ip-acl-targets'></a>
#### Modify IP Access Control Target
You can modify the properties of an access control target. Only the description can be modified.

<a id='delete-ip-acl-targets'></a>
#### Delete IP Access Control Target
When you select an access control group, the access control target menu appears at the bottom.
When a target belonging to an access control group is deleted, the policy for the corresponding IP or CIDR is removed from all load balancers using this access control group.

<a id='apply-ip-acl-groups'></a>
#### Apply IP Access Control Group
Select the load balancer to which you want to apply the IP Access Control Group. Select the groups to configure for the load balancer and click Confirm.
You can apply multiple groups with the same "Access Control Type" to a load balancer.

<a id='restarting-guide-for-maintenance'></a>

## Load Balancer Restart Guide for Equipment Maintenance

NHN Cloud periodically updates the software on load balancer equipment to enhance the security and stability of its underlying infrastructure services. For load balancer equipment maintenance, load balancers running on the equipment subject to maintenance must be restarted to migrate them to load balancer equipment where maintenance has been completed.

Load balancers that require a restart display a **! Restart** button next to their name, which can be used to restart them.

Navigate to the project containing the load balancer designated for maintenance and perform the restart using the following procedure.

1. Check the load balancer subject to maintenance. A load balancer with a **! Restart** button next to its name is the one subject to maintenance.
   ![image-001](http://static.toastoven.net/prod_load_balancer/lb_p_migration_ko_1.png)
   Hovering the mouse cursor over the **! Restart** button displays the detailed maintenance schedule. It is recommended to perform the restart at a time that does not affect your services.
   ![image-002](http://static.toastoven.net/prod_load_balancer/lb_p_migration_ko_2.png)
2. Select the load balancer subject to maintenance and click the **! Restart** button next to its name.
3. When a window appears asking whether to restart the load balancer, click the **Confirm** button.
   ![image-003](http://static.toastoven.net/prod_load_balancer/lb_p_migration_ko_3.png)
4. Wait until the status indicator turns green and the **! Restart** button disappears.
   If the load balancer status indicator does not change or the **! Restart** button does not disappear, try refreshing the page.
   ![image-004](http://static.toastoven.net/prod_load_balancer/lb_p_migration_ko_4.png)

While the load balancer is being restarted, no operations can be performed on it.
If the load balancer restart does not complete successfully, it is automatically reported to the administrator, and NHN Cloud will contact you separately.