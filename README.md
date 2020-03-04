# Azure

Azure

https://linuxacademy.com/course/az-500-microsoft-azure-security-technologies/
https://linuxacademy.com/course/microsoft-certified-azure-developer-exam-203-prep/



VNET
	Subnet
VNET Peering
Build in Traffic Filer - NSG

PaaS
	
	App Service
		Web Apps
		Mobile Apps
		Functions - server less piece of code and execute on azure compute nodes
		Logic Apps - server less to connect different services 
		Container - 
			- Azure Container service (AKS)

       Database Serice
		SQL
			Azure SQL
			SQL Managed Instances 
			Azure Database for Postgres SQL
			Azure Database for MySQl
			Cosmos DB
	
	Storage Service
		Blob - binary format - images, videos 
		Tables
		Files
		Message
		
	Other Services
		ML and AI
		IoT
		Identity  - Azure AD
		Operations
		Security 
	
		
		
Defense in Depth 
	Confidentiality 
		Principle of least privilege
	Integrity 
		Preventing unauthorized changes
		Data transmission 
	Availability 

Security Layers
		Physical Security - building security, datacenter hardware
		  Identity & Access - authenticate & authorization , auditing events and changes 
		    Perimeter	- DDOS protection to filter large scale attacks 
		      Network	- limit communication via micro segmentation and access controls, Deny by default,  restrict inbound and limit outbound internet 
		       Compute	- Harden VM,  implement endpoint protection 
			Application	- ensure application are secure, store sensitive application secrets appropriately, make security design a requirement for all new application 
			  Data	- 
	
Compliance and Security Requirements 

https://servicetrust.microsoft.com/ViewPage/TrustDocuments
https://servicetrust.microsoft.com/ViewPage/BluePrintOverview


Azure Active Directory  - > https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-whatis

	Azure AD Features 
		Enterprise Identity Solutions - create a single identity for users and keep them isync across the enterprise 
		Single Sign-On-
		MFA- enhance security with additional factors of authentication 
		Self Service- 

A subscription will have one unique domain that is associated with a domain, ie. Subscription is one to one mapping with the AD tenant. 
		


AD Connect Overview
	Sync Services - responsible for creating users, groups and object in the azure active directory . Also responsible for making sure the identity information of on premisses users and groups are completely matching with cloud.
		Filtering - used to limit which objects are synchronized to azure active directory. By default all users, groups, contexts and windows 10 computers Ernie are sync, you can change these filters using domains, organization units or other attributes if want to
		Password hash sync - this sync password hashes on your on prem unto azure AD. End user can enter same password on prem and cloud and need to manage only in one location. User defined password policy on prem will carry over to cloud.
			Password Sync Options
				Password Sync - Ensures user passwords are the same in both directories (Active Directory Domain Services  and Azure AD)
				Passthrough Authentication - Easy method to keep users and passwords align.When user logs into AD, the request is forwarded to AD DS and get the approval.
				Active Directory Federation Services- fully federate across AD DS and Azure AD, along with other services such as servicenow.

		Password writeback - change the password on cloud and write back to on prem
		Device writeback - registered devices in azure ad can be written back to on prem ad and used for conditional access 
		Prevent accidental delete - turned on by default; 
		Automatic Upgrade - enabled by default on express setting installations, ensure the azure ad connect is always updated with the latest updates; 
	Active Directory Federation Services -  allows us to configure a hybrid env includes on premises AD
	Health Monitoring- health activities 

*General - 
Active Directory Users;
  A cloud based user account
  A synchronized on-premises directory account
  A guest user, also known as a B2B collaboration guests.

Azure Active Directory Groups:

	Type of Group
		Security 
		Office 365

Membership type for security group
	Assigned - static in nature, the administrator determines group membership 
	Dynamic User - user  membership based on attribute values.If a particular user account matches the query, it is added to the group.If the attribute changes the account in removed.
	Dynamic Device - device membership based on attribute values.If a particular device account matches the query, it is added to the group.If the attribute changes the account in removed. Attributes for the specific devices are examined to determine group membership, not the attributes for the device owner.

Nested Group - ability to add a security group to another security group - way to easily manage group membership as well as licenses and permission to the users.
	
Design Decisions/ Authentication 
	
	Cloud Authentication 

		Cloud Only - exist completely on cloud no op prem
	
		Password Hash Sync + Seamless SSO - supports on prem, simplest way to authenticate on prem directory objects in azure AD. Premium features like identity protection do required password hash sync no mater what authentication method you use (including the below one).

		Pass-Through Authentication + Seamless SSO - supports on prem, simple password verification, using the software agent run in the on prem to validate 
	
	Federated Authentication 
		Uses on prem AD Federation Services to validate the username password. 
		
		AD FS
			
		3rd Party Federation Provider’s 


Azure HD Hybrid Identity with Password Hash Sync
	User Sign In on cloud ->Azure AD | <- identity sync with password hashes - Azure AD Connect - Active Directory 
	
	Effort - Least effort required, Part of AD Connect Sync process that runs every 2 mins
	User Experience- Deploy seamless SSO eliminating unnecessary prompts after user signs is.
	Business Continuity- Highly available as the cloud service scales with Microsoft datacenters.

