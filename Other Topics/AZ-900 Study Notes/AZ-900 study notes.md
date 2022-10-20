# AZ-900

Notes on Azure, based on various sources available in 2022.

General note: The Azure portal can be viewed on any browser and hence can be viewed on almost any operating system. The Azure CLI is available on all platforms.

## Azure Cloud concepts

#### General Cloud Terms

The following terms are used to describe the benefits of using cloud services.

*Scalability* means the ability to scale, so being able to add more resources when needed.

*Elasticity* means the ability to scale dynamically; automatic upscaling and downscaling, to avoid either a lack or an excess of resources.

*Agility* means requests for new resources and services can be delivered quickly, which is made possible by the cloud. New services can be delivered much more quickly via the cloud than via on-premises.

*Fault tolerance* - able to keep services alive despite component failures.

*Disaster recovery* - able to recover from a major disaster that takes down services.

*Availability* = uptime / (uptime - downtime), where 99.99% availability suggests less than an hour of downtime per year.

*Economy of Scale*, in the context of the cloud, means that the cloud provider has the benefit of buying in bulk to reduce costs per unit. A small company can then indirectly benefit from this effect by becoming a customer of the cloud provider.

*CapEx* - own infrastructure, with a big initial investment. Value lowers over time. Tax deduction extends over a longer time (varies).

*OpEx* - operational expenditure for "rented" infrastructure, which is easier to compare to revenue on an annual basis to judge the value of those expenses. Tax deduction is possible within the same year because you are paying every month. The cloud can help you focus on OpEx instead of CapEx.

#### Cloud Models

- **IaaS** runs hardware and virtualization, but everything including the OS and beyond is up to you.
- **PaaS** also runs the OS, the runtime and any middleware in between for you.
- **SaaS** means you have direct access to sophisticated software.

Other terms that are sometimes used:

**NaaS** Network as a service, **DSaaS** Data Science as a service.

#### IaaS use cases
- temporary high performance computing - rent the machines when you need them.
- testing an application during release.
(current state of the OS is always known to you - cloud provider is not going to install/change anything.)
- only pay for resources in use, keeping configuration in the cloud but stopping the VM when you don't need it.
- you get to choose when the VM is updated.
note: requires you to update your own OS, close your own ports and generally protect your own server.

#### PaaS use cases
- when quickly moving on-premises applications to the cloud, with flexible deployment options for your applications.
- need to run a web app that needs a specific framework running (such as PHP) and you don't want to manage and configure this on the VM.
- don't have to worry about docker installation or configuration as it's included on all App Service VMs by default.
- backing up and restoring data tend to be more user-friendly and feature-rich in PaaS, because of custom software already installed by the cloud provider.

#### PaaS example services
	Azure CDN
	Azure Cosmos DB
	Azure SQL Database
	Azure Database for MySQL
	Azure Storage
	Azure Synapse Analytics

#### SaaS use cases
- ready-to-go software that works from just about any device
- cloud provider takes care of availability, backups and patches
- you don't have to know anything about the software internals

#### Use case examples for cloud models
- Public cloud model: easy and fast to move to the cloud.
- Private cloud model: transparency/security concerns, privacy and regulatory concerns.
- Hybrid cloud model: keeping sensitive data on-premises while still benefitting from applications running in the public cloud.
- Community cloud model: hospitals sharing medical data.

## Azure Service concepts

#### Basic Resource Hierarchy

**Azure Virtual Machines (VMs)** are software-based computers that run within a physical computer. The physical computer is considered the host and the VMs are often referred to as guests. A VM does not need to run the same operating system as the host. Azure VMs support marketplace and images.

VMs still count as IaaS, because you have total control over the operating system. This means VMs are useful for custom requirements and a high degree of control, as opposed to PaaS offerings.

**Resource Groups** are required groupings for your Azure VMs. All the resources in your resource group should share the same lifecycle. You deploy, update and delete them together. Each resource can only exist in one resource group. You cannot have a resource group within another resource group.

**Azure Subscriptions** contain one or more Resource Groups.

**Management Groups** are folders that can contain one or more Subscriptions, or more Management Groups.

**Containers** are packages of software that contain all of the necessary elements to run in any environment. Containers only emulate operating systems, not the whole machine, making them smaller than Virtual Machines.

#### Regions, Zones and Availability/Scale Sets

**Azure Regions** are geological locations with one or more Availability Zones (with some exceptions) where one or more data centers, which house Azure servers, are located.

**Azure Availability Zones** are physically separate locations within an Azure region that are tolerant to local failures. Some specific Azure regions might not provide any Availability Zones.

