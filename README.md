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
- size-limited: you pay for what you provision (you have to provision storage in advance).
- 1-to-1 connection
- EBS volumes are replicated across multiple servers in a single Availability Zone.

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

> Amazon S3 supports buckets and objects, and there is no hierarchy. However, by using prefixes and delimiters in an
> object key name, the Amazon S3 console and the AWS SDKs are able to infer hierarchy and introduce the concept of
> folders.

### Amazon S3 use cases

- Backup and storage
- Media hosting
- Software delivery
- Data lakes
- Static websites
- Static content

## Security in Amazon S3

Everything in Amazon S3 is **private by default**.

![S3 security](images/s3-security.png)

To be more specific about who can do what with your Amazon S3 resources, Amazon S3 provides several security management
features: **IAM policies**, **S3 bucket policies**, and **encryption** to develop and implement custom security
policies.

### Amazon S3 and IAM policies

When IAM policies are attached to resources (buckets and objects) or IAM users, groups, and roles, the policies define
which actions they can perform. Access policies attached to the resources are referred to as **resource-based policies**
and access policies attached to users in an account are called **user policies**.

You should use IAM policies for private buckets in the following **two scenarios**:

- You have **many buckets** with **different permission requirements**. Instead of defining many different S3 bucket
  policies, you can use IAM policies.
- You want **all policies** to be in a **centralized location**. By using IAM policies, you can manage all policy
  information in one location.

### Amazon S3 encryption

Amazon S3 reinforces encryption **in transit** (as it travels to and from Amazon S3) and **at rest**. To protect data,
Amazon S3 **automatically** encrypts all objects on upload and applies server-side encryption with S3-managed keys as
the base level of encryption for every bucket in Amazon S3 at no additional cost.

## Amazon S3 storage classes

| Storage Class                                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| S3 Standard                                    | This is considered general-purpose storage for cloud applications, dynamic websites, content distribution, mobile and gaming applications, and big data analytics.                                                                                                                                                                                                                                                                                                                                                                            |
| S3 Intelligent-Tiering                         | This tier is useful if your data has **unknown or changing access patters**. S3 Intelligent-Tiering stores objects in three tiers: a **frequent** access tier, an **infrequent** access tier, and an **archive instance** access tier. Amazon S3 monitors access patterns of your data and **automatically** moves your data to the most cost-effective storage tier based on frequency of access.                                                                                                                                            |
| S3 Standard-Infrequent Access (S3 Standard-IA) | This tier is for data that is accessed **less frequently** but requires **rapid access** when needed. S3 Standard-IA offers the high durability, high throughput, and low latency of S3 Standard, with a low per-GB storage price and per-GB retrieval fee. This storage tier is ideal if you want to store long-term backups, disaster recovery files, and so on.                                                                                                                                                                            |
| S3 One Zone-Infrequent Access (S3 One Zone-IA) | Unlike other S3 storage classes that store data in a minimum of three Availability Zones, S3 One Zone-IA stores data in a **single Availability Zone**, which makes it less expensive than S3 Standard-IA. S3 One Zone-IA is ideal for customers who want a lower-cost option for infrequently accessed data, but do not require the availability and resilience of S3 Standard or S3 Standard-IA. It's a good choice for storing secondary backup copies of on-premises data or easily recreatable data.                                     |
| S3 Glacier Instant Retrieval                   | Use S3 Glacier Instant Retrieval for archiving data that is **rarely accessed** and requires **millisecond retrieval**. Data stored in this storage class offers a cost savings of up to 68 percent compared to the S3 Standard-IA storage class, with the same latency and throughput performance.                                                                                                                                                                                                                                           |
| S3 Glacier Flexible Retrieval                  | S3 Glacier Flexible Retrieval offers low-cost storage for archived data that is **accessed 1–2 times per year**. With S3 Glacier Flexible Retrieval, your data can be **accessed** in as little as **1–5 minutes** using an expedited retrieval. You can also request **free bulk retrievals** in **up to 5–12 hours**. It is an ideal solution for backup, disaster recovery, offsite data storage needs, and for when some data occasionally must be retrieved in minutes.                                                                  |
| S3 Glacier Deep Archive                        | S3 Glacier Deep Archive is the **lowest-cost** Amazon S3 storage class. It supports long-term retention and digital preservation for data that might be accessed once or twice a year. Data stored in the S3 Glacier Deep Archive storage class has a default **retrieval time of 12 hours**. It is designed for customers that retain data sets for 7–10 years or longer, to meet regulatory compliance requirements. Examples include those in highly regulated industries, such as the financial services, healthcare, and public sectors. |
| S3 on Outposts                                 | Amazon S3 on Outposts delivers object storage to your on-premises AWS Outposts environment using S3 API's and features. For workloads that require satisfying local data residency requirements or need to keep data close to on premises applications for performance reasons, the S3 Outposts storage class is the ideal option.                                                                                                                                                                                                            |