Azure HD Hybrid Identity with Pass-through authentication.

	User sign in on cloud -> Azure AD | <-pass-through authentication- Authentication Agent  - Active Directory 
								| <- identity is sync - Azure AD Connect - Active Directory 

	Effort- 1 or more agents installed on existing servers, must have access to on-prem AD controllers, need outbound access to internet.
	User Experience-Deploy seamless SSO eliminating unnecessary prompts after user signs is.
	Business Continuity- recommend to deploy 2 extra pass through agents for redundancy , deploy password hash sync as a backup method. 

Azure HD Hybrid Identity with Federated authentication 
	User sign-in -> Azure AD ->redirected | -> Federation Proxy | -> Federation Servers  -> Active Directory 
				| <- identity is sync - Azure AD Connect - Active Directory 
	
	
SSO and MFA	

	SSO - > Provided by Azure AD connect for users using password sync or passthrough authentication 
	
	MFA -> 

Service Principal 
	Essentially first of all we have to have a user with permissions in Azure A.D. That can create an application. So they create an Azure A.D. application registration first of all so it could be for my particular web application I'll create an app registration for that then I'll create a key for that application that I'm ultimately going to use later on to authenticate against Azure A.D. and I'll also sign that application to a role.
And this is really the concept around a service principle and you'll see that in the upcoming demo after I've created the application I create a service principle which allows that application to access Azure Active Directory then what I can do is use that service I can take that service principle and the service might be something like octopus deploy Visual Studio Team Services and I can now allow it to log in as the application using that service principle and now it can execute tasks for example deploy VMS could deploy entire army templates that I've created which deploy a whole host of pieces of infrastructure and away we go. But all of this can be done under the service principle context. I don't have to sort of give a user's permissions out to the application every single time the application can authenticate itself.

App Registation

The concept on an application here is a little bit different because you think of your apps as things you sort of deploy on servers or run as web apps. This is more around not a person being able to do things but an application I.D. client that can basically do things. And this is all about control in what that application can do what certificates and secrets that has. What API permissions it has and what you know all back roles it has to perform say against your subscription.

	create password
	register new application using New-AzureRMADApplication
	Get the application id
	Create a spn  = New-AzureRMADServicePrinciapl using the app id
	New Role assignment New-AzureRmRoleAssignment with role definition contributor followed by spnname application id
	
	Go to console Azure Active Dir -> App Registrations ->  Click the application, setting and click keys 
 
Identity Protection 
	Identity Protection Capability 
		Detect Vulnerability and risky accounts.
		Investigating risk events
		Risk based conditional access 
	Identify Protection Roles that exists
		Global Administrator - 
				Can do - Full access to identity Protection, OnBoard Identity Protection 
		Security Administrator - 
				Can do - Full access to identity protection 
				Cannot do - Onboard identity protection, resets password for user
		Security reader -
				Can do - Read only access to identity protection 
				Cannot do - Onboard identity protection, remediate users, configure policies, reset password 
			
	
Privileged Identity Management and Tenant Security. (PIM)

	Key features;
		-Visibility into users with privileged access.  Such as;
			-Azure Resource 
			-Azure AD
		-Enable on-demand administrative access 
		-View administrator history 
		-Setup Alerts
		-Require Approvals (via workflow)
	PIM Process - Required AD P2 Lic.
		-Activation Process  - Additional Authentication.
		-Activated User - Read to do work.
	PIM Roles
		- Privileged Role Administrator 
			- Can manage role assignments in Azure AD, and all aspects of PIM
		-Security Administrator 
			-Can read security information and reports, and manage configuration in Azure AD and Offie 365

		First Person to use PIM is assigned the security administrator and privileged role administrator roles.
		Only privileged role administrators can manage Azure AD directory role assignments groups.

	Assigned Roles ( Directory vs Resource)
		Directory Roles
			Azure AD roles
			Eg. Global Admin etc.
			Roles can be ‘eligible’ or ‘permanent’
		Resource Roles
			Use Azure RBAC
			Built-in or custom roles
			E. Subscription Admin etc.
		-

