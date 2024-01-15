## EC2 instance families

| Instance family       |                                                                                                                                                        Description                                                                                                                                                         |                                                                                                                                                                                                                                                                Use Cases |
|-----------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| General purpose       |                                                                                           General purpose instances provide a balance of compute, memory, and networking resources, and can be used for a variety of workloads.                                                                                            |                                                                                                                                                          Ideal for applications that use these resources in equal proportions, such as web servers and code repositories |
| Compute optimized     |                                                                                                    Compute optimized instances are ideal for compute-bound applications that benefit from high-performance processors.                                                                                                     | Well-suited for batch processing workloads, media transcoding, high performance web servers, high performance computing (HPC), scientific modeling, dedicated gaming servers and ad server engines, machine learning inference, and other compute intensive applications |
| Memory optimized      |                                                                                                  Memory optimized instances are designed to deliver fast performance for workloads that process large datasets in memory.                                                                                                  |                                                                 Memory-intensive applications, such as high-performance databases, distributed web-scale in-memory caches, mid-size in-memory databases, real-time big-data analytics, and other enterprise applications |
| Accelerated computing |                                  Accelerated computing instances use hardware accelerators or co-processors to perform functions such as floating-point number calculations, graphics processing, or data pattern matching more efficiently than is possible in software running on CPUs.                                  |                                                                                                                Machine learning, HPC, computational fluid dynamics, computational finance, seismic analysis, speech recognition, autonomous vehicles, and drug discovery |
| Storage optimized     | Storage optimized instances are designed for workloads that require high sequential read and write access to large datasets on local storage. They are optimized to deliver tens of thousands of low-latency random I/O operations per second (IOPS) to applications that replicate their data across different instances. |                                                                                                                   NoSQL databases (Cassandra, MongoDB and Redis), in-memory databases, scale-out transactional databases, data warehousing, Elasticsearch, and analytics |
| HPC optimized         |                                                                                        High performance computing (HPC) instances are purpose built to offer the best price performance for running HPC workloads at scale on AWS.                                                                                         |                                                                                                                                     Ideal for applications that benefit from high-performance processors, such as large, complex simulations and deep learning workloads |

## Serverless

With serverless compute, you can spend time on the things that differentiate your application,
rather than spending time on ensuring `availability`, `scaling`, and `managing servers`. Every
definition
of serverless mentions the following four aspects:

- no servers to provision or manage.
- scales with usage.
- You never pay for idle resources.
- Availability and fault tolerance are built in.

## Fargate

AWS Fargate is a purpose-built **serverless compute engine for containers**. AWS Fargate scales and
manages the infrastructure by allocating the right amount of compute. This eliminates the need to
choose and
manage EC2 instances, cluster capacity, and scaling. Fargate supports both Amazon ECS and Amazon EKS
architecture and provides workload isolation and improved security by design.

![AWS Fargate.jpg](images/AWS Fargate.jpg)

## AWS Lambda

A **function** is a resource that is invoked to run the code in Lambda. Lambda runs instances of a
function to process events.

**Triggers** describe when a Lambda function should run. A trigger integrates a Lambda function with
other AWS services and event source mappings.

An **event** is a JSON-formatted document that contains data for a Lambda function to process. The
runtime converts the event to an object and passes it to the function code. When you invoke a
function, you determine the structure and contents of the event.

An **application environment** provides a **secure** and **isolated** runtime environment for a
Lambda function. An application environment manages the processes and resources that are required to
run the function.

Lambda supports two types of **deployment packages**:

- A .zip file archive – containing the function code and its dependencies

- A container image – compatible with the Open Container Initiative (OCI) specification. You
  add your function code and dependencies to the image. You must also include the operating system
  and a Lambda runtime.

The **runtime** provides a language-specific environment that runs in an application environment.

The AWS **Lambda function handler** is the method in the function code that processes events. When
the function is invoked, Lambda runs the handler method. When the handler exits or returns a
response, it becomes available to handle another event.

Python example:

```
def handler_name (event, context):

...

return some_value
```

## Pricing

With **On-Demand** Instances, you pay for compute capacity per hour or per second, depending on
which instances that you run. Billing begins whenever the instance is running, and billing stops
when the instance is in a stopped or terminated state.

Use cases:

- low cost and flexibility of Amazon EC2 without upfront payment or long-term
  commitments