An **Availability Set** protects you from downtime caused by issues happening in a datacenter. It involves using fault domains and update domains and requires deploying at least two VMs. 

*Update Domains* protect you from planned maintenance events. 

*Fault Domains* protect from unplanned updates and unexpected downtime. By default, Azure assigns two Fault Domains to an Availability Set.

An Availability Set should not be confused with an Availability Zone. The latter means multiple servers in distinct data centers in a region with a 99.99 availability SLA, and the former means multiple racks in a datacenter with a 99.95 availability SLA.

An Availability Set has the disadvantage of requiring you to set up a load balancer in front of the VMs for handling traffic. Other disadvantages include having to explicitly set up new machines, and having to configure them.

A **Scale Set** solves some of the disadvantages of an availability set. It provides options for creating a load balancer or gateway, and allows you to pick the operating system and exactly how many VMs you need. Azure will create as many VMs as you want, up to 1000, in one step. 

The Scale Set is still IaaS, but is one step beyond simply using VMs. As with VMs, you are still in control of the OS and you still have to prepare images etc. However, you now use a single image to create a set of identical VMs. The resulting VMs will have built-in autoscaling features.

The auto-scale feature will automatically start new VMs to handle rising pressure or scale back and deallocate instances. This will provide you with elasticity. Alternatively, you can pick a static amount of VMs.

Scale Sets are deployed in availability sets automatically, so you immediately benefit from multiple fault domains and update domains. Unlike VMs in an availability set, VMs in a Scale Set are also compatible with availability zones, so you will be protected from problems in an Azure datacenter.

*Moving beyond VMs*
With VMs, the OS has to be replicated across every VM. For optimization reasons you could switch to using containers instead. Every app can then run inside of its own container, instead of on its own OS, within the container runtime. This likely means moving on to Azure Container Instances, the simplest and fastest way to run containers in Azure, and leaving behind IaaS to use PaaS instead. More on this below.

**Azure Load Balancer** provides even traffic distribution with high scalability and availability. It supports both inbound and outbound scenarios. Both UDP and TCP applications are supported. It can be a good idea to use two load balancers - one for external traffic and one for internal traffic.

**Azure Application Gateway** is similar to Azure Load Balancer but offers more features including a firewall. If you are using two load balancers, one for internal and one for external traffic, then the outer load balancer can be replaced with Gateway. You can then use a feature called "SSL termination" to decrypt traffic (move from https to http) and internally use unencrypted traffic to reduce the processing power required to handle requests.

Gateway can also be placed in front of multiple App Services, not just in front of multiple VMs.

**Content Delivery Network:** Handles static content for web applications, such as images and stylesheets. This content can then be distributed across the world, across muliple POP (point of presence) locations, to reduce latency for users living in different continents. Microsoft has over 120 of these locations (as of 2020).

**ExpressRoute** lets you extend your on-premises networks into the Microsoft cloud over a private connection with the help of a connectivity provider. Can be used to connect to both Azure services and Microsoft 365 services.

#### Workload products available in Azure (non-exhaustive list)

**Azure Compute Services** are the hosting services responsible for hosting and running the application workloads. These include Azure Virtual Machines (VMs), Azure Container Service, Azure App Services etc.

Serverless products include: **Azure Functions**, **Logic Apps**, **Event Grid**.

**Azure Functions:** A Function is designed for microservices. It is basically a small piece of code that runs when something triggers it. Recommended for short-lived, event-driven processes. You can write code without having to worry about deploying it or about designing elasticity - as requests increase, Functions will meet the demand with as many resources as needed and then automatically scale down as requests fall. Functions are PaaS but are sometimes called "serverless". You can have a (cheap) consumption-based plan or a dedicated plan for your hosting/pricing model.

**Logic Apps:** Create and run workflows with little to no code. These are similar to Function Apps in that they are kicked off by a trigger. Offers a visual designer and pre-built operations.

**Event Grid:** An event broker to integrate applications using events (where an event is the smallest amount of info that fully describes something that happened in the system). Events can be communicated to other applications and services, and Azure Event Grid can guarantee availability of the resulting event-driven architecture by spreading across fault domains/availability zones.

**Azure Container Instances (ACI)** is a PaaS service that offers the ability to run containerized applications easily. This is a simple service for simple use cases, with no autoscaling and a maximum of 20 nodes.

**Azure Kubernetes Service (AKS)** is an opensource, high-scale, PaaS container orchestration service. It can scale to add additional containers when needed and it can then scale back when the needs are reduced. It’s responsible for monitoring containers, and ensuring that they’re always running. It has a minimum of 3 nodes and a max of 100.