### Amazon S3 versioning

Versioning keeps multiple versions of a single object in the same bucket. This preserves old versions of an object
without using different names, which helps with object recovery from accidental deletions, accidental overwrites, or
application failures.
![s3-versioning](images/s3-versioning.png)

By using **versioning-enabled buckets**, you can recover objects from accidental **deletion** or **overwrite**:

- Deleting an object does not remove the object permanently. Instead, Amazon S3 puts a **marker** on the object that
  shows that you tried to delete it. If you want to restore the object, you can **remove the marker** and the object is
  **reinstated**.
- If you **overwrite** an object, it results in a **new object version** in the bucket. You still have access to
  previous versions of the object.

### Versioning states

Buckets can be in one of three states. The versioning **state** applies to **all objects** in the bucket. Storage *
*costs** are incurred for **all objects** in your bucket, including **all versions**.

- **Unversioned (default)** - No new and existing objects in the bucket have a version.
- **Versioning-enabled** - Versioning is enabled for all objects in the bucket. After you version-enable a bucket, it
  can **never return to an unversioned state**. However, you can **suspend versioning** on that bucket.
- **Versioning-suspended** - Versioning is **suspended for new objects**. All new objects in the bucket will not have a
  version. However, all **existing** objects **keep** their object versions.

### Managing your storage lifecycle

Two types of actions:

- Transition actions define when objects should transition to another storage class.
- Expiration actions define when objects expire and should be permanently deleted.

![storage-lifecycle](images/storage-lifecycle.png)

The following use cases are good candidates for the use of lifecycle configuration rules:

- Periodic logs
- Data that changes in access frequency

### Choosing the Right Storage Service

| EC2 instance store                           | EBS                                                                               | S3                                                                                | EFS                                                                                                                                                               |
|----------------------------------------------|-----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ephemeral block storage                      | Block storage.                                                                    | Object storage.                                                                   | File storage.                                                                                                                                                     |
| Included in the EC2 instance cost            | You pay for what you provision (you have to provision storage in advance).        | You pay for what you use (you don’t have to provision storage in advance).        | Amazon EFS is elastic, and automatically scales up or down as you add or remove files. And you pay only for what you use.                                         |    
| Is terminated together with the EC2 instance | EBS volumes are replicated across multiple servers in a single Availability Zone. | Amazon S3 replicates your objects across multiple Availability Zones in a Region. | Amazon EFS is highly available and designed to be highly durable. All files and directories are redundantly stored within and across multiple Availability Zones. |
| Built-in drive                               | Most EBS volumes can only be attached to a single EC2 instance at a time.         | Amazon S3 is not storage attached to compute.                                     | Amazon EFS offers native lifecyle management of your files and a range of storage classes to choose from.                                                         |

#### Amazon FSx

Amazon FSx provides native compatibility with **third-party file systems**. You can choose from NetApp ONTAP, OpenZFS,
Windows File Server, and Lustre. With Amazon FSx, you don't need to worry about managing file servers and storage. This
is because Amazon FSx automates time consuming administration task such as hardware provisioning, software
configuration, patching, and backups. This frees you up to focus on your applications, end users, and business.

## Databases

> ACID - atomicity, consistenct, isolation, durability

### Unmanaged databases

If you host a database on Amazon EC2, AWS implements and maintains the physical infrastructure and hardware and installs
the EC2 instance operating system (OS). However, you are still responsible for managing the EC2 instance, managing the
database on that host, optimizing queries, and managing customer data.

This is called an **unmanaged database** option. In this option, AWS is responsible for and has control over the
hardware and underlying infrastructure. You are responsible for and have control over management of the host and
database.

![unmanaged-DB](images/unmanaged-db.png)

### Managed databases

Managed database services provide the setup of both the EC2 instance and the database, and they provide systems for high
availability, scalability, patching, and backups. However, in this model, you’re still responsible for database tuning,
query optimization, and ensuring that your customer data is secure. This option provides the ultimate convenience but
the least amount of control compared to the two previous options.

![managed-db](images/managed-db.png)