- Applications with short-term, spiky, or unpredictable workloads that cannot be interrupted
- Applications being developed or tested on Amazon EC2 for the first time

With Amazon EC2 **Spot Instances**, you can request spare Amazon EC2 computing capacity for up to 90
percent off the On-Demand price.

Use cases:

- Applications that have flexible start and end times
- Applications that are only feasible at very low compute prices
- Users with fault-tolerant or stateless workloads

With Spot Instances, you set a limit on how much you want to pay for the instance hour. This is
compared against the current Spot price that AWS determines. If the amount that you pay is more than
the current Spot price and there is capacity, you will receive an instance.

**Savings Plans** are a flexible pricing model that offers low usage prices for a 1-year or 3-year
term commitment to a consistent amount of usage. Savings Plans apply to Amazon EC2, AWS Lambda, and
AWS Fargate usage and provide up to 72 percent savings on AWS compute usage.

For workloads that have predictable and consistent usage, Savings Plans can provide significant
savings compared to On-Demand Instances.

Use cases:

- Workloads with a consistent and steady-state usage
- Customers who want to use different instance types and compute solutions across different
  locations
- Customers who can make monetary commitment to use Amazon EC2 over a 1-year or 3-year term

For applications with steady state usage that might require reserved capacity, Amazon EC2 offers the
**Reserved Instances** option. With this option, you save up to 75 percent compared to On-Demand
Instance pricing. You can choose between three payment options: All Upfront, Partial Upfront, or No
Upfront. You can select either a 1-year or 3-year term for each of these options.

With **Reserved Instances**, you can choose the type that best fits your applications needs.

- Standard Reserved Instances: These provide the most significant discount (up to 72 percent off
  On-Demand pricing) and are best suited for steady-state usage.
- Convertible Reserved Instances: These provide a discount (up to 54 percent off On-Demand pricing)
  and the capability to change the attributes of the Reserved Instance if the exchange results in
  the creation of Reserved Instances of equal or greater value. Like Standard Reserved Instances,
  Convertible Reserved Instances are best suited for steady-state usage.
- Scheduled Reserved Instances: These are available to launch within the time windows that you
  reserve. With this option, you can match your capacity reservation to a predictable recurring
  schedule that only requires a fraction of a day, a week, or a month.

A **Dedicated Host** is a physical Amazon EC2 server that is dedicated for your use. Dedicated Hosts
can help you reduce costs because you can use your existing server-bound software licenses, such as
Windows Server, SQL Server, and Oracle licenses. And they can also help you meet compliance
requirements. Amazon EC2 Dedicated Host is also integrated with AWS License Manager, a service that
helps you manage your software licenses, including Microsoft Windows Server and Microsoft SQL Server
licenses.

- Dedicated Hosts can be purchased on demand (hourly).
- Dedicated Hosts can be purchased as a Reservation for up to 70 percent off the On-Demand price.

## Amazon VPC

A virtual private cloud (VPC) is an isolated network.
After you create your VPC, you must create **subnets** inside the network. The typical use case for
subnets is to isolate or optimize network traffic. In AWS, subnets are used to provide high
availability and connectivity options for your resources.

**Reserved IPs**

For AWS to configure your VPC appropriately, AWS reserves five IP addresses in each subnet. These IP
addresses are used for routing, Domain Name System (DNS), and network management.

For example, consider a VPC with the IP range 10.0.0.0/22. The VPC includes 1,024 total IP
addresses. This is then divided into four equal-sized subnets, each with a /24 IP range with 256 IP
addresses. Out of each of those IP ranges, there are only 251 IP addresses that can be used because
AWS reserves five.

![reserved-ips.png](images/reserved-ips.png)

AWS reserves five IP addresses in each subnet that cannot be assigned to a resource.

The five reserved IP addresses can impact how you design your network. A common starting place for
those who are new to the cloud is to create a VPC with an IP range of /16 and create subnets with an
IP range of /24. This provides a large amount of IP addresses to work with at both the VPC and
subnet levels.

### Gateways

**Internet gateway**