**App Service Plan:** A PaaS service designed as an enterprise grade web application service. Every web app you create runs inside of an App Service plan. Multiple apps can run inside of a single App Service plan.

You can create a default C# ASP.NET web app in visual studio and then very quickly deploy this to the App Service. Many other programming languages are supported as well. You sacrifice a large amount of control over the platform/hardware in exchange for easy maintenance and autoscaling.

The following pricing tiers are available in App Service: 
- Free - no-cost
- Shared - low-cost
- Basic, Standard, Premium, and PremiumV2 - highest cost tiers with additional features

"You are charged for App Service plans even when no web apps are running in them. If you do have web apps in your App Service plan, you are still charged if you stop the web apps. The only way to avoid being billed for an App Service plan is to delete it."

**Azure SQL Database:** a PaaS offering for SQL Server (relational) database hosting. Could also be called DBaaS or database as a service. Provides rich query capabilities.

**Azure SQL Managed Instance:** offers the same capabilities as on-premises SQL Server but in the cloud. More expensive than Azure Sql Database but offers more features.

**SQL Data Warehouse:** a version of Sql Server for massively parallel processing operations, so for Big Data.

**Cosmos DB:** a hosted, globally distributed, NoSQL (non-relational) database system. Designed for highly responsive and/or multi-regional applications.

An **Azure Virtual Network (VNet)** is an emulation of a physical networking infrastructure that allows Virtual Machines to communicate amongst themselves. You can even use a VNet to communicate between your on-premises resources and your Azure resources. 

VNets can be segmented into subnets. This can be done to group related resources, and to apply security rules and filters across multiple resources within the same subnet. Network filtering will function through Network Security Groups or Application Security Groups - more on those topics will be discussed in the section *Securing Networks*.

Multiple VNets can be linked up with *VNet Peering*, and then act as one. A VNet can only reside within a single region, forcing you to create multiple VNs across regions. VNet peering can still join the VNets together. Another way to accomplish this goal across is with VPN Gateway, depending on your company's needs.

#### Other notes:

-The initial *Domain Name* for a new Azure AD tenant cannot be changed and cannot be deleted. You can only add custom domain names for your business.

-**SKU** stands for *Stock Keeping Unit*, used to refer to Virtual Machine SKUs in the context of Azure. SKUs represent different, distinct shapes of purchasable products (specific VMs). These can be compared in terms of performance and costs.

## Azure Storage Concepts

#### Azure Storage Services

- blob storage: unstructured data like files and documents
- file storage: supports SMB protocol and can be attached to network drives. so it is storage for files accessed via shared drive protocols. it is designed to implement lift-and-shift scenarios or to extend on-premise file shares.
- disk storage: disk emulation in the cloud, to store the VM disks as used by IaaS VMs. disks can be managed or unmanaged.
- table storage: store (semi-)structered data in the form of NoSQL non-relational data. designed for fast access.
- queue storage: store/retrieve messages for async applications that pass messages. meant for small pieces of data.

#### Account Types

An **Azure Storage Account** is highly scalable and highly durable.

Account types include:
- general purpose v2: blobs (binary large object), files, tables, queues; most redundancy options
- premium block blob: best for high transaction rates or low storage latency
- premium file share: best for enterprise/high performance applications that need to scale
- premium page blob: larger blobs; best for random read and write operations
note: all premium account types use solid state drives.
note: you can't change the storage account type after it's been created.

**Azure Files** is a completely managed fileshare. It relieves you of the burden of managing a VM and adding disks to the VM. Azure Files shares are backed by Azure Storage, so you will need a storage account to create an Azure Files share. Use Azure File Sync to keep your files in Azure Files synchronized with your on-premises server.

#### Categories of Redundancy Options

- Redundancy in primary region: **LRS** and **ZRS**
- Redundancy in a secondary region: **GRS** or **GZRS**
- *Read access to the secondary region* when using **GRS** or **GZRS** (normally not available until a failover)

#### Data Redundancy Options

- **LRS: Locally-redundant storage** - the cheapest option; protection against server-rack and drive failures
- **ZRS: Zone-redundant storage** - recommended for high availability; protection against datacenter-level failures
- **GRS: Geo-redundant storage** - recommended for backups; failover capabilities in a secondary region (works even if the entire primary region is harmed by a regional disaster). You cannot choose the secondary region as this is decided by microsoft. Paired regions are listed online.
- **GZRS: Geo-zone-redundant storage** - offers the protection of both GRS and ZRS.

#### Blob Access Tiers

