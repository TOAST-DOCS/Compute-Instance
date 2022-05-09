## Compute > Instance > Installation Component Guide

## NAT Instance
NAT instance is an instance that allows you to access internet over a specific IP address band in the private network instance.
This feature is available only in the Pyeongchon region, Korea.

### Key Features
* The instance of a private network not connect with an internet gateway can access the internet via the NAT instance.
* Accesses the Internet by changing the floating IP of the NAT instance to source IP.
* The packets delivered to the NAT instance are delivered according to the routing setting of the routing table connected to the subnet of NAT instance.
* Do not add a route setting that specifies the NAT instance as a gateway in the routing table connected to the subnet of the NAT instance.
* The instance of a private network cannot receive the inbound traffic coming from the internet.
* Specifies IP address bandwidth of destination to allow Internet access through the NAT instance.
* Allows access to another instance by accessing the NAT instance.
* Sets security group.
* Sets network ACL.
* Redundancy not supported.
* The option of checking network source/target must be disabled in the Network Interface settings for the NAT instance to work.
* If a private image is created with NAT instance, the function might not work normally.

> [Note] Difference with NAT gateway
>
> | Classification | NAT gateway | NAT instance |
> |--|--|--|
> |Availability| Redundancy supported | Redundancy not supported |
> |Maintenance|Maintained by NHN Cloud| Directly maintained by users|
> |Security group|Not settable| Settable|Amount
> |Network ACL| Settable | Settable|
> |SSH|Unavailable| Available|

### Source/target check setting
For the NAT instance to work normally, the option of checking network source/target must be disabled in the Network Interface settings.

### Routing setting
Specifies the NAT instance as a route gateway. The packets delivered to the NAT instance are delivered according to the routing setting of the routing table connected to the subnet of NAT instance.

### Caution on settings
* Do not add a routing setting that specifies the NAT instance as a gateway in the routing table connected to the subnet of the NAT instance.
* It is strongly recommended to separate the NAT instance subnet from the subnet of an instance that is going to use the NAT instance as a gateway and use a different routing table.
* If the NAT instance subnet and the subnet of an instance that is going to use the NAT instance as a gateway are the same or they are connected to the same routing table, and you have to add a routing setting that specifies the NAT instance as a gateway, then a floating IP must be set for the NAT instance. Also, the target CIDR must be set to IP Prefix 0 (/0). Any other settings will only cause the communication to fail, and might impact the communication of all instances that use the routing table.

> [Note] Routing setting for NAT instance
>
> * If the subnet of NAT instance is connected to Routing Table 1 and the instances that are going to use the NAT instance as a gateway are connected to Routing Table 2
>     * In the routing setting of Routing Table 2, the NAT instance can be specified as a gateway for a specific CIDR (e.g. 8.8.8.8/32).
>     * You should specify the NAT instance as a gateway in the route setting of Routing Table 1 except in the case of the NAT instance connected to a floating IP, in which case IP prefix 0 (/0) can be set for the target CIDR.
> * If the NAT instance subnet and the subnet of instances that are going to use the NAT instance as a gateway are both connected to Routing Table 1
>     * If NAT instance is connected to a floating IP, IP Prefix 0 (/0) can be set for the target CIDR to route.
>     * Without using the above settings, you should not specify the NAT instance as a gateway in the routing setting of Routing Table 1.


## MS-SQL Instance

## MySQL Instance

## PostgreSQL Instance

## CUBRID Instance

## MariaDB Instance

## Tibero Instance

## JEUS Instance

The images provided by default include CentOS 7.8 with JEUS8Fix1 (Domain Administrator Server 2022.03.22) and CentOS 7.8 with JEUS8Fix1 (Managed Server 2022.03.22). To install Domain Administrator Server, use CentOS 7.8 with JEUS8Fix1 (Domain Administrator Server 2022.03.22) image. To install Managed Server, use the CentOS 7.8 with JEUS8Fix1 (Managed Server 2022.03.22) image.

JEUS is installed in `~/apps/jeus8`.

The following properties are set during installation.

| Property | Default value |
| --- | --- |
| Domain name | jeus_domain |
| WebAdmin port | 9736 |
| Admin server name | adminServer |
| Admin user ID | administrator |
| Admin user password | jeusadmin |
| Node manager | java |


### Check the Startup

To configure or control JEUS, you must start the node manager and then control it through WebAdmin or jeusadmin.

### Connect to the Instance

After the instance creation is complete, use SSH to access the instance.
The instance must have a floating IP associated and TCP port 22 (SSH) must be allowed in the security group.

### Start the Node Manager

Connect to the shell and run the node manager with the startNodeManager command.
Since the node managers need to communicate with each other, add an allow rule for the default port 7730 to the security group.

### Start JEUS

To run Domain Administrator Server, use the startDomainAdminServer command.

```
startDomainAdminServer -uadministrator -pjeusadmin
```

### JEUS WebAdmin

Run WebAdmin as follows:

1. Set a floating IP on the instance where DAS is installed.
2. Add an allow rule for port 9736 to that instance's security group.
3. If you connect to in http://{floatingIP}:9736/webadmin in a web browser, you can see the WebAdmin screen.


## WebtoB Instance

The image provided by default is CentOS 7.8 with WebtoB5Fix4 (2022.03.22).
WebtoB is installed in `~/apps/webtob`.


### Check the Startup

#### Connect to the Instance

After the instance creation is complete, use SSH to access the instance.
The instance must have a floating IP associated and TCP port 22 (SSH) must be allowed in the security group.

#### Compile the Configuration File

Compile the configuration file using the wscfl command.

```
wscfl -i http.m
```

#### Start WebtoB

Start WebtoB using wsboot.

```
wsboot
```

You can use wsadmin to view or control the status.