To activate internet connectivity for your VPC, you must create an internet gateway. Think of the
gateway as similar to a modem. Just as a modem connects your computer to the internet, the internet
gateway connects your VPC to the internet. Unlike your modem at home, which sometimes goes down or
offline, an internet gateway is highly available and scalable. After you create an internet gateway,
you attach it to your VPC.
An internet gateway is a VPC component that permits communication between your VPC and the internet.
A virtual private gateway is the VPN endpoint on the Amazon side of your VPN connection that can be
attached to a single VPC.

![igw.png](images/igw.png)

**Virtual private gateway**

A virtual private gateway connects your VPC to another private network. When you create and attach a
virtual private gateway to a VPC, the gateway acts as anchor on the AWS side of the connection. On
the other side of the connection, you will need to connect a customer gateway to the other private
network. A customer gateway device is a physical device or software application on your side of the
connection. When you have both gateways, you can then establish an encrypted virtual private
network (VPN) connection between the two sides.

![pgw.jpg](images/pgw.jpg)

**AWS Direct Connect**

To establish a secure physical connection between your on-premises data center and your Amazon VPC,
you can use AWS Direct Connect. With AWS Direct Connect, your internal network is linked to an AWS
Direct Connect location over a standard Ethernet fiber-optic cable. This connection allows you to
create virtual interfaces directly to public AWS services or to your VPC.

![direct-connect.png](images/direct-connect.png)

### Main route table

When you create a VPC, AWS creates a route table called the **main route table**. A route table
contains a set of rules, called **routes**, that are used to determine where network traffic is
directed. AWS assumes that when you create a new VPC with subnets, you want traffic to flow between
them. Therefore, the default configuration of the main route table is to allow traffic between all
subnets in the local network.

The following **rules** apply to the main route table:

- You cannot delete the main route table.
- You cannot set a gateway route table as the main route table.
- You can replace the main route table with a custom subnet route table.
- You can add, remove, and modify routes in the main route table.
- You can explicitly associate a subnet with the main route table, even if it's already
  implicitly associated.

### Custom route tables

The main route table is used implicitly by subnets that do not have an explicit route table
association. However, you might want to provide different routes on a per-subnet basis for traffic
to access resources outside of the VPC. For example, your application might consist of a frontend
and a database. You can create separate subnets for the resources and provide different routes for
each of them.

If you associate a subnet with a custom route table, the subnet will use it instead of the main
route table. Each custom route table that you create will have the local route already inside it,
allowing communication to flow between all resources and subnets inside the VPC. You can protect
your VPC by explicitly associating each new subnet with a custom route table and leaving the main
route table in its original default state.

![route-table.png](images/route-table.png)

### Secure subnets with network access control lists

Think of a network access control list (network **ACL**) as a virtual firewall at the subnet level.
A network ACL lets you control what kind of traffic is allowed to enter or leave your subnet. You
can configure this by setting up rules that define what you want to filter.

Network ACLs are considered **stateless**, so you need to include **both** the **inbound and
outbound** ports used for the protocol. If you don’t include the outbound range, your server would
respond but the traffic would never leave the subnet.

Because network ACLs are configured by **default** to **allow incoming and outgoing traffic**, you
don’t need to change their initial settings unless you need additional security layers.

### Secure EC2 instances with security groups

The default configuration of a **security group** blocks all inbound traffic and allows all outbound
traffic.

Security groups are **stateful**. That means that they will remember if a connection is originally
initiated by the EC2 instance or from the outside, and temporarily allow traffic to respond without
modifying the inbound rules.

_**Example**_

**Subnets** can be used to segregate traffic between computers in your network. **Security groups**
can be used in the same way. A **common design pattern** is to organize resources into different
groups and create security groups for each to control network communication between them.
Network traffic between different resource groups can be controlled using security groups.

![security-group.jpg](images/security-group.jpg)

This example defines three tiers and isolates each tier with defined security group rules. In this
case, internet traffic to the web tier is allowed over HTTPS. Web tier to application tier traffic
is allowed over HTTP, and application tier to database tier traffic is allowed over MySQL. This is
different from traditional on-premises environments, in which you isolate groups of resources with a
VLAN configuration. In AWS, security groups allow you to achieve the same isolation without tying
the security groups to your network.

## Storage Types

![storage-types.png](images/storage-types.png)

### File storage

Ideal when you require **centralized access** to files that must be **easily shared and managed** by
multiple host computers. Typically, this storage is mounted onto multiple hosts, and requires file
locking and integration with existing file system communication protocols.