- *Hot tier:* lowest data access cost, highest storage cost
- *Cool tier:* low storage cost, higher data access cost
- *Archive tier:* lowest storage cost, highest data retrieval cost

You can move data between access tiers via the feature known as Blob Lifecycle Management, setting policies for moving between tiers, because aged data is less likely to see frequent data access and should be treated accordingly.

**Blob types**
- block blob: composed of blocks so that it is optimized for uploading; most cost-effective way to store a large number of files
- append blob: can only append blocks; optimized for appending data which is ideal for logs
- page blob: made for frequent random read/write; used to store disks for virtual machines and databases

#### Data Transfer Options

Data transfer considerations: amount of data, frequency of data transfer, available network bandwidth

Online options for data transfer:
- **Azure Portal:** the basic way of accessing Azure via the browser
- **Azure Storage Explorer:** with a familiar GUI similar to the File Explorer in windows, so tasks can be delegated to business users; this explorer uses AzCopy behind the scenes.
- **AzCopy:** command line tool that can be used to upload and manage data, and can be used to change access tiers from hot to cool etc.
- **PowerShell:** tool to designed to assist automation. Familiar to IT professionals with a history of working with Windows servers. Becomes multi-platform with the aid of PowerShell Core.
- **Azure CLI:** command line interface for Azure with native OS terminal scripting - you will have different scripting capabilities depending on which native OS terminal you are using.
- **Azure Cloud Shell:** offers access to Azure CLI or PowerShell from the browser via Azure Portal. Even works through Azure mobile app.
- **Storage Client Libraries (SDKs)**
- **Azure File Sync:** to extend on-premises data to Azure. Frequently accessed data is kept on premises and less-frequently used data is automatically stored on Azure.

#### Azure Migrate

- can assess your on-premises and virtual servers for migration compatibility
- can assess on-premises SQL Server instances; can then migrate to SQL running on a VM, to an Azure SQL Database, or an Azure SQL Managed Instance (scalable cloud database service)
- can assess web applications running on-premises and then migrate them to run on Azure App service or Azure Kubernetes service (an orchestration service for containers).
- can also assess your on-premises virtual machine infrastructure and then migrate that to Azure Virtual Desktop
- with the Data Box service, you can migrate a large amount of unstructered data. There is both an online version as well an offline variant where Microsoft will actually ship hard drives to you. Upon receiving the hard drives you can then upload and encrypt your data before shipping the drives back to Microsoft.

*Azure Migrate tools:*

- Discovery and Assessment Tool: assess migration readiness, estimate size and cost of Azure servers, identify dependencies; virtual appliance is installed locally to do the assessment
- Server Migration Tool: actually replicates the servers to Azure; replication app is installed locally, mobility service agent is installed on the server; incremental updates after initial migration

*For migrating on-premises SQL Server databases:*

- Data Migration Assistant: detects compatibility issues with the cloud, recommends improvements
- Azure Database Migration Service: for large database migrations 

Both of the db tools above can target SQL servers in VMs, Azure SQL Database, or Azure SQL Managed Instances (scalable cloud database service)

## Azure Management & Monitoring concepts

**Azure Resource Manager (ARM)** is central to all of Azure Management. When using Azure Portal, you're really just using a website that is sending requests to the right ARM endpoint. ARM handles authentication via Azure Active Directory (Azure AD), and then sends a request on to the Azure Service that you want to create or manipulate.

- You can also interact with ARM via the Azure CloudShell which includes Azure Powershell and Azure CLI.
- There is also an Azure Mobile App to create/manage resources and receive alerts via ARM.
- Azure SDKs allow you to call ARM endpoints, so you can turn Azure management into a custom solution.

**Azure Resource Manager Templates** allow you to create resources in a repeatable way, via the command line.

**Azure Service Health** keeps you informed about the health of your cloud resources. This includes information about current and upcoming issues such as outages and planned maintenance.

This is divided up into three parts:
- *Azure Status*: Service outages across all of Azure
- *Service Health*: Service health of services and regions that you actually use. Includes planned maintenance, Health Advisories and Security Advisories. In the menu blade Service issues you can add resource health alerts.
- *Resource Health*: Health of specific resources

#### Azure Monitor 

**Monitor** is a service that collects metrics and logs from the resources in your subscription. You can use it to check the performance and availability of your apps and services. Azure Monitor comes with **Metrics Explorer** and **Log Analytics**. Also included in Monitor is the service called **Application Insights**, which monitors the availability, usage and performance of your web apps. For even deeper monitoring you can use the Application Insights SDK from inside your code.