## Amazon RDS

**Amazon RDS** is a managed database service customers can use to create and manage relational databases in the cloud
without the operational burden of traditional database management.

Supported Amazon RDS engines include the following:

- Commercial: Oracle, SQL Server
- Open source: MySQL, PostgreSQL, MariaDB
- Cloud native: Aurora

### Database instances

Underneath the DB instance is an EC2 instance. However, this instance is managed through the Amazon RDS console instead
of the Amazon EC2 console.

DB instance classes:

- Standard
- Memory optimized
- Burstable

### Storage on Amazon RDS

The storage portion of DB instances for Amazon RDS use Amazon **Elastic Block Store** (Amazon EBS) volumes for database
and log storage. This includes **MySQL**, **MariaDB**, **PostgreSQL**, **Oracle**, and **SQL Server**.

When using **Aurora**, data is stored in cluster volumes, which are single, **virtual volumes** that use solid-state
drives (**SSD**s). A cluster volume contains copies of your data across three Availability Zones in a single AWS Region.
For **nonpersistent**, temporary files, Aurora uses **local storage**.

Amazon RDS provides three **storage types**:

- **General Purpose SSD** (gp2 and gp3)
- **Provisioned IOPS SSD** (io1)
- **Magnetic** (standard)

### Backup data

#### Automated backups

Automated backups are turned on by default. This backs up an entire DB instance (not just individual databases on the
instance) and transaction logs. When a DB instance is being created, you set a backup window that is the period of time
that automatic backups occur.

**Retaining backups**: Automated backups are retained between 0 and 35 days. The 0 days setting stops automated backups
from happening. If you set it to 0, it will also delete all existing automated backups. This is not ideal. The benefit
of automated backups that you can do point-in-time recovery.

**Point-in-time recovery**: This creates a new DB instance using data restored from a specific point in time. This
restoration method provides more granularity by restoring the full backup and rolling back transactions up to the
specified time range.

#### Manual snapshots

If you want to keep your automated backups longer than 35 days, use manual snapshots. Manual snapshots are similar to
taking Amazon EBS snapshots, except you manage them in the Amazon RDS console. These are backups that you can initiate
at any time. They exist until you delete them. For example, to meet a compliance requirement that mandates you to keep
database backups for a year, you need to use manual snapshots. If you restore data from a manual snapshot, it creates a
new DB instance using the data from the snapshot.

### Choosing a backup option

It is advisable to deploy both backup options. Automated backups are beneficial for point-in-time recovery. With manual
snapshots, you can retain backups for longer than 35 days.

### Redundancy with Amazon RDS Multi-AZ

In an Amazon **RDS Multi-AZ deployment**, Amazon RDS creates a **redundant copy** of your database in another
Availability Zone. You end up with **two copies** of your database — a **primary copy** in a subnet in one Availability
Zone and a **standby copy** in a subnet in a second Availability Zone.

The primary copy of your database provides access to your data so that applications can query and display the
information. The data in the primary copy is **synchronously replicated** to the standby copy. The **standby** copy is
**not** considered an **active** database, and it does not get queried by applications.

To improve availability, Amazon RDS Multi-AZ **ensures** that you have **two copies** of your database **running** and
that **one of them** is in the **primary** role. If an availability issue arises, such as the primary database loses
connectivity, Amazon **RDS initiates an automatic failover**.

![multi-az-rds](images/multi-az-rds.png)

When you create a DB instance, a Domain Name System (DNS) name is provided. AWS uses that DNS name to fail over to the
standby database. In an **automatic failover**, the **standby database is promoted to the primary role**, and queries
are redirected to the new primary database.

To help ensure that you don't lose Multi-AZ configuration, there are two ways you can create a new standby database:

- Demote the previous primary to standby if it's still up and running.
- Stand up a new standby DB instance.

### Amazon RDS security

Network **ACLs** and **security groups** help users dictate the **flow of traffic**. If you want to **restrict the
actions** and **resources** others can access, you can use AWS Identity and Access Management (**IAM**) policies.

## Monitoring

**Amazon S3 metrics:**

- Size of objects stored in a bucket
- Number of objects stored in a bucket
- Number of HTTP request made to a bucket

**RDS metrics:**

- Database connections
- CPU utilization of an instance
- Disk space consumption

**EC2 metrics:**

- CPU utilization
- Network utilization
- Disk performance
- Status checks

### Monitoring benefits:

- Respond proactively
- Improve performance and reliability
- Recognize security threats and events
- Make data-driven decisions
- Create cost-effective solutions