#### Use cases for file storage

- **Web serving**: Cloud file storage solutions follow common file-level protocols, file naming
  conventions, and permissions that developers are familiar with. Therefore, file storage can be
  integrated into web applications.


- **Analytics**: Many analytics workloads interact with data through a file interface and rely on
  features such as file lock or writing to portions of a file. Cloud-based file storage supports
  common file-level protocols and has the ability to scale capacity and performance. Therefore, file
  storage can be conveniently integrated into analytics workflows.


- **Media & entertainment**: Many businesses use a hybrid cloud deployment and need standardized
  access using file system protocols (NFS or SMB) or concurrent protocol access. Cloud file storage
  follows existing file system semantics. Therefore, storage of rich media content for processing
  and collaboration can be integrated for content production, digital supply chains, media
  streaming, broadcast playout, analytics, and archive.


- **Home directories**: Businesses wanting to take advantage of the scalability and cost benefits of
  the cloud are extending access to home directories for many of their users. Cloud file storage
  systems adhere to common file-level protocols and standard permissions models. Therefore,
  customers can lift and shift applications that need this capability to the cloud.

### Block storage

File storage treats files as a singular unit, but block storage splits files into fixed-size chunks
of data called blocks that have their own addresses. Each block is an individual piece of data
storage. Because each block is addressable, blocks can be retrieved efficiently. Think of block
storage as a more direct route to access the data.

When data is requested, the addresses are used by the storage system to organize the blocks in the
correct order to form a complete file to present back to the requester. Besides the address, no
additional metadata is associated with each block.

If you want to change one character in a file, you just change the block, or the piece of the file,
that contains the character. This ease of access is why block storage solutions are fast and use
less bandwidth.

#### Use cases for block storage

Because block storage is optimized for **low-latency operations**, it is a preferred storage choice
for **high-performance** enterprise workloads and **transactional**, **mission-critical**, and
**I/O-intensive** applications.

### Object storage

In object storage, files are stored as objects. Objects, much like files, are treated as a single,
distinct unit of data when stored. However, unlike file storage, these objects are stored in a
bucket using a **flat structure**, meaning there are no folders, directories, or complex
hierarchies. Each object contains a unique identifier. This identifier, along with any additional
metadata, is bundled with the data and stored.

#### Use cases for object storage

With object storage, you can store almost any type of data, and there is **no limit** to the number
of objects stored, which makes it readily scalable. Object storage is generally useful when storing
**large or unstructured data sets**.

**Data archiving**

Cloud object storage is excellent for long-term data retention. You can cost-effectively archive
large amounts of rich media content and retain mandated regulatory data for extended periods of
time. You can also use cloud object storage to replace on-premises tape and disk archive
infrastructure. This storage solution provides enhanced data durability, immediate retrieval times,
better security and compliance, and greater data accessibility.

**Backup and recovery**

You can configure object storage systems to replicate content so that if a physical device fails,
duplicate object storage devices become available. This ensures that your systems and applications
continue to run without interruption. You can also replicate data across multiple data centers and
geographical regions.

**Rich media**

With object storage, you can accelerate applications and reduce the cost of storing rich media files
such as videos, digital images, and music. By using storage classes and replication features, you
can create **cost-effective**, **globally replicated architecture** to deliver media to distributed
users.

### Relating back to traditional storage systems

- Block storage in the cloud is analogous to **direct-attached storage** (DAS) or a **storage area
  network** (SAN).
- File storage systems are often supported with a **network-attached storage** (NAS) server.

### Amazon Elastic File System (Amazon EFS)

**Amazon Elastic File System** (Amazon EFS) is a set-and-forget file system that automatically grows
and shrinks as you add and remove files. There is no need for provisioning or managing storage
capacity and performance. Amazon EFS can be used with AWS compute services and on-premises
resources.

With the Amazon EFS simple web interface, you can create and configure file systems quickly without
any minimum fee or setup cost. You **pay only for the storage used** and you can choose from a range
of storage classes designed to fit your use case.

| Standard storage classes                                                                                                                       | One zone storage classes                                                                                                                        |
|------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| EFS Standard and EFS Standard-Infrequent Access (Standard-IA) offer Multi-AZ resilience and the highest levels of durability and availability. | EFS One Zone and EFS One Zone-Infrequent Access (EFS One Zone-IA) provide additional savings by saving your data in a single availability zone. |