Azure Monitor is turned on and collects metrics from Azure services by default. 

You can collect data from virtual machines running outside of Azure by installing agents.

**Log Analytics** has pre-built queries for different purposes, such as alerts, performance, and audit logs for Azure AD. You can send metrics to Log Analytics so they can be queried like other logs.

The *Insights* menu section in Azure Monitor refers to curated visualizations for specific services. Application Insights is for your web applications, Network insights is for your deployed network resources, etc.

You can send logs to a storage account and specify retention (an expiration date), or you can send logs to a third-party tool.

**Metrics** is very lightweight because it only stores numeric data, and so can support near real-time scenarios. This makes it useful for alerts and fast detection of problems. However, Metrics can only store data in a particular structure. Azure Monitor Logs, on the other hand, can store various data types and can support a more complex analysis via log queries.

**Azure Monitor Logs** stores the data that it collects in one or more Log Analytics workspaces. You must create at least one workspace to use Azure Monitor Logs.

After you create a Log Analytics workspace, you need to add and configure data sources. No data is collected automatically. The configuration will be different depending on the data source. 

Data collection in Logs will incur ingestion and retention costs. Before enabling data collection, users should refer to Azure Monitor pricing.

#### Azure Advisor 

This is a "personalized cloud consultant" and a tool that can make recommendations on how to improve performance, availability, security and on how to save costs. All of the recommendations from Azure Advisor are free.

Includes Azure Advisor Security Assistance, which integrates with Azure Security Center, to provide best practice security recommendations.

#### Azure Arc 

**Arc** can allow you to manage resources hosted outside of Azure (on-premises or in other cloud providers). This allows for consistent management and security across your environment. For instance, you can manage servers, Kubernetes clusters, Azure data services (running on-premises or in the public cloud) and SQL Server instances all hosted outside of Azure.

Benefits of Arc include features of ARM, including: organizing resources with groups and tags, searching and indexing with Azure Resource Graph, security and access through role-based access control and subscriptions, automation and update management.

*Azure Arc - Server Management*

With Arc, you can (among other things) monitor managed servers for threats (with Microsoft Defender) and security related events (with Microsoft Sentinel). You can apply Azure policies, and you can collect logs with the Log Analytics agent (not installed by default).

## Azure Security and Privacy Concepts

Includes: 
- Azure identity services: Azure AD
- Access control: role-based access control (RBAC)
- Azure governance: Azure policies and blueprints
- Securing networks: network security groups, firewalls and routing
- Reporting and Compliance: standards, data protection and monitoring

#### Azure Identity Services

In Azure, authentication is provided by Azure AD and authorization is provided by role-based access control (RBAC).

**Azure AD** is for single sign-on (SSO) and application integration - it does not have all the features of Active Directory Domain Services. If you want that, then you need Azure AD Domain Services (Azure AD DS), which is a PaaS offering by Azure but it still does not offer all the same features and cannot (yet) be used to replace on-premises ADDS. Use **Azure AD Connect** to replicate objects from ADDS.

#### Access Control

**Role-based access control (RBAC)**
Three most commonly used, built-in roles for working with resources are: 
- Owner: can manage everything about the resource
- Contributor: can do everything except grant other users access to resources
- Reader: read-only access 

You should start with using the (many) built-in roles, create custom roles only when needed, and always stick with the *Least Privilege Principle* where users have just enough access privileges to do their jobs and no more than that.

The Least Privilege Principle is also part of the **Zero Trust concept:** a model used by Microsoft for implementing security that teaches to "never trust, always verify".
- use least privilege access - limit user access with just-in-time (JIT) and just-enough-access (JEA), risk-based adaptive policies, and data protection measures
- "assume breach" - verify end-to-end encryption and use analytics to get visibility and drive threat detection
- verify explicitly - always authenticate and authorize on all available data points, including identity, location, device health and more.

#### Locks

The use of a lock can prevent the deletion or modification of a resource group and its contents. 

The two types of locks are "Read-only" and "Delete" where in the former case no resources can be added or removed, while in the latter case you can still add new resources to the resource group - you just can't delete them.

#### Specific Governance Tools

You can use governance tools to ensure:
- Security standards: security requirements for cloud deployments.
- Technical standards: standard sizes, builds and configurations.

**Azure Tags**
Security requirements (and cost control, and deployments) can also be enforced with Azure Tags.