Platform Protection: Network Security 
	
	Network overview - docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview 
		
		Core VNET Capabilities 
			Isolation 
			Internet Access 
			Azure resources (VM’s and other cloud services)
			VNet Connectivity  ( we can chain )
			On-Premises Connectivity 
			Traffic Filter 
			Routing - Default and user defined

		VNET’s Key Points 
			Primary building block for azure networking 
			Private network in Azure  based on an address space prefix
			Create subnets in your vnet’s with our IP range. ( we need to think about overlapping address space)
			Bring your own DNS or Azure provided DNS.
			Choose to connect the network to on premises or the internet 
	
		Route Tables;  - search route on the main search - create route table > setting > create route > Next hop type >Virtual appliance’s > 
			Routes itself cannot be used until you associate it with a subnet. You could design an entire round table and then associate it with multiple virtual networks and subnets that you see fit.
			You cannot create a peering if there are overlapping ip addresses in the subnet’s.s
	
		Add Peering to a Subnet -Choose a network >  You can peer with the subnets doesn’t have overlapping address spaces

		General - What is address space and address range ?
				  What is significance of address prefix while adding a route ?

		Network Security Overview
		
			Network access control
				Network Layer Control 
					Secure deployment’s through NSG’s (between different subnets or VM’s), Azure Security Center (just in time VM access) and Service Endpoints  (restrict blob storage on a VM to be accessed from public)
				Resource Control and Forced Tunneling
					Control routing behavior on VNets with custom behavior and shut down devices initiating connecting to the internet via Forced tunneling
				Virtual Network Security Appliances 
					Enable security a level higher than the network for extra protection. 
			Firewall
				Cloud based network security service to protect Azure Virtual Network resources.
				Fully stateful firewall service.
				Useful when NSG is not just enough or for compliance reasons.
			
			Secure remote access and cross premises connectivity
				Connect individual workstation to a VNET (Point 2 site VPN Connection) - Connect to Desktop using VPN and VPN directly into the VNET
				Connect on-premises network to a VNET with a VPN ( Site to Site VPN Connection)
				Connect on-premises network to a VNET with a dedicated WAN link ( Express route service )
				Connect virtual networks to each other
			
			Availability 
				HTTP Load Balancing (App Gateway)
				Network Load Balancing (Azure Load Balancer)
				Global Load balancing (Traffic Manager for cross regions)
				Increase availability Increases Performance 

			Name Resolution
				Safe guarding DNS, making sure DNS is accurate 
			Global Traffic Routing
				Front Door - more efficient routing 
			DDoS
				
			Monitoring and Threat Detection 
				
			Logging and Auditing

		NSG - Network Security Groups 
	
			Is a Network filter 
			Used to allow or restrict traffic to resources in your Azure network
			Inbound and outbound rules
			Associated with Subnet or Network Interface Card.
		
			NSG rules can be applied to NI or Subnet. Subnet rules apply to ALL the resources in subnet.
				Rule precedents perspective  - 
				Properties (some of the properties that you configure when you create a network security group)
					Protocol (eg TCP/ UDP)
					Source & Destination Port Range ( 1- 65536 0r * for all)
					Source & Destination Address Prefix (Use ranges or default tags)
					Direction (inbound or outbound)
					Priority - what order the rules are evaluated  - lover number has the higher priority 
						Rules are enforced based on priority 
						Range from 100 to 4096
						Lower number have higher priority
					Access (Allow/ Deny)
					NSG Default Tags
						
			Networking Limits
				-
				-
		Demo - Create and Configure NSG
			In case you want to deny all RDP traffic to a specific subnet -> Create new inbound traffic rules -> Add -> 3389 (Destination port range) . Priority 100 - highest
			Now - we could associate it with a network interface or subnet 
		General - 
		 	outbound traffic get denied int the network interface card then to the NSG on the subnet if it's reverse and come in inbound the traffic is stopped potentially first at the subnet NSG and then again at the network interface card NSG.

		Azure Load Balancing Service	
	
			Azure Load Balancer - Works on the transport layer/Layer 4 in network reference stack. It provides network level distribution of traffic across instances of an application  running the same DC
				Key Features 
					Layer 4
					Basic and standard SKUs available 
					Service Monitoring- probes health of various services behind the LB
					Automatic Reconfiguration
					Hash based distribution-  this is how Azure LB distributes its traffic and by default it uses five tuple hash composed of a source ip, source port, destination ip, destination port and protocol type to map traffic to the available servers.It also provide stickiness only within a transport session 
					Internal and public options - you can configure an internal load balancer for traffic to hit it and then disperse for internal applications or it could be people coming over the Internet and you want expose the a public IP and then you know once they hit that load balancer then we pass traffic to the servers behind the load balancer.

			Application Gateway - Good for web application that has public IP’s. Works on the application later/ Layer 7. It also access the reverse proxy service terminates the client connection and forward the request to backend end points
				Key Features 
					Layer 7 Application Load Balancing 
					Cookie based session Affinity - to maintain same user session for web applications 
					SSL Offload - 
					End to End SSL - does it by terminate the SSL connection at the app gateway and then applies routing rules to all of those traffic and reeccrypts the package and forward it to the appropriate backend based on the routing rule that you have defined.
					Web Application Firewall- SQL injection, Cross Site Scripting, etc.
					URL Based Content Routing  - allows directing traffic to different backend service based on the content the user actually requesting
					Requires its own subnet - need the create the subnet before creating the app gateway
					
			Traffic Manager - Global scale, globally distributed endpoints, this works at the DNS level.
				 Key Features 	
					Only support internet facing application
		
		General - How do we know Layer 4 or Layer 7 suits for our need ? Will WAF add latency ?
		
		Demo - create and configure azure load balancer 
					Front end portion of the load balancer - Chose internal / public; choose public ip or vent/subnet ; static or dynamic IP address assignment 
					Bank end portion of the load balancer - Go the resource (Front end portion) 
													     Backend pools -> Add -> associated to > 
													     health probes -> Add -> protocol 
													     load balancing rules  ->Add -> Choose the fontend IP address (setting/overview), session persistence, 
			     	
				configure App Gateway -> Add - > Tier- > SKU Size (how to find this) -> choose virtual network -> subnet -> 
					
		Azure Firewall
			
				Key Features
					Built in HA - no need to configure load balancer.
					Availability Zone Support - Can span availability zone which increases SLA to 99.99%.Standard SLA 99.95% for ingle zone
					Application FQDN Filtering Rules - you can limit outbound HTTP/S traffic to a specified list of fully qualified domain names
					Network traffic filtering rules - you can centrally create allow or deny network filtering rules by source and destination IP address, port and protocol.
					FQDN Tags - Allow well known traffic (eg. Windows Update) through your firewall. You can create the app rule and include the tag.
					Service Tags - Microsoft manager grouping of services
					Threat Intelligence - Filter traffic based on Microsoft threat intelligence feed. 
					SNAT/ DNAT Support - Outbound SNAT support, Inbound DNAT support.
		Demo - 
			 New firewall - > 
						new rg
						new virtual network -> 
						choose a virtual network ->  after creating a VNET and three , AzureFirewallSubnet, subnet inside that and three vm’s inside that. One is public ip with RDP access to it
						Create route table -> create -> associate with a subnet - > create/ add routes - >next hop type -> virtual appliances- > next hop address  - Firewall IP
						Go to the rules section of the firewall - >  application rule collection - > priority 200 ->  FQDN - allow google
															 network rule collection -> 	rules - IP address Allow DNS- UDP - Source address- vent address range -> google id > ports 53
												
		Distributed Denial of Service

				The goal of a DoS attack is to prevent access system or service 
				Botnets are collections of internet connected systems that an individual controls and uses without theirs owners knowledge.
				A DDoS is a collection of attack types aimed at disrupting the availability of the target
				DDoS involves many systems sending traffic to targets as part of botnet
				-How does it works-
				Azure DDoS Protection, combined with application design best practices, provides defense again DDoS attack. 
				DDoS protection provides the following service tiers;
					Basic - Automatically enabled as part of Azure platform 
					Standard- Provides additional mitigation capabilities over the basic service tier.
				DDoS protection Standard can mitigate the following types of attacks 
					Volumetric Attacks - like flood the network 
					Protocol Attacks - exploiting the weakness in the layer 3/4 
					Resource later attacks  - protect from sql injection, cross site scripting

				
		Network Watcher    - All services -> network wather
				
				In order to use NW yo have to install a NW agent in the machines 	
				
				After enabling the network watcher for that region
			
					Topology View - 
					Connection Monitor - Create a connection monitor - Click the Details pane on the bottom.
					Network Performance Monitor - NEW
					IP flow verify - to check whether a packet is allowed or denied from a VM	
					Next Hop - FIND
					Security group view- list all the effective rules for that particular network interface 
					VPM Trouble shoot - 
					Packet Capture 
					Connection troubleshoot - 

		5 ip’s are reserved for azure every subnet.	