### Amazon FSx

Amazon FSx is a fully managed service that offers reliability, security, scalability, and a broad
set of capabilities that make it convenient and cost effective to launch, run, and scale
high-performance file systems in the cloud. With Amazon FSx, you can choose between four widely used
file systems: Lustre, NetApp ONTAP, OpenZFS, and Windows File Server. You can choose based on your
familiarity with a file system or based on your workload requirements for feature sets, performance
profiles, and data management capabilities.

## Block Storage with Amazon EC2 Instance Store and Amazon EBS

The unique characteristics of block storage make it the preferred option for transactional,
mission-critical, and I/O-intensive applications.

### Amazon EC2 instance store

Amazon Elastic Compute Cloud (Amazon EC2) instance store provides temporary block-level storage for
an instance. This storage is located on disks that are physically attached to the host computer.
This ties the lifecycle of the data to the lifecycle of the EC2 instance (ephemeral storage).

Instance store is ideal if you host applications that replicate data to other EC2 instances, such as
Hadoop clusters. For these cluster-based workloads, having the speed of locally attached volumes and
the resiliency of replicated data helps you achieve data distribution at high performance. It’s also
ideal for temporary storage of information that changes frequently, such as buffers, caches, scratch
data, and other temporary content.

![EC2 instance store](images/ec2-instance-store.png)

## Amazon EBS

**Amazon Elastic Block Store** (Amazon EBS) is block-level storage that you can attach to an Amazon
EC2 instance.

- detachable
- distinct
- size-limited
- 1-to-1 connection

> AWS announced the Amazon EBS multi-attach feature that permits Provisioned IOPS SSD (io1 or io2)
> volumes to be attached to multiple EC2 instances at one time. This feature is not available for
> all
> instance types, and all instances must be in the same Availability Zone

### Scaling Amazon EBS volumes

- increase volume size (max size 64 TiB)
- attach multiple volumes

### Amazon EBS use cases

Amazon EBS is useful when you must retrieve data quickly and have data persist long term.

- Operating systems

Boot and root volumes can be used to store an operating system. The root device for an instance
launched from an Amazon Machine Image (AMI) is typically an EBS volume. These are commonly referred
to as EBS-backed AMIs.

- Databases

As a storage layer for databases running on Amazon EC2 that will scale with your performance needs
and provide consistent and low-latency performance.

- Enterprise applications

Amazon EBS provides high availability and high durability block storage to run business-critical
applications.

- Big data analytics engines

Amazon EBS offers data persistence, dynamic performance adjustments, and the ability to detach and
reattach volumes, so you can resize clusters for big data analytics.

### EBS volume types

- **solid-state drives** (SSDs) - used for transactional workloads with frequent read/write
  operations with small I/O
  size.

  ![SSD volumes](images/SSD volumes.PNG)
- **hard-disk drives** (HDDs) - used for large streaming workloads that need high throughput
  performance

  ![HDD volumes](images/HDD volumes.PNG)

### Amazon EBS snapshots

EBS snapshots are incremental backups that only save the blocks on the volume that have changed
after your most recent snapshot. The backups are stored redundantly in multiple Availability Zones
using Amazon S3.

## Object Storage with Amazon S3

Unlike Amazon EBS, Amazon **Simple Storage Service** (Amazon S3) is a standalone storage solution
that isn’t tied to compute. With Amazon S3, you can retrieve your data from anywhere on the web.

Amazon S3 is an object storage service. Object storage stores data in a flat structure. An object is
a file combined with metadata.

A bucket must be created first. The combination of a bucket name, key, and version ID uniquely
identifies the object.

### Amazon S3 bucket names

Amazon S3 supports **global buckets**. Therefore, each bucket name must be **unique** across all AWS
accounts in all AWS Regions within a partition. A partition is a grouping of Regions, of which AWS
currently has three: Standard Regions, China Regions, and AWS GovCloud (US).

- Bucket names must be between 3 (min) and 63 (max) characters long.
- Bucket names can consist only of lowercase letters, numbers, dots (.), and hyphens (-).
- Bucket names must begin and end with a letter or number.
- Buckets must not be formatted as an IP address.
- A bucket name cannot be used by another AWS account in the same partition until the bucket is
  deleted.

### Object key names