**Azure Policy**
A policy is a collection of rules. Each policy is assigned to a scope such as a subscription. You can make sure resources will remain compliant with corporate standards. Policies involve definitions, assignments and parameters. Built-in policies exist for storage accounts, resource types and allowed locations. You can also enforce the use of certain tags or even certain VM SKUs (Stock Keeping Unit). One built-in policy is called "Allowed virtual machine size SKUs" which limits what sort of SKUs can be deployed. In script form, a policy has a number of if/then statements.

**Azure Initiative**
An initiative is a collection of policies. Initiatives can be assigned to a scope such as a management group. As seen with policies, initiatives involve definitions, assignments and parameters.

**Azure Blueprints**
A way of orchestrating deployment of resource templates and artifacts such as roles. Blueprints are special in that they maintain a relationship with the resources that they have deployed. This is very different from using a resource template. Changing a resource template has no effect on resources that have already been deployed with said template. A change to a Blueprint CAN affect deployed resources.

Blueprints include policies, initiatives and artifacts.

Using Blueprints involves: 
- Blueprint definition: includes details of resource groups, resource manager templates included for deployment, policies included, roles assigned to resources that the Blueprint has created.
- Blueprint publishing: with a version number and change notes. When you change the Blueprint and save the draft you will then publish it under a new version number.
- Blueprint assignment: assign to a scope; select location, version number and other finalizing parameters.

#### Securing Networks

Basic security principle: plan your security with defense-in-depth, meaning with multiple layers.

You can secure Virtual Networks (VNs) with Network Security Groups and Application Security Groups.

**Network Security Groups (NSGs)**
- NSGs filter traffic: allow or deny inbound and outbound traffic with inbound/outbound lists. 
- NSGs contain rules ordered by priority, from highest to lowest. 
- Attached to subnets or network cards.
- Each NSG can be linked to multiple resources.
- NSGs are stateful (when inbound traffic is allowed, return traffic will be allowed outbound)
- NSG properties include: name, priority, source or destination, protocol (TCP, UDP), direction (inbound/outbound), port range, action (allow/deny).

**Application Security Groups** 
Make sure NSGs are configured once and do not need to be readjusted every time a set of servers is added.
- they do not replace NSGs but enhance them.
- allow you to reference a group of resources.
- used as a source or destination in NSG.
- create the application security group, link it to resources, and then use this group when working with NSGs.

**VPN Gateway** allows you to connect a VNet to your on-premises environment over the public internet, or to connect VNets to each other for cross-regional networks.

**Azure Firewall** 
This is an Azure-managed stateful firewall service that protects access to your virtual networks. Features include threat intelligence (learning about what traffic is harmless and what is suspicious), outbound and inbound NAT, integration with Azure Monitor, network traffic filtering rules and unrestricted scalability.

**Azure DDoS Protection** 
This works hand in hand with Azure Firewall and also integrates with Azure Monitor. Features include multi-layer support, attack analytics, scale and elasticity and protection from unplanned costs. It comes in Basic and Standard tier, where Basic is free. Both tiers have always-on detection, are backed by an SLA and have an availability guarantee. Standard tier also includes real time metrics, post attack reports and access to live support from DDoS experts.

**Azure User Defined Routes:** 
Use to override default system routes. Often used when traffic must be filtered through a virtual application.

**Azure Security and Reporting Tools**
- Azure Information Protection and Information Security Tools
- Azure Key Vault
- Azure Security Center

#### Reporting and Compliance

**Azure Information Protection (AIP)** is used to classify documents and emails, labeled as confidential etc. Labeled documents can be protected with encryption and can then only be opened with the appropriate rights. AIP labels can be applied manually, automatically, or show up as recommendations. 

Three common security and reporting resources are Azure Monitor (logs), Azure Service Health (report incidents), Azure advanced thread protection (identify suspicious activity).

**Azure Key Vault** is for secrets management, key management and certificate management. Can enable logging and enable centralized administration of secrets. It is recommended to create new key vaults for new applications/environments, to create regular backups, to turn on logging and set up alerts and to turn on soft delete and purge protection.

**Azure Security Center** reports on security threats as well as compliance status against certain standards. It is immediately available for PaaS services. For non-Azure services, you can deploy agents for monitoring.

Included with Security Center is **Azure Sentinel**, which is intelligent and handles:
- Security information event management (SIEM)
- Security orchestration automated response (SOAR)

#### Azure Compliance and Data Protection Standards

Two main topics:
- Azure Service Trust Portal 
- Azure Special Regions

Common compliance standards include HIPAA (a US standard for safeguarding medical info), PCI (Payment Card Industry), GDPR (EU data protection laws), FedRAMP, ISO 27001