Platform Protection: Host Security 				

Introduction to Virtual Machine
	
	A - Basic  - Basic version of the A series for testing and development 
	A - Standard - General Purpose VM’s
	B- Burstable- Burstable instances that can burst to the full capacity of the CPU when needed. Gets credits for the unused CPU usage.
	D- General Purpose - Built for enterprise applications.DS instances offer premium storage
	E- Memory Optimized-High Memory to CPU core ratio. ES instances offer premium storage
	F- CPU Optimized- High CPU core to memory ratio. FS instances offer premium storage 
	G- Godzilla - Very large instance ideal for large databases and big data use cases.
	H- Hight Performance compute -High Performance compute instances aimed at very high end computational needs such as molecular modeling and other scientific applications.
	L- Storage Optimized- Storage Optimized instances which offer a higher disk throughput and IO
	M- Memory Large - Another large scale memory option that allows for up to 3.5 TB of RAM
	N - GPU Enabled - GPU Enabled instances 
	SAP HANA on Azure Cerified instances - Specialized instances purposely build and certified for running SAP HANA
	
VM Specialization’s 
	S - Premium Storage Options Available; Example DSv2
	M - Larger Memory Configuration of intense type; Example A2m_v2
	R - Support remote directory memory access; Ex. H16mr

Azure Compute Units (ACU)
	Windows Virtual Machines
		docs.microsoft.com/en-us/azure/virtual-machins/windows/
			Left Panel -> Concepts -> VM Types and sizes -> Azure Compute Units
	Linux Virtual Machines
		docs.microsoft.com/en-us/azure/virtual-machines/linux/

Creating new VM.
	Availability Options - 
		Availability Zones - availability zones in the same region but they are in separate data centers for additional redundancy 
		Availability set -  are inside the same data center but allows you to have two virtual machines in separate racks of power and network connectivity 

	Data Disk
		Create and attach a secondary disk 
	
		Use existing disk
			temporary disk that's on a local disk associated with the host that the virtual machine runs on.
			So that has specific use cases where I want fast access to a locally cached disk and then this disk

	Accelerated Networking
			 Accelerated Networking is only available on certain instance types and certain operating system versions, example - Redhat 7.4 , the Accelerated 	Networking is available on that machine specific T series version of instance types and that allows faster networking between VM that have ACC-NT enabled, essentially bypasses one of the virtualization layer and directly goes to the NIC Card and gives better throughput 	
	
	BACKUP 
			
	WinRM Self-Signed Certificate Procedures
		docs.microsoft.com/en-us/azure/virtual-machines/windows/winrm
	