## Amazon CloudWatch

Amazon CloudWatch is a monitoring and observability service that collects your resource data and provides actionable
insights into your applications.

### Visibility using CloudWatch

You can use CloudWatch to do the following:

- Detect anomalous behavior in your environments.
- Set alarms to alert you when something is not right.
- Visualize logs and metrics with the AWS Management Console.
- Take automated actions like scaling.
- Troubleshoot issues.
- Discover insights to keep your applications healthy.

![cloud-watch](images/cloud-watch.png)

Many AWS services automatically send metrics to CloudWatch **for free** at a rate of **1 data point per metric per
5-minute interval**. This is called **basic monitoring**, and it gives you visibility into your systems without any
extra cost.

For applications running on EC2 instances, you can get more granularity by posting metrics every minute instead of every
5-minutes using a feature like **detailed monitoring**. Detailed monitoring **incurs a fee**.

### CloudWatch concepts

CloudWatch concepts

**Metrics** are the fundamental concept in CloudWatch. A metric represents a **time-ordered set of data points** that
are published to CloudWatch. Every metric data point must be associated with a timestamp.

AWS services that send data to CloudWatch attach **dimensions** to each metric. A dimension is a **name and value pair**
that is part of the metric’s identity. You can use dimensions to filter the results that CloudWatch returns. For
example, many Amazon EC2 metrics publish InstanceId as a dimension name and the actual instance ID as the value for that
dimension.

By default, many AWS services provide **metrics at no charge** for resources such as **EC2** instances, Amazon Elastic
Block Store (Amazon **EBS**) volumes, and Amazon **RDS** database (DB) instances. For a **charge**, you can activate
features such as **detailed monitoring** or publishing your **own application metrics** on resources such as your EC2
instances.

### Custom metrics

Some examples of custom metrics include the following:

- Webpage load times
- Request error rates
- Number of processes or threads on your instance
- Amount of work performed by your application

### CloudWatch dashboards

**Dashboards** are customizable home pages you can configure for data visualization for one or more metrics through
widgets, such as a graph or text.

Data from different AWS Regions can be collected into a single dashboard to create a global view of your architecture.

CloudWatch aggregates statistics according to the period of time that you specify when creating your graph or requesting
your metrics. You can also choose whether your metric widgets display live data. **Live data** is data published within
the last minute that has not been fully aggregated.

You are not bound to using CloudWatch exclusively for all your visualization needs. You can use **external** or **custom
tools** to ingest and analyze CloudWatch metrics using the **GetMetricData API**.

As far as security is concerned, with AWS Identity and Access Management (**IAM**) policies, you control who has access
to view or manage your CloudWatch dashboards.

### Amazon CloudWatch Logs

**CloudWatch Logs** is centralized place for logs to be stored and analyzed. With this service, you can monitor, store,
and access your log files from applications running on EC2 instances, AWS Lambda functions, and other sources.

Some services, like **Lambda**, are **set up to send log data** to CloudWatch Logs with minimal effort. With Lambda, all
you need to do is give the Lambda function the **correct IAM permissions** to post logs to CloudWatch Logs. Other
services require more configuration. For example, to send your application logs from an **EC2 instance** into CloudWatch
Logs, you need to install and configure the **CloudWatch Logs agent** on the EC2 instance. With the CloudWatch Logs
agent, EC2 instances can automatically send log data to CloudWatch Logs.

#### CloudWatch Logs terminology

![terminology](images/cloud-watch-terminology.png)

### CloudWatch alarms

You can create CloudWatch alarms to automatically initiate actions based on sustained state changes of your metrics. You
configure when alarms are invoked and the action that is performed.

To set up an alarm you need to choose the **metric**, **threshold**, and **time period**.

An alarm can be invoked when it transitions from one state to another. After an alarm is invoked, it can **initiate an
action**. Actions can be an Amazon **EC2 action**, an **automatic scaling** action, or a **notification** sent to Amazon
**Simple Notification Service** (Amazon SNS).

An alarm can be in 3 possible states: **OK**, **ALARM**, and **INSUFFICIENT_DATA**.

### Prevent and troubleshoot issues with CloudWatch alarms

You can set up different alarms for different reasons to help you prevent or troubleshoot operational issues. In the
scenario just described, the alarm invokes an Amazon SNS notification that goes to a person who looks into the issue
manually.

Another option is to have alarms invoke actions that automatically remediate technical issues.

### Solution Optimization