You can use Azure Blueprints to deploy a complete environment configured to your company's compliance needs. Use it to deploy a secure and compliant subscription. Azure itself is certified against all the standards that it supports. In Azure Security Center you can see your current compliance posture measured against common standards, such as GDPR or PCI.

In the **Azure Service Trust Portal** you will find Compliance Manager, Trust Documents with security implementation and design info, compliance information specific to certain industries and regions, a section called My Library to save and access compliance documents and finally a link to Microsoft's general Trust Center.

**Azure Special Regions** exist for compliance and legal reasons. They are not generally available and you must request access from Microsoft to use them. These regions are US Gov, China and Germany.

To enforce isolation of your data, you can utilize *Dedicated Hosts*. Each of these is dedicated to a single organization. This host-level isolation can help you meet certain compliance requirements.

## Azure Pricing and Support concepts

#### Azure Subscriptions 

A **Subscription** is a logical container of Azure resources and administration. Adopting Azure starts with creating a new Subscription, tying it to an account, and then deploying the cloud resources you want to use. This creates a hierarchy of:
- Subscription 
- Resource Group 
- Resources that share a lifecycle.

An **Azure Account** is used for contact information and billing details regarding a Subscription. Every new Subscription has to be associated with an Azure Account.

The elements of a Subscription are as follows:
- legal agreement
- billing unit
- logical boundary of scale (can only have a certain amount of VNets deployed, or certain amount of web apps)
- first container created and administrative boundary

A Subscription has a trust relationship with at least one **Azure Active Directory**. An AAD directory can have trusts with multiple subscriptions, but each Subscription can trust only one AAD directory for identity and access management.

Reasons to add a new Subscription:
- when you expect to exceed the limits within a Subscription (set by Microsoft)
- to divide permissions among department heads and separate security concerns
- to constrain/scope resource providers (allow specific services to run and disallow other services)
- delegate administration through RBAC

Naming conventions for your new Subscriptions are extremely important to help manage and find resources. New names should ideally include the company, department (IT, finance etc.), location, environment (production, development etc.), instance (numerical value).

**Management Groups** allow you to apply governance conditions (access & policies) a level above Subscriptions. A common way to use Management Groups is to have one Root Management Group that contains sub groups such as an IT Management Group, a Marketing Management Group etc. This way, the sub groups can inherit permissions and Azure policies from the Root Management Group.

#### Planning and Management of Costs

Azure Subscription options include Free (access to free services and $200 in Azure credit over 30 days), Pay-As-You-Go (monthly), Student ($100 in Azure credit over 12 months), Enterprise Agreement (purchase services from Microsoft under single agreement).

Options for purchasing Azure products and services: 
- Enterprise Agreement (EA): for very large organizations
- Direct from Microsoft: bill from Microsoft
- Indirect - Cloud Solution Provider (CSP): bill from CSP

Azure services that are always free (at the time of writing) include Security Center (free tier), Azure DevOps 5 users, DevTest Labs, Event Grid with 100,000 operations per month etc.

Factors affecting cost include resource types, service types (Direct, Enterprise Agreement etc), locations, egress (outbound) traffic. Sending data into Azure (ingress) is free.

An **Azure Zone** is a geographical grouping of Azure Regions for billing purposes. Data transfer pricing is based on the Zones.

Zone composition at the time of writing:
- Zone 1: United States, US Government, Europe, Canada, UK, France, Switzerland
- Zone 2: East Asia, Southeast Asia, Japan, Australia, India, Korea
- Zone 3: Brazil, South Africa, UAE
- DE Zone 1: Germany

Best practices for minimizing Azure costs include: Reserved Instances (predict the amount of VMs you need to run for the year and pre-pay for a discount), Quotas (limits around resources), Spending Limits (disable deployment of new resources upon reaching limits), Tags (as a tool for showback or chargeback, and quickly identifying app owner), Azure Cost Management (analyze and predict costs), Azure Hybrid Benefit (licensing benefit, involving running the workloads of your on-premises OS and SQL Server licences in the cloud)

Pricing calculators:
- **Azure Pricing Calculator** - estimate costs for combinations of Azure products.
- **Total Cost of Ownership (TCO) Calculator** - estimate cost savings when migrating to Azure, along with TCO, over the next five years.

#### Support Options Available with Azure

- Basic support plan is free and mostly involves support for subscription and billing.
- Developer - non production environment - email support during business hours, with response within 8 hours. General guidance on architecture.