Azure Disk Encryption

	Key Features;
		protect and safeguard your data by encrypting the OS volumes and data disks that are attached to your VM for Windows
		uses bit lock for Windows and  uses DRM crypt for Linux
		It also integrates with key vault for customer management of encryption keys.
	Azure Disk encryption scenarios
		Enabling and disabling on encryption 
			on new VM’s created from the supported azure gallery images
			on existing VMs that run in Azure
			on new Windows VMs created from pre encrypted VHD and encryption keys 
			on windows virtual machine scale sets
			on data drives for linux virtual machine scale sets
			of managed disk VMs
			updating encryption settings of an existing encrypted premium and non premium storage VM
			backing up and restoring of encrypted VMS supported.
			Bring your own encryption and bring your own key scenarios in which you basically want to use your own encryption keys and store them in a azure key vault

	Non Supported Scenarios
		encrypting basic tier VMs or VM that were created in the classic VM creation method
		disabling encryption on an OS drive or data drive on a Linux VM when the OS drive is encrypted
		encrypting the OS drive for Linux virtual machine scales sets.
		encrypting windows VMs that are configured with software based RAID Systems
		encrypting custom images on Linux VMs
		integration with an on premises key management system is not supported and you need to use the cloud based key vault 
		Azure files (Shared file system)
		Network file system is not supported 
		Dynamic volumes are not supported 
		Ephemeral OS Disks are not supported 
	
	Disk Encryption Pre-Requisties   /azure/security/azure-security-disk-encryption-prerequisite
		Supported VM Sizes - 
		Supported OS - 
		Networking - Connectivity to AzureAD , Connect to KeyVault, Azure Storage 
		Key Vault - Enable for Encryption, Enable for Deployment, Enable for template deployment

	Configuration Methods
		PowerShell
		Azure CLI
		ARM Templates

	Disk Encryption on Linux ????????????????????

	VM Security Best Practices for IaaS workloads in Azure  /azure/security/fundamentals/iaas
		Pre-requisite - A virtual machine that wants to use security center needs a log analytics workspace to send the agent data to. (All Resource - Log Analytics)
		Security Center is built on top of log analytics 
		Just remember to make sure that you're using these standard licenses if you want to use any of the advanced features in security center.
	
		Security Center > Compute & Apps > <vm> for recommendations.

		VM Security: Control Access
		Best Practice : Control VM access	
			Use Azure policies to establish conventions for resources in your organization and create customized policies 
			Apply these policies to resources, such as resources groups. VMs that belongs to the resource group inherit these policies 
		
		VM Security - ARM Templates. (Infrastructure as code_
		Best Practice: Reduce variability in your setup and deployment of VMs
			Use Azure Resource Manager templates to strengthen your deployment choices and make it easier to understand and inventory the VM’s in your environment.

		VM Security- Secure Access
		Best Practices: Secure Privileged Access
			Use a least privilege approach and built in Azure roles to enable users to access and set up VMs
			Virtual Machine Contributor: Can manage VMs, but not the virtual network or storage account to which they are connected
			Classic Virtual Machine Contributor: Can manage VMs created by using the classic deployment model, but not the virtual network or storage account to which the VMs are connected
			Security Admin: In Security Center only: Can view security policies, view security states, edit security policies, view alerts and recommendation, dismiss alerts and recommendations.
			DevTest Lab User: Can view everything and connect, start, restart, and shut down VMs

		VM Security- Availability 
		Best practices: Deploy VM with uptime in mind.
			Use Availability Sets and Availability Zones to meet SLA requirements
		VM Security- Malware Protection
		Best Practice: Use Endpoint Protection 
			Prevent viruses and other malware by installing Microsoft or 3rd Party Endpoint Protection.
		VM Security- System Update
		Best Practice: Keep your VMs current
			Use the Update Management solution in Azure Automation to manage operating system updates for your Windows and Linux computers that are deployed in Azure, in on-premises environments, or in other cloud providers.
			You can quickly access the status of available updates on all agent computers and manage the process of installing required updates for servers. 
		Best Practice: Ensure at deployment that images you built include the most recent round of Windows updates. 
			Check for and install all Windows updates as a first step of every deployment 
			This measure is especially important to apply when you deploy images that come from either you or your own library 
			Although images from the Azure marketplace are updated automatically by default, there can be lag time(up to few weeks) after a public release.
		VM Security- Backup
		Best practices: Deploy and test a backup solution
			A backup needs to be handled the same way that you handle any other operation. This is true of systems that are part of your production environment extending to the cloud.
		VM Security- Monitor your Security Posture
		Best Practice: Monitor your VMs security posture
			Apply OS Security settings with recommended configuration rules.
			Identity and download system security and critical updates that might be missing
			Deploy recommendations for endpoint antimalware projection 	
			Validate disk encryption 
			Access and remediate vulnerabilities 
			Detect threats 
	
		VM Security: Monitor Performance 
		Best Practices: Monitor your VM performance 
			Create alerts and enforce logging for your VM
			Routinely exam VMs that have activity which is not ‘normal’. 
	 
		VM Security- Disk Encryption 
		Best Practice: Enable encryption on VMs
			Azure Disk Encryption generates and writes the encryption keys to your key vault. Managing encryption keys in your key vault requires Azure AD authentication.
			Create an Azure AD application for this purpose.  
		Best Practice: Use key encryption key (KEK) for an additional layer of security for encryption keys. Ass s KEK to your key vault.

		Best Practice: To make sure the encryption secrets don’t cross regional boundaries, Azure Disk Encryption needs the key vault and the VMs to be located in the same region.

		VM Security: Network Connectivity 
		Best Practice: Prevent inadvertent exposure to network routing and security.
			Use RBAC to ensure that only the central networking gourd has permission to networking resources.
		Best practice: identity and remediate exposed VMs that allow access from “any” source IP address
			Use Azure Security Center
		Best Practice: Restrict management ports - RDP, SSH
 
Container and Security 

	Container Overview
	—nothing
	
	AKS Security 
		
		Master Security - Microsoft
		Node Security - Customer 
		Container Security - Customer 

		Best Practices:	
			Secure Access to AKS 
				 Identity and Access Management all back roles etc.
			Secure Container Access- 
				secure container access so that's secure in the access to the containers themselves once a deployed network access authentication who can get into those containers et cetera
			Update AKS Nodes
				
	Container Scanning
		No native solutions
		Twistlock Enterprise 
		Aqua Cloud native
		Sysdig
	
	
	Azure CLI
		az login 
		az group create —name AZContainersRG —location eastusa2
		az acr create —resource-group AZContainersRG —name AZDemoACR —sku basic
		az acr login —name AZDemoACR
		az acr list
		docker tag azure-vote-front <loginServer>/image-name:v1
		docker push <loginServer>/image-name:v1
		az acr repository list —name AZDemoACR
		az acr repository show-tags —name AZDemoACR —repository <name> —output table

		az —version

	Create a service principal to allow the AKS cluster to interact with other Azure resources 
	
		az ad sp create-for-rbac —skip-assignment 
		az acr show —resource-group AZContainersRG —name AZDemoACR
		az role assignment create —assignee  <appId> —scope <acrId> —role Reader

		az aks create —resource-group AZContainersRG —name AZDemoAKS —node-count 1 —service-principal <appId> —client-secret <password> —generate-ssh-keys
		
		install kubectl
		az aks install-cli

		az aks get-credentials —resource-group AZContainersRG —name AZDemoAKS
		kubectl get nodes

		az acr list —resource-group AZContainersRG
		
		new nodes (VMs)
		az aks scale —resource-group AZContainersRG —name AZDemoAKS —node-count 3


Governance and Role-Based Access Control (RBAC)

RBAC Overview
	Create Users, Apps, Groups
	Assign them to objects in Azure with a specific Role
		Using Azure Active Directory. we create our users our apps our groups. Can user service principles we can give an app a permissions set as well and then we scope it so we sign the apps that uses the groups to specific objects in Azure

Azure RBAC Built-in Roles
	Owner - full access to all resources and this also includes the right to delegate access to others.
	Contributor- Can create and manage all types of Azure resources, but cannot grant access to others.
	Reader- Can view existing Azure resources, but cannot perform any other actions against them
	Other Roles-  /active-directory/role-based-access-built-in-roles 
	
	Roles include various actions
	Action defines what type of operations you can perform on a given resource type
		- Write enables you to perform PUT,POST, PATCH and DELETE operations 
		- Read enables you to perform GET operations
	Use PowerShell to get latest roles 
		Get-AzureRMRoleDefenition

User Rights

RBAC Custom Roles	
	Create if none of the build-in roles works for you
	Eash tenant can have to 200 roles 
	Use “Actions” and “NotActions”
	Assignable Scopes - Subscription, Resource Groups, Individual Resources 

	Azure Active Directory > New Group > Create a group with members, then go ahead and assign permission to user or group to resources. > go to subscriptions OR Resource group  > Access control (IAM) > Add => Create a Role > Section the Group

Azure Polices 	

	Enforce Governance- 
	Built in or Custom Code 
	Assigned to Subscriptions or Resource Groups 
	Create > Assign 
		 first create the policy and then we assign it.
	
	Azure Region Policy 
		Resource Group > Policies >   

Subscription Policies 
	
	Subscription > Policies- Compliance 
								Assign policy 

Azure Resource Locks

	Mechanism for locking down resources you want to ensure have an extra layer of protection before they can be deleted.
	2 options available 
		- CanNotDelete: Authorized users can read and modify but not delete the resource
		- ReadOnly: Authorized users can read the resource but cannot update or delete

	Resource Group > Locks  > Add 

Security Operations 

	

	Azure Monitor and Alerts 
	
	All Service > Monitor 
		Create Metrics 

	Dashboard > Virtual Machine > Overview
      	Dashboard > Diagnostic Settings >

	Log Analytics 

		Key features 
			Central Role in Monitoring 
			Data Sources
				could be virtual machines that you want to plug in and get data on.
				could be system log events windows log events performance events et cetera.
				also got other log analytics sources.
			Other Log Analytics Sources (Security Center and App Insights)
				And so these are two sets of sources that you can use whether you manually saying hey I want to get data from this machine or I would ingest data from other log analytics sources native to Azure.
			Search Queries 
				 got all that data in there we do queries on it to then output the data.
			Output Options
	
		Search Query Fundamental  - /azure/log-analytics/log-analytics-tutorial-viewdata

			Start with the source table (e.g..Event)
			Follow on with a series of operators 
			Separate out additional operations by using pipe |
			Join other tables and workspaces using “union”
		
		Demo
			All Services > Monitor > Logs
			
			Create a new log analytics workspace 	
				Create a resource > Log Analytics 

			Resource Groups > Select Resource 
						Connect Log Analytics with workspace 

			Advanced Settings
				
			Query;
				Perf | where TimeGenerated > ago(1h) | where CounterName == @“% Processor Time” | summarize avg(CounterValue) by Computer, (bin(TimeGenerated, 15m)) | render timechart
	 		
	Azure Security Center
		Centralized Policy Management
			ensure compliance with company or regulatory security requirements.We can centrally manage security policies across all of our hybrid cloud workloads so this works across
		Continuous Security Assessment 	
			can continually monitor all the machines network storage web apps etc. that we have and not just check them for threats and potential security issues that it will also give us actionable recommendations.
		Actionable Recommendations 
			
		Advanced Cloud Defense 
			can reduce threats with just in time access to management ports and some of the threat intelligence features that you'll see as well.
		Prioritized Alerts and Incidents 
		
		Integrated Security Solutions
			can collect such analyze security data from a variety of sources.	

	Prevent and Respond to Threats in Azure Security Center
	
		All Resource > Security Center > Overview > Compute 

Securing Data Service 

Azure SQL Service

	Azure SQL Database - Key features
		Predictable Performance - Measured in database throughput units (DTUs)
		High Compatibility - Supporting existing SQL client applications via tabular database stream(TDS) endpoint.
		Simplified Management - This includes SQL Server specific  Azure tools
	
	Azure SQL Database Tiers
		Basic - small database with single concurrent user
			- good for small scale apps; single active operation; 5 DTU
		Standard - Medium sized database that must support multiple concurrent connections
			- good for cloud apps; multiple operations; 10 -100 DTU
		Premium - Large database that must support a large number of concurrent connections and operations 
			- good for hight transaction volumes; large number of users; mission critical apps; 100 -800 DTU

	Azure SQL managed instances
	
	Third-party Database in Azure  - Managed 
	
		Managed database options;
			Build-in HA at no additional cost
			Predictable performance 
			Pay-as-you-go
			Auto-scaling
			Encryption at-rest and in transit
			Automatic backups
			Enterprise-grade security and compliance 	

	Third-party Database in Azure  - Non Managed 
		
		Non-Managed database options;
			Windows Azure VMs hosting MySQL installations 
			Linux Azure VMs hosting MySQL installations 
			ClearDB offering managed MySQL instance

	Design Auditing and Caching Strategies
		
		Why Audit
			- Maintain Regulatory Compliance 
			- Understand DB activity 
			- Gain deeper insights 
		What it does 
			- Tracks DB events and write them to an audit log
			- Utilize OMS workspace, Storage Accounts or Event Hubs
		You can use SQL database auditing to:
			Retain - An audit trail of selected events.	
			Report - Report on database activity using preconfigured reports and a dashboard to quickly get started
			Analyze - Analyze reports, find unusual activity, suspicious activity and trends
		
		Audit logs are written to Append Blobs in Azure Blob storage on your azure subscription
		All storage types (v1,v2, blob) are supported 
		All storage replication configurations are supported 
		Premium storage is currently not supported 
		Storage in Vnet are currently not supported 
		Storage behind a Firewall is currently not supported

	Cosmos DB
	   Features	
		Globally Distributed Database Service 
		Supports schema less data
		Used to build highly responsive Always On applications with constantly changing data
	  
	   Cosmos DB APIS
		Accessible via various API’s e.g.
			Document DB
			Mongo DB
			Graph
			Tables
		Automatically partitioned for:
			-performance 
			-storage capacity for various regions
	
       Cosmos DB Consistency Levels
  	  
	Consistency Level
		Strong - Guaranteed write operation only committed and visible on the primary after it has been committed and confirmed by all replicas
		Bounded Staleness - Allows to configure how stale docs can be within the replicas; staleness means the quantity or version count a replica document can be behind a primary document.
		Session - Guarantees that all read and write operations are consistent within a user session
		Consistent Prefix - Ensures changes are read in the order that matches the sequence of the corresponding writes.
		Eventual - Offers looser consistency and commits and write operations  against the primary immediately.
	
	Consistency Strategy 
			Stronger Consistency Level - Ensure documents in replicas do not lag behind the primary
								     - Recommended for applications that require all replica to exactly match the primary at any point in time
								     - Negative affect on the write operations 
	
			Weaker Consistency Level  - Ensures the database operates at peak efficiency 
								    - Recommended for all apps that required high performance 
								    - Read operations against a replica can return stale data
			
	Azure Data Lake Store Overview

		Securing data in Azure Data Lake Store
			
			Authentication 
				Integrated with AAD
			Access Control
				ACLs can be enabled on the root folder, on subfolders, and on individual files
			Encryption 
				Data lake store also provides encryption for data that is stored in the account
				Specify the encryption settings while creating the Data Lake store account
				Choose for have your data encrypted or no
				
	Types of Data
		Structured Data
			aka - Relational Data
			Adheres to a sachem
			All the data has the same field or properties 
		Semi Structured Data
			Doesn’t fit neatly into tables, rows and columns
			Uses tags or keys to organize and provide a hierarchy for the data
		Unstructured Data
			No designated structure 
			No restriction on the kinds of data it can hold

Storage Account

	Storage Account Types
		General Purpose v1
		Blob Account
		General Purpose v2 - main choice.
	
	Block Blobs vs Page Blobs
		Block blob
			Ideal for storing text or binary files
			A single block blob can contain up to 50,000 blocks of up to 100 MB each, for a total size of 4.75 TB
		Page Blobs
			Efficient for read/write operations
			Used by Azure VM
			Up to 8 TB in size
	
	Storage Tiers	
		Hot - Access very periodically 
			Higher storage costs
			Lower access costs
		Cold
			Lower storage costs
			Higher access costs
		
		Archive -
			Lowest storage costs
			Highest retrieval costs
			When a blob is on archive storage it is offline and cannot be read
	
	Replication Options 
		LRS - three copies locally in the same data center 
		ZRS -  three copies in a zone based regions, so a couple of data centers in the same region
		GRS - six copies across multiple regions
		RAGRS - read access geo redundant storage that allows us to replicate to another region and actually read data thats basically warms and ready for us to access 

	Manage Permissions/Access
		Container Permissions 
			Access policy specifically to the container - on the container inside your blob storage you can set public access level, the default is private
												Blob - Anonymous read access for blobs only.
												Container- Anonymous read access for container and blobs.
			Shared Access Services (SAS) 
				It is a query string that we add onto the U.R.L. of a storage resource. And this string informs Azure of what access should be granted.
				 Account SAS Tokens
					granted at the account level to grant permissions to services within the account
				 Service SAS Tokens 
					grants access to a specific service within the storage account
						sv - storage service version
						ss - bfqt signed services 
						srt - sco signed resource types  - service, container, object
						sp- rwdlacup -signed permission - read, write, delete, list, add, create, update, process
						se - signed expiry & start
						spr - https signed protocol 
						sig - signature 

Key Management

		Key Vault Overview
			Service in Azure for safeguarding keys and secrets 
			Uses keys that are protected by hardware security modules (HSMs)
			Can import or create new keys
			Can be accessed AAD-Authenticated requests
		
		Use Cases
			Developers: 
				Keeping keys external from applications
					application can make a request say using a service principle to the key vault to gain access to that key and then actually use that key.
				Allowing customers to bring their own keys for Software-as-a-Service (SaaS) apps.
			Azure Admins:
				Storing password in Key Vault which can be referenced during ARM deployments.
				Bring your own storage keys
		
		Key Management Tasks
			Create or import a key or secret
			Revoke or delete a key or secret
			Authorize users to access the key vault 
			Configure and monitor key usage
				
				
SQL Elastic Pool - simple cost effective way of scaling databases for unpredictable usage demands.

?? Centralize storage account logging for all guest level metrics 
?? Compliance Manager ?
			
 No need of public ip if you are connecting to the VM using VPN or express route connection

 ?? Disk Encryption on Linux.


API Management 

	Contains three major components
		API Gateway/ Proxy
		Azure Portal / Publisher Portal
		Developer Portal

	Products are logical containers for API’s and then we can apply authentication or separate policies
	


API DASHBOARD 

MQ For UPM 

For benefits ? Set up there ?

Grafana for upm3/ upm4 services ? - 

xdomain-hcp/service-cdm-constmgmt-ci-issue

https://github.optum.com/xdomain-hcp/service-cdm-constmgmt-ci-issues


https://1drv.ms/u/s!AoJ9Tk1rIDYdhad6bKGc1JnyUcEs3A?e=rCqwhg
https://www.elastic.co/blog/elasticsearch-service-on-elastic-cloud-now-available-on-microsoft-azure
For everyone here... if this totally confuses you, reach out to Martijn Van Overbeek as he's the primary point of contact for all things Virtual Networks.
From Manoj Samgharshanan to Me: (Privately) (10:49 AM)	
Hi, this is Ruchi, I’m your Training Manager. Please call me at 617-517-4143 or email me at ruchi.singh@netcomlearning.com if you have any questions or concerns. Thanks

https://docs.microsoft.com/en-us/azure/cosmos-db/local-emulator
https://docs.microsoft.com/en-us/azure/cosmos-db/data-explorer

https://hub.docker.com/r/microsoft/azure-cosmosdb-emulator/

https://docs.microsoft.com/en-us/azure/architecture/patterns/valet-key

QU2B19V50Y8YMYR8SZ
https://www.microsoftazurepass.com/Home/HowTo

https://www.microsoftazurepass.com/Home/HowTo?Length=5

https://www.bing.com/videos/search?q=api+gateway+in+azure&view=detail&mid=8D4671549AEE7D128A5C8D4671549AEE7D128A5C&FORM=VIRE https://docs.microsoft.com/en-us/azure/service-bus-relay/relay-faq


From Eric Hart to Me: (Privately) (11:26 AM)
 ARIN is your best friend. https://www.arin.net/reference/research/statistics/address_filters/ 
From Eric Hart to Everyone: (05:24 PM)
 We have an internal logging project called Project NESS. Brett Stringham drives this initiative. This will become the standard.

https://medium.com/awesome-azure/azure-difference-between-traffic-manager-and-front-door-service-in-azure-4bd112ed812f 
From Liviu Brezai to Everyone: (04:50 PM)
 http://www.visiocafe.com/microsoft.htm 
 
From Erik Paasonen to Everyone: (04:53 PM)
 our team uses draw.io on Mac, XML based save file: https://www.draw.io 
From Eric Hart to Everyone: (04:53 PM)
 typo... stenciltown.omnigroup.com/categories/all/ 
From gunjan das to Everyone: (04:54 PM)
 Draw.io is pretty good tool , you can install a laptop version and use it 
https://docs.microsoft.com/en-us/azure/architecture/architectures/?filter=reference-architecture&sort=-publish_date

support.omnigroup.com/omnigraffle-import-visio

Azure API Gateway - https://www.youtube.com/watch?v=6msBznWBypc

Valid access token or bearer token. 

examcollection.com