Other support plans include 24/7 support via email or phone, with a response within the hour.
- Standard: production environment - help with architecture based on best practices, some support for onboarding practices, service reviews, Azure Advisor consultations. Access to web seminars for training purposes. Proactive guidance from a ProDirect delivery manager.
- Professional Direct: business critical - similar to Standard
- Premier: substantial dependence - customer-specific architectural support, reviews led by Technical Account Manager (TAM), on-demand training, Azure Event Management (with additional fee) for launch support

Also available for support: Azure Knowledge Center, MSDN Forum

**Azure Service Level Agreements (SLAs)**
An **SLA** outlines what a service provider will provide to its customers, and what standards the provider will meet. In the context of Azure, an SLA details the commitments for uptime and connectivity. Different services will have different SLAs.

A **Composite SLA** will involve more than one service, and this is very likely what a company will see going forward because it is most likely using more than one service at a time.

An example of a composite SLA: App Service web app (99.95 uptime) and SQL Server database (99.99 uptime) put together would have a 99.94 uptime SLA.

You can determine the right SLA for your company depending on the following points:
- balanced cost and complexity with high availability
- dependencies of the application - will require you to understand the SLAs of those dependencies as well
- recovery metrics: *RTO* = max downtime, *RPO* = max duration of data loss
- availability metrics: *Mean Time to Recover (MTTR)* = average time it take to restore a component after failure, *Mean Time Between Failures (MTBF)* = how long a component is expected to last until the next failure
- composite SLA - what your SLAs will combine into

#### Service Lifecycle in Azure

Topics:
- public and private preview features
- general availability (GA)
- keeping track of product and feature updates 

At the start of the cycle are previews, made available for evaluation and feedback. 

**Public Previews** are available to all Azure customers. **Private Previews** tend to require an invitation or special signup. 

When previews go live they are considered to fall under **General Availability (GA)** and become part of the default product set.

You can monitor feature updates and product changes via the What's New page in Azure Portal, via azure.microsoft.com/updates, or by reading the announcements on the official Azure Blog https://azure.microsoft.com/en-us/blog/topics/announcements/

## Random Sample Exam Questions

Q: Does MS provide a separate portal for Azure portal specific previews? 
A: Yes.

Q: A company wants to store data that is infrequently used. It needs to be accessed via Power BI. What could be a cost-effective data layer for this requirement?
Options: Azure SQL db, Azure PostgreSQL, Azure Cosmos DB, Azure Data Lake
Answer: Option D: Azure Data Lake. The other options should be used for frequently accessed data.

Q: A company needs 50 custom VMs. 20 are windows-based and 30 are ubuntu. Which option would reduce administrative effort?
Options: Azure Load Balancer, Azure Web Apps, Azure Traffic Manager, Azure Scale Sets.
Answer: Option D: Azure Scale Sets. The other options are incorrect because Load Balancer is for diverting traffic to back-end VMs at the network layer, Web Apps is for hosting web apps, and Traffic Manager is used for DNS-based traffic routing.

Q: A company wants to enforce Multi-factor authentication for users entering Azure. Which option lets them do this?
Options: Azure Service Trust Portal, Azure Security Center, Azure DDoS Protection, Azure privileged identity management
Answer: Options D: Azure privileged identity management. Trust portal is for information on compliance with data protection standards and regulatons. Security Centre is a unified infrastructure security management system in Azure.

Q: A company wants to host 2 VMs in Azure. When the VM is stopped, do you still incur costs for the storage attached to the VM?
A: Yes. Turning the VM off only helps to avoid storage transaction costs. The disk(s) will also incur monthly costs.

Q: Can you create VMs in Azure if you run a powershell script on a Linux machine, with Azure CLI tools installed?
A: No (trick question). You also need Powershell Core installed to run powershell scripts on Linux.

Q: A company is planning to host resources in Azure. They want to ensure Azure complies with the rules and regulations in the region. Which option can assist the company in getting the required compliance reports?
Options: Azure AD, Microsoft Trust Center, Azure Advisor, Azure Security Centre
Answer: Option B: Microsoft Trust Center. Azure AD is for identity management. Azure Advisor is for recommendations in Azure.

Q: The company has just deployed a VM and needs to know if it is having any issues. Which option will let you view such issues?
Options: Azure Advisor, Azure AD, A Virtual Machine menu blade, Azure Monitor
Answer: The VM menu blade called resource health will show the most specific information for the VM. Azure Monitor is for showing the health of your entire Azure infrastructure (although Monitor's Metrics Explorer can show individual resources).

Q: What feature of a system makes it elastic?
Options: Self-healing after a crash, withstanding DDoS attacks, staying available while updates are being made to the system, increasing and reducing capacity based on actual demand.
Answer: Option D: Increasing and reducing capacity based on actual demand.
