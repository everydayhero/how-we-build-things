

At everydayhero, security and data-privacy best practices are not an afterthought. We believe that only an end-to-end, layered approach can be effective. Security is ingrained in all processes and decisions, across both engineering teams and the wider business.

It is very common for people to come to us with questions about these practices - everyone from individual donors or fundraisers, right through to our largest enterprise customers. In order to address this demand, we maintain this extensive list of questions that have been asked of us.

### About everydayhero.

> Who are you / what's your address?

```
Everydayhero, Pty Ltd.
333 Ann Street
Brisbane, QLD, 4000
```

> If I can't find the answers I need here, who can I ask?

No problem at all. We appreciate that security questionnaires tend to pass through many non-technical roles during their completion, so we appreciate any insight that allows us to make this obligation less painful for both parties.

Please feel free to send an email with your questions to the everydayhero Tooling Team <tooling-team@everydayhero.com> CCing our CTO, Dan Sowter <daniel.sowter@everydayhero.com>. We'll get back to you as soon as we can, and improve this document with our new answers.

> What sort of data do you collect / manage?

We manage the full lifecycle from introduction & on-boarding through to post-campaign analysis and off-boarding for consumers, charities and events partners.

> Do you store credit cards?

No. Credit-card data never touches our systems in any way.

### Physical & Virtual Infrastructure

> Where are your data centers?

https://aws.amazon.com/. We use the United States East 1 region (North Virginia) for primary systems of record. Content delivery networks perform caching in other regions (Cloudfront).

### Databases & other durable storage
### The software we deliver
### Development practices as they apply to security

> Any outsourced providers?

http://www.itoc.com.au/ for some platform / operations consulting, and Amazon Web Services for managed offerings where available.

### Organizational policies

> Do you have a corporate level Information Security Policy document which is approved, published and communicated as appropriate, to all your employees?

Yes. The policy documents are available on the internal intranet to all employees.

> Do you have an Information Security Policy and procedures, and have implemented the same for the operational facilities (e.g. data centers, field offices) that governs:
a) Internet, email, voice mail and facsimile machines usage by staff
b) Remote access to vendor networks and systems
c) Personnel management including exit and entry procedures
d) Backup, recovery and archival of information
e) Secure operating system and software application configuration and management
f) Access, processing and disposal of customer information
g) Computer security incident response and investigation
h) Security vulnerability notification and remediation
i) Protection against malicious code and viruses
j) Business continuity and disaster recovery
k)    Change management
l)     Physical security

Yes, we have documented information security policies and procedures governing the aforementioned areas. Operational facility compliance (physical layer security) is governed by Amazon Web Services which is a PCI-DSS and ISO 27018 certified provider.

> Do you have an individual in the role of Information Security Manager or have a cross functional forum assigned with the responsibility for protection of individual assets, reviewing information security policy, reviewing and monitoring security incidents, carrying out specific security processes and taking up major initiatives to enhance information security?

Yes. We have a cross functional DevOps guild consisting of 6+ members spanning multiple teams that meets fortnightly.

> Have you established and implemented training and awareness programs to communicate the information security policy and procedures?

Partially. An information security awareness program has been established and is currently being incorporated into HR new employee on-boarding processes.

> Do you have independent reviews or audits to provide assurance that the information security policy has been implemented effectively? If yes, who conducts such reviews and how frequently?

Partially. We are currently undergoing a pre-assessment audit for PCI-DSS compliance by our QSA, Bridgepoint. Processes are in place to review policies annually.

> Do you maintain an inventory of all important information processing systems associated with information assets as a part of your risk management process?

Yes. We maintain an inventory of all information processing systems in our configuration management systems, (Ansible, Terraform and the AWS API).

> Do you have an appropriate set of procedures for the labelling and deployment of assets in both physical and electronic formats?

Yes. All equipment is labelled and asset-tracked by corporate IT. Virtual servers are asset tracked in Ansible.

> Do you conduct verification checks at the time of job applications on your staff for:
a) Satisfactory character references
b) Check of curriculum vitae accuracy
c) Confirmation of the claimed academic and professional qualifications
d) Independent identity check
e) Criminal background check" Partially Appropriate verification checks are performed with the exception of D and E.
Do you make it mandatory for all employees to sign the Confidentiality Agreement at the time of joining the organization?

Yes. A confidentiality clause is part of every employment agreement.

> Have you configured your firewalls, network routers, switches, load balances, name servers, mail servers and other network components in accordance with security standards?  

Yes. We maintain an ongoing best-practices dialog with local AWS representatives, as well as local user-meetups to validate our approach.

> Have you deployed firewalls, filtering routers or similar network segmentation devices between the networks hosting client data and your corporate networks to minimize the potential of unauthorized access?   

Yes. Within our platform network, we utilize multiple segregated VPC's, private subnets and host level EC2 security groups. Production, staging and test environments are further segregated from each other. This platform network is independent and completely isolated from our corporate IT network.

> Are the servers in a separate subnet from the desktops?

Yes. All workstations are in a corporate network which is completely segregated from servers hosted by Amazon. Corporate network servers are similarly isolated from our laptops.

> Have you configured all infrastructure platforms and services for example OS, web servers, database servers, firewalls, routers, switches, etc. according to the industry best practices and configuration guides?  

Yes. We minimise our exposure in this area by making use of managed service offerings. Where we cannot, we use a pull-request / peer review model to guarantee the quality of our configurations.

> How do you prevent execution of  malicious or unapproved programs on servers? Are executables enabled using a whitelist approach?

Yes. We run CoreOS, a minimal operating system with no superfluous services. Each executable is isolated inside a per-process Docker container.

> Do you remove all unnecessary user and system accounts from each system within the organization; what is the frequency of such procedure?

Yes. All user account are controlled by our configuration management tool. Account removal is performed at the soonest possible time.

> Do you restrict user accounts on each system to the operational staff who have job related need to access the system?

Yes. We start with a policy dictating 0 user access, and add only when required.

> Do you have a password policy, does the policy follow any client specified characteristics of minimum length, character set mix, repeated characters, etc.

Yes. Where passwords are used, policies enforce minimum length, complexity, age and password reuse.

> Do you use strong authentication processes for example token-based authentication to access critical business applications or systems, special access privileges, administrative privileges and remote access to systems?

Yes. We use multi-factor authentication, public/private key pairs, encrypted password managers and accounts with minimal privileges.

> How do you ensure that the remote administrative access to production systems is performed over encrypted connections such as SSH, SCP, SSL enabled web interfaces, VPN, etc?

Access to any system is via a hardened bastion host using per-user SSH key pairs. There is no public access via any other method.

> Do you monitor on a regular basis, reputable sources of computer vulnerability information and take appropriate measures to obtain, test and apply relevant service packs, patches, upgrades and workarounds?

Yes, via security mailing lists and automated code quality analysis tools. All changes are tested in a staging environment before deployment to production.

> Do you have a maximum time defined within which critical security fixes or patches must be applied?

Yes. Each security issue is immediately assessed as to its severity, impact, vulnerability and exploitability when defining a timeframe. We replace servers regularly rather than upgrading, ensuring the latest patch levels. All patches are generally applied as soon as possible, but not exceeding a maximum of 30 days from vendor release.

> Do you use any third party / specialist security consulting firms to perform comprehensive security assessments covering policies, procedures, on-site assessment and penetration testing?

Yes, we use Qualys for our automated scanning, and BugCrowd to perform penetration tests and vulnerability reporting.

> Do you maintain log files for firewall, intrusion detection, system log and application logs, can they be supplied to the client on request to support investigations or any legal action?

Yes, all infrastructure access and changes are logged to AWS CloudWatch. Centralised logging is used for host level activity. Logs (appropriately redacted and sanitized) can be made available to our partners and law enforcement authorities as required.

> Are unmanaged BYOD devices able to access information resources, particularly those storing client information?

No, all devices are provided and managed by corporate IT.

> Do you follow software design, development, testing and deployment life cycle methodologies for all software releases; how are information security and information risk management considerations applied?

Yes. We follow an Agile development methodology and apply industry best practices. Security is assessed during each phase of development. A pull-request / peer review model provides a safety-net against single-developer failures. All code changes must also pass automated static analysis (for security vulnerabilities) before they are eligible to be merged.

> How do you ensure that each user of any application is uniquely and unambiguously identified through the use of an identifier e.g. user-ID?

Where the level of authorization granted to a user depends on their identity (ie. Non-public areas) we generate new users with corresponding IDs.

> Are there strong authentication mechanism in place such as token-based authentication for accessing critical business applications, applications with sensitive information, special access privileges and external access capabilities

Yes. Depending on the risk implied by the data held by each application, we have a spectrum of security policies, from strong-passwords, through oAuth 2.0, through keyphrased-SSH-keys, through 2-factor requirements on critical systems.

> Do you ensure that all client information when transmitted between you and user’s web browser, uses SSL or Transport Layer Security Protocol with no less that 256-bit keys?

Yes, all traffic is transmitted via SSL/TLS. Cipher suites and protocols are regularly scanned and updated to maintain an A grade. The latest report can be found at the following URL: https://www.ssllabs.com/ssltest/analyze.html?d=everydayhero.com"

> In cases where production data is used for development or testing, how do you ensure that data integrity is maintained to prevent the disclosure of personal information to unauthorized personnel?  Do you have a data sanitization process?

Testing and staging environments are completely segregated from production and share no data. Developers are unable to access production data for development purposes.

> Who is allowed unescorted access to operational facilities other than authorized resources working on operational related tasks?

No one. We have no physical access to data centers.

> Is the physical entry to sensitive areas like data centre, network room, UPS areas, etc. controlled by electronic card access locks in all instances, is one card used for all areas?

We have no physical access to data centers

> How do you dispose of hardware; desktops, laptops, servers, hard discs or other devices? What procedure is followed to ensure  cleansing of the hardware of confidential information before their disposal, refurbishment or donation?

All laptop drives are encrypted. A secure wipe is performed when disposing of hardware. For platform infrastructure, AWS is responsible for physical security including disposal.

> Do you deploy reputable and industry standard virus and malicious code detection / protection software; is this applied to all workstations and servers within the operational facilities (operational facilities, data centers, offices)?

Yes, McAffee Endpoint Protection is used on all laptops. ClamAV is used on all servers.

> Do you have a detailed and documented plan for responding to  security incidents that includes processes and procedures for the following:
a) Assessing the severity of the incident
b) Identifying the cause of the incident
c) Repairing the cause of the incident
d) Restoration of normal operations
e) Documenting the results of the response"

Yes. A detailed incident response plan is available to all staff. It outlines processes to assess, identify and respond to incidents, escalation policies and emergency contacts. It is updated regularly in response to new scenarios.

> Do you ensure that all systems, applications and data in the operational facilities are backed up in a manner consistent with the business resumption plan of the organization? Are there any exceptions?

All data backup is managed by AWS RDS / ElastiCache / S3 policies.

> What processes do you utilize in order to ensure new activities meet your compliance obligations?

All changes go through a peer review system before implementation.

> Does your organization have a public privacy policy?

Yes, available at the following URL:
https://everydayhero.com/au/terms/privacy/"

> Does your organization have a register of information assets containing personal data?

Yes. All application codebases maintain schema.rb files delineating all tables and fields for their connected databases.

> Does your organization store private information outside of its country of operation? Under which jurisdictions?

Yes. We operate in many countries and jurisdictions. Primary data is stored in the United States.

> Does your organization utilize Cloud infrastructure or services for any systems storing critical or sensitive client information? What Cloud services do you utilize?

Yes. Amazon AWS, Zendesk, Periscope are the most relevant. We are a cloud-native company, and as such make use of dozens of such services. everydayhero never stores any credit card data.

> How has your organization assessed the security of your cloud service providers?

By reviewing their security and compliance policies - eg Amazon's https://aws.amazon.com/security/

>Tell us about the environment in which your solution is installed"
Where is your application/database/website hosted?

Amazon Web Services' US-East-1 region.

> Is this solution on shared infrastructure? If so what is it shared with?

Not shared within the EDH AWS account.

> Are physical or virtual machines used?

Virtual

> Do you have a staging, test, and or development environment?

Yes. https://www.everydayhero-staging.com/au/

> Are there firewalls used in the solution?

Yes.

> How are the firewalls configured for this solution?

AWS EC2 VPC Security Groups, ACLs and subnet segregation.

> Are servers and server types segregated with implemented security controls (Web, App, DB)

Yes. Servers are segregated on role, with unique security groups.

> Is there an anti-virus solution implemented? Provide details including how it is managed.

Yes. We use ClamAV.

> Is there a Intrustion Prevention System implemented (Network and/or Host)? Provide Details

Yes. We use Alienvault.

> What is the technology footprint of solution e.g. IIS, SQL Server, Java, Tomcat, Flash.

Nginx -> Ruby on rails (for the most part)

> Are there any additional security measures
in place that you would like to detail?

Only the AWS Elastic load balancers have public IP addresses. We are currently under a PCI DSS 3.1 QSA-lead audit, and have controls in place to expect an Attestation of Compliance to be supplied in the next few months.

> What Data is used/stored in this solution?

Donor details

> Is Customer Data involved? Please provide details.

Yes, name, address, phone number, email

> Where is the data stored?

AWS-managed services (RDS, ElastiCache etc)

> How is the data secured? (Is the data encrypted and if so how)

It is not encrypted at rest.

> What protocols are used to submit and/or transfer the data? (e.g. HTTP, FTP, SMTP, Telnet)

HTTPS only.

> Who can access the data?

Most of the data is publicly available to all visitors on a supporter page. The remainder is available to users with appropriate authentication and authorisation via the platform, or to senior developers who require access to the supporting infrastructure.

> How do they access the data?

Over https for the platform, via SSH for the infrastructure.

> Can the data be exported or extracted? If so how and to what format?

Yes. It depends on the data which is required. All formats are possible. Some (CSV, Excel) are available as reports, or APIs (JSON). Other formats require developer effort.

> How will the solution protect data being accessed by other participants/customers or non-authorised personnel?

Application and infrastructure users are required to authenticate, and do not share accounts.

> How are backups performed and where are they stored?

AWS-managed services (RDS, ElastiCache etc) back up automatically to S3.

> Are backups secured and if so how?

AWS IAM roles are used to manage access credentials.

> Are Certificates used (if so provide details on their issuance and usage)?

Yes, for HTTPS/SSL. Issued by Trustico.

> Is Email/SMTP being used to transfer information (if so provide details)?

No

> Who has access to the solution?

everydayhero staff, with varying levels of access as is suitable for their role.

> How many users will have access for administration?

Depends which administration area. Application-level admin tasks are segregated by region, and are distint from platform infrastructure.

> Are user and functional accounts periodically reviewed and validated through a revalidation process?

Yes

> How do they access the solution? (e.g. VPN, Out of Band Management)

SSH

> How do they authenticate?

Keypairs

> Is there a centralised authentication system or directory?

One centralised system for application admins, another for infrastructure.

> Is there a password policy and if so what is it?

Yes. Passwords are not allowed for any infrastructure access. 2 factor device + SSH keypairs are required.

> Is there an account lockout policy and if so what is it?

Yes. We use fail2ban, set to 30minutes, notifying the team of an intrusion attempt.

> Are there authorisation restrictions restricting what administrators and/or support personnel can do?

Yes.

> Is there auditing in place to report on access (success/failure)?

Yes.

> Is there auditing in place to report on changes and modification (success/failure)?

Yes.

> Are backups run frequently?

Yes.

> Is there a disaster recovery (DR) or business continuity solution in place should the primary environment fail?

Yes.

> Is there an operating system and/or application security patch management strategy/process defined? (Please provide details)

Yes. All infrastructure and OS configuration is managed as code, deployed by pipeline.

> Is logging performed on any of the solution?

Yes.

> Are logs regularly monitored, reviewed, and where are they logging to?

Yes. All logs are centralised to S3 + ElasticSearch

> Who reviews the logs?

Platform and application engineers.

> Do you have policies and procedures in place describing what controls you have implemented to protect information and systems? If so please describe.

Yes. Our policy document is here - https://docs.google.com/document/d/1z9YmOGt9czsCgGgsIim93DqDu1Uy-ShGWHDd9arNiCI/edit?usp=sharing

> Can you provide partner with documentation describing your Information Security Management Program?

Yes.

> Do you notifiy the partner when you make changes to your information security and/or privacy policies?

No. We prefer a continuous delivery model for all products, including policy documentation. The link above will remain current.

> Do you require strong (multifactor) authentication options for physical access to your facilities?

Single-factor (keycard) on EDH offices. We have 0 physical access to our hosting environment (Amazon Web Services)

> Is access to your facilities logged and audited?

Yes

> What forms of authentication are used for operations?

SSH via private / public keypairs for unique users.

> Is two-factor authentication used to manage critical components within the infrastructure?

Yes. Both AWS console and SSH access require 2-factor auth.

> Do any accounts have system-wide privileges for the entire cloud system and, if so, for what operations (read/write/delete)?

Yes. Some AWS users have full infrastructure admin rights.

> How are the accounts with the highest level of privilege authenticated and managed?

AWS IAM users, with password and 2-factor policies, and fortnightly review by the EDH devops-guild.

> Do you regularly audit and review administrator access? If so please describe.

Fortnightly, by the EDH devops-guild (5+ members spanning multiple teams).

> Do you log and audit your staff privileged access and activities? If so please describe.

Via the AWS CloudTrail service for infrastructure. Via centralised logging for host-level activity.

> Do you document how you grant and approve access to tenant systems or data?

Via pull-request. All configuration is managed as code, and updated by a pipeline.

> Do you provide anomaly detection (the ability to spot unusual and potentially malicious IP traffic and user or support team behaviour)? For example, analysis of failed and successful logins, unusual time of day, and multiple logins, etc.

Yes - regularly reviewed Kibana dashboards, and automated thresholds in Alienvault.

> How are the detection reports communicated to the partner?

Via your campaign's account manager.

> What provisions exist in the event of the theft of a partner’s credentials (detection, revocation, evidence for actions)?

Partners are not provided credentials.

> Does the system allow for a federated IDM infrastructure? Do you support ADFS?

No. OS users are managed by our ansible pipelines.

> Provide a description of all levels of user access that are available. What are the reports that are available? Provide a sample of the report.

All servers are accessed via the bastion host. 32 users exist on the server today (including robot accounts for pipelines). 15 are members of the sudoers group.

> How often are these reports available?

Always. It's declarative code on github, not a survey / observation.

> For access violations identified, provide the documented process for remediation that is followed.

We don't remove access. We provision replacement infrastructure.

> What checks are made on the identity of user accounts at registration? Are any standards followed?

Peer-review. A new user is a code-change.

> What processes are in place for de-provisioning credentials?

We don't remove access. We provision replacement infrastructure.

> Do you have a documented security incident response plan?

Yes.

> Do you integrate partner requirements into your security incident response plans?

No.

> Do you publish a roles and responsibilities document specifying what you vs. your tenants are responsible for during security incidents?

No.

> What is the process for reporting any compromise of customer, user directory, personally identifiable or credit card data?

You will be notified via your account manager.

> Do you have a formal process in place for detecting, identifying, analyzing and responding to incidents?

Yes.

> Do you provide a periodical (and/or upon request) report on security incidents?

Upon request.

> Do you have the ability to sanitise all computing resources of tenant data once a customer has exited your environment?

Our networks are single-tenant.

> Do you have controls in place to prevent data leakage or intentional/accidential compromise between tenants in a multi-tenant environment?

Our networks are single-tenant.

> Can you provide the physical location/geography of storage of a tenant’s data upon request?

All data is stored in the AWS US-East-1 region.

> Do you carry out penetration testing? How often? What is actually tested during the penetration test?

Annually. The tests are at the discretion of the white hat researchers - we do not restrict their acceptable attack methods.

> Can the partner conduct its own penetration testing and vulnerability testing against your environment and the hosted partner application?

Pen-tests must be pre-arranged, but can be accommodated. We will not host a partner application.

> Do you (or a third party) conduct internal audits regularly?

Yes, fortnightly.

> Do you (or a third party) conduct external audits regularly?

Yes, quartery.

> Do you (or a third party) conduct network penetration tests of your cloud service infrastructure regularly?

Yes, quartery.

> Do you (or a third party) conduct application penetration tests of your cloud service infrastructure regularly?

Yes, annually.

> Do you allow tenants to view the results of your or third party audit reports?

Only with an NDA in place.

> Will you make the results of vulnerability scans available to tenants at their request?

Only with an NDA in place.

> Define the host and network controls employed to protect the systems hosting the applications and information for the partner. These should include details of certification against external standards (e.g., ISO 27001/2).

Network (VPC + private subnets) + Host (EC2 VPC security groups)

> Detail policies and procedures for backup of the host, web application and partner data. This should include procedures for the management of removable media and methods for securely destroying media no longer required.

No physical access exists. All data backup is managed by AWS RDS / ElastiCache / S3 policies.

> Detail your change control procedure and policy.

Code > pipeline. Humans can't make changes directly.

> How are audit logs reviewed? What recorded events result in action being taken?

Manually by a member of our devops-guild. At the member's discretion - a spike in sudos or new users would be a classic example.

> Is it possible to segment data within audit logs so they can be made available to the partner and/or law enforcement without compromising other customers and still be admissible in court?

We are single-tenant, and comply with law enforcement requests as required.

> Is there a staged environment to reduce risk, e.g., development, test and operational environments, and are they separated?

Yes, and they are on separate networks.

> Specify the controls used to protect against malicious code.

Peer review + static analysis.

> What method is used to check and protect the integrity of audit logs?

AWS CloudTrail audits all S3 / Lambda events associated with logging.

> What time source is used to synchronise systems and provide accurate audit log time stamping?

AWS manages ntp servers for all systems.

> What encryption techniques are used for data in transit? Describe in details the use of algorithms; key length; and key management.

TLS / SSL  - https://www.ssllabs.com/ssltest/analyze.html?d=everydayhero.com

> What encryption techniques are used for data at rest? Describe in details the use of algorithms; key length; and key management.

None.

> Do you ensure virtual images are hardened by default?

Yes. Our ansisble pipelines manage this.

> Is the hardened virtual image protected from unauthorized access? How?

Yes - at the network level, and by OS configuration (users + groups)

> Is the host firewall run with only the minimum ports necessary to support the services within the virtual instance?

Yes.

> Request information on how multi-tenanted applications are isolated from each other – a high level description of containment and isolation measures is required.

We are single-tenant.

> Can you ensure that the patch management process covers all layers of the cloud delivery technologies – i.e., network , server operating systems, virtualisation software, applications and security subsystems?

Code pipelines drive every level of the stack. Network, OS, host configuration, application containers, host agents.







Is the payment card provider PCI DSS compliant in terms of their payment services and has completed either an SAQ or ROC?
What version of the PCI regulation are they accredited?

We are currently undergoing a PCI DSS 3.1 compliance pre-audit, submitting a ROC. It is important to understand that everydayhero does not store card-holder data at any time.
What period is covered by the PCI DSS compliance?
Expiry will be documented on our as-yet-unissued AOC. I assume it will be valid for 12 months from issue.
What is the service provider PCI level (1 or 2 based on transactions)?
Level 3 in any single region, level 2 when all regions are considered in aggregate.
How will the NSPCC consume the payment service (e.g. e-Commerce website, API, PDQ, MOTO, etc.)?
e-Commerce website.
If supplying payment card machines please specify the make and models. Include rental period.
N/A
Can the provider supply a dataflow or network diagram showing the customer’s interaction with the PCI service? Please provide.
Yes (attached).
Does the provider ensure transmission and storage of card holder data is encrypted?
Yes, through our payment gateway provider agreements. Again, it is important to understand that everydayhero does not store card-holder data at any time.
Will card holder storage, processing, and transmission remain out of scope of the NSPCC’s environment?
Yes.
Does the PCI service include fraud prevention?
Yes, through our payment gateway provider agreements.
How frequent are security scans on the provider’s network perimeter?
Quarterly.
Has the provider experienced any breaches of card holder data (last 12 months)? And how was this rectified?


No.
Is the provider registered under the data protection act?  Provide reference no.
No.


Is the supplier registered under the Data Protection Act? If so provide reference no.
Yes

Everyday Hero Limited registered Z1791934
Is the supplier accredited to the ISO 27001 Security standard?

No

Does the supplier run yearly compulsory security awareness training to all staff?

No
Security guidelines are available on our Intranet and are updated regularly
Will the supplier or its sub-contractors host our information outside the EU?
Yes

Data is hosted on AWS (Amazon) – who are party to the Safe Harbour Agreement
If the supplier or it sub-contractors plan to take credit card payments on our behalf are they
PCI compliant?
Yes

The everydayhero environment has recently moved through the threshold where self-assessment has now been surpassed. An external audit has been undertaken during August and the results will be shortly known.
Does the supplier have a responsible attitude to the Copyright, Designs and Patents Act (1988)?
Yes


Will the supplier remove all our data from their systems once our engagement is completed?
Yes


Security Policy



Does the supplier have an information security policy which has been communicated to all their staff? Please send a copy.
Yes


Does the supplier review its security policy on yearly basis?
Yes


Organization and Information Security



Does the supplier have someone appointed in the role of Security Manager. And are staff aware of their information security responsibilities?
Yes


Does the supplier use a 3rd party supplier to host their information processing facilities and have they verified that the 3rd party has adequate information security measures in place?
Yes


Asset Management



Does the supplier clearly define information assets and assigned these to owners who are responsible for maintaining their protection?


Unclear on the specific type of information assets you refer to here.
Does the supplier have an information classification system to ensure that information receives the appropriate level of protection?

No
Across the company we operate with a wide range of information, both internal and customer information.  In each case, information is handled appropriately and sensitively by appropriately skilled staff.
Human Resource Security



Does the supplier put employees, contractors and sub-contractors through strict pre-employment vetting, including reference and security checks?
Yes


Does the supplier ensure that employees, contractors and third parties exit the supplier or change employment in an orderly manner?
Yes


Physical and Environmental Security



Does the supplier implement security measures to prevent the unauthorised access to their premises, as well as, secure areas within their offices?
Yes


Does the supplier have measures to prevent the loss, damage or theft of equipment which may result in an interruption to the supplier’s activities?
Yes


Communications and Operations Management



Does the supplier follow a formal IT service management process such as the IT Infrastructure Library (ITIL) to run its IT operations?

Yes


Does the supplier follow a formal change process to assess the impact of planned changes to its business?
Yes


Does the supplier employ network controls such firewalls and intrusion detection systems to protect their systems?
Yes


Does the supplier employ anti-virus software to protect computers and network gateways?
Yes


Does the supplier regularly patch computers on its estate to protect them against threats?
Yes


Does the supplier ensure that they use legitimate software?
Yes


Does the supplier run a formal backup procedure to maintain the integrity and availability of their information processing facilities?
Yes


Does the supplier employ tools to exchange information with 3rd parties in a secure manner?
Yes


Does the supplier regularly perform Internet penetration tests on their network?
Yes


Access Control



Does the supplier employ complex passwords for user accounts and require users to regularly change their passwords?
Yes


Does the supplier ensure that users only have access to the information they are authorised?
Yes


Does the supplier ensure that users cannot make unauthorized changes to computers or applications?
Yes


Does the supplier have a process to ensure that when people leave the organization their access to systems is disabled or removed?
Yes


Information Systems Acquisitions, Development and Maintenance



When the supplier collects information on our behalf over the Internet are cryptographic controls employed to protect the collection of this information?
Yes

All traffic is over https / SSL.
Does the supplier ensure that applications used to process our information do not introduce any errors or allow information to be misused, and the technology kept up to date?
Yes

This is achieved through a combination of peer-review, and automated static code analysis.
Does the supplier develop their own web applications to collect and process our data and therefore make sure those applications are developed with information security in mind?
eg. OWASP guidelines.
Yes

Compliance with OWASP guidelines is enforced as part of our ongoing PCI compliance obligations.
Information Security Incident Management



Does the supplier have a defined process for managing information security incidents?
Yes

As per PCI compliance.
Does the supplier intend to report any information security incidents impacting NSPCC information immediately to the NSPCC?
Yes

Any incidents will be raised with you via your account manager.
Business Continuity Management



Does the supplier have a business continuity plan to protect the business from major failures and to ensure timely resumption?

Yes

A1
How is management’s direction and support for information security demonstrated to staff?
•	Is there a documented security policy?
•	How are staff made aware of the Security Policy?
•	What compliance procedures are in place to ensure the policy is read, understood and implemented?
•	Outline management responsibilities for security.
•	Clear definition of roles and responsibility for security within the organisation structure.
•	Organisation security forum with appropriate Senior Management representation.
All members of staff undergo security training as part of the onboarding process, with subsequent training as the require increased access to our platform data. It is important to understand throughout these responses that everydayhero maintains a strict separation of corporate IT (office network, laptops, emails, internet access etc) versus platform (servers, databases, remote access etc).

There is no connectivity between the two networks, no user-federation, no privileges afforded to people simply for being any employee or on our office network. The answers supplied will speak to the security of the platform (given this is the network responsible for the security of Fairfax data). As such, many questions are not applicable, because the underlying assumptions are invalid.

Information security (and staff access) is enforced by AWS’s IAM service. Levels of access by individual staff members are reviewed every 2 weeks at the meeting of our internal “Devops Guild”.

Reference
Question
Answer Guidelines & Examples
A2
What security controls do you have in place for any outsourced operations?
•	Use of monitoring reports and compliance returns.
•	Regular on site reviews of the outsourced operation.
•	Regular service monitoring meetings with the vendor.
•	Security requirements explicitly detailed in the service contract, including an Incident Response / Threat Management process and Security Vulnerability Management process
Where 3rd parties require access to our data, they go through our normal devops-guild audit process, with access revoked at the end of their work. It’s worth noting that 3rd party access or effort is extremely rare.

Reference
Question
Answer Guidelines & Examples
A3
How do you ensure that the security policy, standards and procedures are up to date?
•	Regular management review.
•	How often is the Information Security Policy reviewed?
•	What is the process for review?
We conduct fortnightly review.

Reference
Question
Answer Guidelines & Examples
A4
How are threats & vulnerabilities managed?
•	Risk assessments undertaken
•	Maintenance of an asset inventory and data classification to ensure appropriate handling/storage/destruction of documents etc.
•	Appropriate insurance in place for IT-related risks
Our company shares access to a single instant-messaging application, called Slack. Our operations, engineering and development teams each maintain a running dialog of threats, coordination resolution across company concerns.

Eg. An SLL vulnerability is acknowledged in the wild. The devops team secure our servers, load-balancers etc, then fan-out responsibility that belongs inside any applications that are identified as vulnerable to the responsible team, holding their hands through the deploy process until the all-clear is given.

A5
What is the coverage of safeguards in your policy? Is it comprehensive enough to cover all aspects of security domains in ISO27001?
•	Please tick the appropriate items
Response (Completed by THIRD PARTY)

[x] Information classification
[x] Security roles, responsibilities and training
[x] Information protection
[x] Privacy policy
[x] Internet use
[x] Email usage
[x] Encryption use
[x] Application security

[x] System security
[x] Network security
[x] User access management
[x] Remote access
[x] Business partner connectivity
[x] Physical security
[x] Termination policy
[x] Incident handling


Not sure of ISO27001 compliance.



1.1.	Personnel Security
Reference
Question
Answer Guidelines & Examples
B1
How do you ensure prospective employees are appropriately vetted for sensitive jobs?
•	Take up employment references / other verification of previous employment
•	Obtain evidence of stated academic and professional qualifications
•	Credit reference checks
•	Criminal and Police record checks
•	Independent identity checks (e.g. passport)
All new hires are vetted by HR, who reach out to their previous employers.

Reference
Question
Answer Guidelines & Examples
B2
How do you ensure that security responsibilities are addressed by staff?
•	Comprehensive employment contracts including:
•	Confidentiality clauses
•	Reference to security responsibilities
•	Penalties / disciplinary proceedings for non compliance
•	Regular staff compliance returns for security responsibilities and other legal and regulatory requirements
Security clauses are incorporated into our employee contracts.

Reference
Question
Answer Guidelines & Examples
B3
How do you ensure that security responsibilities are addressed by third party contractors/temporary employees?
•	Comprehensive employment contracts including:
•	Confidentiality clauses
•	Reference to security responsibilities
•	Penalties / disciplinary proceedings for non compliance
•	Regular staff compliance returns for security responsibilities and other legal and regulatory requirements
Security clauses are incorporated into our 3rd party contracts.

Reference
Question
Answer Guidelines & Examples
B4
What measures do you (third parties) have in place to ensure no over reliance on key personnel?
•	Adequate backup of all key roles and responsibilities.
•	Fully documented procedures.
•	Succession planning.
Succession planning, and pair-programming.

Reference
Question
Answer Guidelines & Examples
B5
How do you ensure staff have appropriate skills and training to support Fairfax services?
•	Formal Training Courses,
•	Mentoring
•	On the job training
•	Certification programme
On the job training and mentoring, supplemented by training courses as required.

Reference
Question
Answer Guidelines & Examples
B6
What controls do you have covering employee resignation or dismissal?
•	Termination procedure covering removal of access to buildings, systems etc.
•	Automated feed from payroll/HR into department responsible for revoking physical access to building and logical access to company systems.
•	Regular review of system access rights.
•	Automated expiry date on contractor site and system access.
•	Termination procedure covering removal of access to buildings, systems etc.
•	Automated feed from payroll/HR into department responsible for revoking physical access to building and logical access to company systems.
•	Regular review of system access rights.
•	Automated expiry date on contractor site and system access.
These apply at both the corporate IT and platform-access levels.

Reference
Question
Answer Guidelines & Examples
B7
Are service provider personnel provided with training to increase awareness of information security obligations and initiatives?
•	Security awareness training mandatory
•	What training is provided
•	Are employees required to go through Information Security training on a regular training?
•	Security awareness training is mandatory



1.2.	Physical Security

Reference
Question
Answer Guidelines & Examples
C1
What locations are used to provide the service to Fairfax
•	Country/city
•	Are these locations single tenant or multi-tenant – please give a description
Physical security is maintained by Amazon Web Services. Details are here - http://aws.amazon.com/security/

Reference
Question
Answer Guidelines & Examples
C2
How do you prevent unauthorised access, damage and/or interference to business premises?  
•	Access should be controlled at all times.
•	All access should be authorised and recorded by designated Management.
•	Visitor access should be authorised, justified and supervised.
•	How are visitors identified to staff
•	What logging or tracking of visitors is performed? Is this reviewed for compliance – when and how is it reviewed?
•	24x7 on-site security guards
•	CCTV monitoring of external perimeter and external access points
•	No externally facing windows for computer facility or sensitive processing areas.
•	Sensitive processing areas segregated with additional access controls
•	The location of the site should be fit for purpose.
Physical security is maintained by Amazon Web Services. Details are here - http://aws.amazon.com/security/

Reference
Question
Answer Guidelines & Examples
C3
How do you prevent unauthorised access, damage and/or interference to business equipment and/or Fairfax processing facilities?
•	Adequate fire protection should be implemented.
•	Physically segregated access zones within the building.
•	CCTV coverage.
•	Access control mechanisms (e.g. card swipe systems, biometrics etc) including the authorisation, review and revocation process.
•	Access to the computer room(s) restricted to authorised persons.
•	Access by external personnel (service and telecom engineers, cleaners etc) restricted and supervised.
•	All servers and network components should be racked appropriately unless self-standing.
Physical security is maintained by Amazon Web Services. Details are here - http://aws.amazon.com/security/

Reference
Question
Answer Guidelines & Examples
C4
What environmental controls are in place to protect the computer facility?
•	Air conditioning with heat and humidity sensors.
•	Gas dump fire suppressant system with roof and floor void sensors.
•	Flood detection systems.
•	VESDA
•	Uninterruptable Power Supply (UPS).
•	Backup generator(s).
•	Not located in a flood zone.
•	Dual power feed and resilient network connections
•	Cables should be properly trunked and secured.
Physical security is maintained by Amazon Web Services. Details are here - http://aws.amazon.com/security/

Reference
Question
Answer Guidelines & Examples
C5
How do you ensure the environmental controls remain effective and operational?
•	Building Management System.
•	24/7 monitoring.
•	Scheduled maintenance.
•	Onsite engineers.
•	Adequate spare parts maintained on site.
•	Maintenance contracts kept up to date.
Physical security is maintained by Amazon Web Services. Details are here - http://aws.amazon.com/security/



1.3.	Media & Data Security and Disposal
Reference
Question
Answer Guidelines & Examples
D1
How do you ensure the safe handling of Fairfax provided data?
•	How will Fairfax data be secured in transit, storage, use and disposal?  
•	What is your policy on the classification and safe handling of third party (e.g. Fairfax) data.
•	What are the media handling procedures?
There is no physical “handling”. All data is transmitted to and from AWS over SSL.

Reference
Question
Answer Guidelines & Examples
D2
What security controls will be used to ensure that reports and aggregated data are securely transmitted to Fairfax (If Applicable)?
•	Access control over reporting modules provided to privileged Fairfax staff only?
•	Secure courier delivery of any printed material?
There is no physical “handling”. All data is transmitted to and from AWS over SSL.

Reference
Question
Answer Guidelines & Examples
D3
How do you ensure the secure disposal/destruction of Fairfax provided data?
•	What methods of permanent data erasure are used?
•	Physical destruction of the media e.g. shredding, incineration.
•	Use of a specialist third party with appropriate contract.
•	Use of secure data destruction software.
Secure destruction is a concern of Amazon Web Services. Details are here - http://aws.amazon.com/security/



1.4.	System Management
Reference
Question
Answer Guidelines & Examples
E1
How do you ensure the required service is provided at the right time and for the agreed duration (e.g. 24*7)?
•	An SLA will be agreed with Fairfax prior to commencement of the service, specifying Fairfax’s requirements, change control procedures etc.  
•	Frequency of review of compliance with the SLA.
•	All outages or reduced service delivery must be advised to Fairfax.
If the platform must go offline, there'd best be a very good reason. Losing a datacenter isn't good enough; we're now spread over three. In Amazon terms, we call these "availability-zones" or AZs. They are geographically isolated, with their own power grids, flood plains and physical access restrictions.

Every application that contributes to the availability of the platform (and as of writing, we're up in the high 20s) is run in a minimum of two AZs, with sufficient capacity to handle the platform load should its counterparts fail.

In the storage layer, we have handed over control of nearly all databases (in their many flavours) to Amazon's managed service offerings. Operations that took our engineers days to plan and execute are now a mouse-click, and a 2 minute automatic failover. All databases are backed up to S3 (99.99999% durability guarantees) on a regular recurring schedule, and operate across multiple AZs, minimising disruption in a failure scenario.

Coupling this new, robust architecture with a proactive, state-of-the-art monitoring and alerting system, we are making continual improvements to the availability of our platform, and increasing transparency and accountability when we stumble.


Reference
Question
Answer Guidelines & Examples
E2
Will the equipment and services be for the sole use of Fairfax?
•	Identify any equipment that will be used in the provision of the service that will not be used solely for providing the Fairfax service.
No.

Reference
Question
Answer Guidelines & Examples
E3
How do you ensure the ongoing service availability in the event of a system failure?
•	Regular backups taken that cover different time periods e.g. daily, weekly, monthly backups.
•	Where are backups stored?
•	Timing and frequency of backups to ensure optimum resilience and ability to recover within business acceptable timeframes
•	Protection of backup media whilst on-site, off-site (secure fireproof safe) and in transit.
In the storage layer, we have handed over control of nearly all databases (in their many flavours) to Amazon's managed service offerings. Operations that took our engineers days to plan and execute are now a mouse-click, and a 2 minute automatic failover. All databases are backed up to S3 (99.99999% durability guarantees) on a regular recurring schedule, and operate across multiple AZs, minimising disruption in a failure scenario.


Reference
Question
Answer Guidelines & Examples
E4
How do you ensure the system is sized to meet the service levels?
•	Capacity requirements monitored and regularly reviewed and systems and networks scaled accordingly.
We make extensive use of Amazon's Elastic Load-Balancers, and a separate routing layer in front of all our services.

This means that as more traffic starts showing up for any particular event, which may affect different platform services depending on the country, we can add and remove capacity seamlessly
    - without requiring downtime
    - addressing the bottleneck, not the whole platform
    - making use of provisioning APIs and the latest containerization technology to get new resources up and running quickly

[Amazon-managed databases](http://aws.amazon.com/rds/) give us a convenient "get bigger, go faster" knob, to accomodate scaling events that can't be satisfied in the application layer

We're complementing these infrastructure-level scaling advantages with application-architectural changes to scale even further

Reference
Question
Answer Guidelines & Examples
E5
What processes and/or procedures are in place to manage system problems?
•	Alerting and monitoring should be in place 24x7.  
•	Procedures should include advising Fairfax or any significant alerts, security breaches or other incidents.
•	Escalation and response procedures should be in place.
•	Faults should be logged, investigated and rectified.
We have automated alerting and incident-raising systems in place 24x7.

These encompass service level health monitoring, through escalation processes, through to post-mortems in the event of an incident.

Reference
Question
Answer Guidelines & Examples
E6
How will system changes to the Fairfax service be managed?
•	The third party should have named contacts at Fairfax who are responsible for authorising any changes.
•	Backout procedures must be detailed prior to implementing any change.
•	All changes must be adequately tested in a test environment prior to implementation in production.
•	Procedures must be in place to record and manage problems through to resolution.
•	Change control procedures must be agreed and documented.  These change procedures should detail how and when changes are executed.  This should include an emergency change process.
Change-management is via the standard Continuous Delivery pipeline method. Code > Pull request > Automated tests > Merge (by 2nd party) > More automated tests > deployed to QA environement > approved for production by QA person > deployed to production > more automated tests.

Reference
Question
Answer Guidelines & Examples
E7
How is security administered on your systems?
•	Security administrators should not be end users.
•	Security administration procedures and authorisation processes must exist.
•	There should be a segregation of duties between roles e.g. developers do not have administration responsibilities for live services.
•	Role and responsibilities should be clearly documented.
•	Audit trails maintained of administrator access which are subject to independent review
All users and system level privileges (like all of our server configuration) are maintained in code, with the corresponding audit-trail that comes with our version control system, git. Furthermore, AWS changes are documented in http://aws.amazon.com/cloudtrail/

Reference
Question
Answer Guidelines & Examples
E8
How is system patch and vulnerability identification managed?
•	Use of vulnerability alerting services.
•	Mailing lists from application/operating system suppliers of vulnerability fixes and patch releases.
•	Risk assessment process
•	Defined CERT process
•	What procedures are in place to ensure that the live system is not adversely affected by any fix/patch?
•	Our devops team members subscribe and contribute to the various mailing lists.
•	Our applications undergo static analysis to scan for vulnerabilities before they are deployed.
•	We subject the platform to regular 3rd party penetration testing.

After a vulnerability is identified, system-configuration changes are pushed through the same continuous deployment pipeline as our applications – thus, system changes are tested first in staging, manually, QAed, then promoted to the production load pool.

Reference
Question
Answer Guidelines & Examples
E9
User Sign-on
Describe how your employee authenticate
•	Does each employee have a unique credential
•	Do multiple users share accounts or logins to systems using the same user ID
Every user has unique credentials. Sharing SSH keys is expressly forbidden.

Reference
Question
Answer Guidelines & Examples
E10
Describe your application test strategy
•	Where is application testing – in a test environment or production
•	How do you handle test accounts in production?
•	Are test accounts allowed in production
•	How are test accounts monitored
Continuous Delivery pipeline method. Code > Pull request > Automated tests > Merge (by 2nd party) > More automated tests > deployed to QA environement > approved for production by QA person > deployed to production > more automated tests.

Test accounts are not allowed in production.
Production data is not made available for staging / test environments.




1.5.	Legal & Regulatory Compliance

Reference
Question
Answer Guidelines & Examples
F1
How does your organisation ensure compliance with the legislative and regulatory requirements that impact your business and the Fairfax proposal?
In this contract, the primary legislation is the Privacy Act.

•	Describe any specific controls and procedures to ensure compliance with the Privacy Act. planning.
Our privacy policies and terms / conditions are supplied by Blackbaud’s legal department. We undergo PCI auditing on a regular basis.

Reference
Question
Answer Guidelines & Examples
F2
What internationally recognised standards are the information security practices compliant with or adhered to?

•	ISO27001/2
We do not seek formal adherence to any standards.  

Reference
Question
Answer Guidelines & Examples
F3
Is your organisation PCI compliant (If Applicable)?

•	Are you on the list of compliant service providers
•	Are you working towards compliance – to what degree are you compliant
We are working towards a QSA-led audit in July 2015, during which we expect to be found compliant.

Reference
Question
Answer Guidelines & Examples
F4
Do you have any internal/external reviews of your security program and processes?

•	For example, Internal Audit, Independent SAS70/ISAE 3402 review etc.
We have quartery ASV scans by a 3rd party. Later this year we have a full https://bugcrowd.com/ penetration test scheduled.

Reference
Question
Answer Guidelines & Examples
F5
Has your organisation had any litigation or legal enforcement action against you that pertain to data privacy or security?


No.

Reference
Question
Answer Guidelines & Examples
F6
Is Fairfax notified when:
•	Litigation or legal enforcement action is brought that pertains to data privacy or security
•	The Australia court system or a government authority requests for disclosure of Fairfax information
•	Describe any specific controls and procedures to ensure compliance with the Privacy Act.
Yes.




1.6.	Contingency Resilience

Reference
Question
Answer Guidelines & Examples
G1
A business continuity management process should be implemented to reduce the disruption caused by disasters, incidents and/or security failures.
•	Named individual with overall responsibility for business recovery/continuity management.
•	The business recovery plans should be documented.
The devops guild maintain an on-call roster of people responsible for system recovery in the event of any incident, with 2nd tier escalation onto the development teams as required.

Reference
Question
Answer Guidelines & Examples
G2
With what frequency are contingency and business recovery reviewed?
•	With what frequency are plans reviewed?
•	How often are plans tested
•	How are the plans tested
•	Services should be fully resilient unless Fairfax has confirmed in writing that this is not required.
•	Services should be contingent unless Fairfax has confirmed in writing that this is not required.
•	Contingency should be provided at a separate geographical location which should comply with all controls detailed in this checklist.
Continuous improvement. The guild meets fortnightly, and incidents are followed by a post-mortem for sharing lessons-learned.


1.7.	Platform Security

Reference
Question
Answer Guidelines & Examples
H1
Are server builds standardised and hardened to an appropriate level?
•	Documented platform security standard(s) covering the level of hardening implemented
e.g.  all unnecessary and redundant network services, devices, processes, protocols, system & network utilities, programs & accounts, are disabled/removed; all operations/services should be running with minimum privileges required;  appropriate file system security should be applied.

Strong user account and password controls (min length, max length, failed attempts, history, lockout etc).

The configuration settings should be defined based on the 'least privilege' principle.
All server configuration is automated, incorporating security best-practices. We start with the minimal-possible operating system (CoreOS), and add only that which is absolutely required. Password-based access is forbidden entirely.

Reference
Question
Answer Guidelines & Examples
H2
How do you ensure that servers etc are built to a consistent and secure configuration?
•	Technical security standards should be in place detailing standard server builds, firewall configuration etc.
•	Hardening scripts applied
•	Vendor supplied utilities to check and assess security configuration
•	Penetration testing by the organisation and/or external experts.
Machines are ONLY built by our automated tooling. They also have very short lives – we don’t update or repair servers. We destroy them, and build a replacement.

Reference
Question
Answer Guidelines & Examples
H3
How is access to information, and business processes controlled?

•	On the basis of business and security requirements, with the principle of “Least Privilege”?
•	Are all user and administration accounts unique, justified, authorised and regularly reviewed?  
•	Are default accounts deleted or disabled?
•	Are all default passwords changed?
•	Is significant activity logged, stored and reviewed?
•	Is access to audit trails restricted?
•	Are appropriate password controls implemented, for example minimum password length, password expiry and password history?
•	What is the process for disseminating user accounts and passwords to users?
•	Are user accounts created with the minimum privileges required to fulfill their role?
•	Are privileged accounts (e.g. root) used only under change control procedures and not for day-to-day system operation?
•	Is all access logged and reviewed?
•	What monitoring exists to identify any misuse or unauthorised access?
Least privilege is applied rigorously.
Review is fortnightly
All activity is centrally-logged, and monitored.

Reference
Question
Answer Guidelines & Examples
H4
How do you ensure data integrity and confidentiality?
•	All data/systems risk assessed and classified.
•	All data secured and only accessible by authorised parties.
•	Outline controls in place so only authorised users permitted access to data.
•	Cryptographic controls securely managed where implemented.  Procedures documented and key changes made under dual control.
We are secure by default – we have separate networks, and VERY limited user access to production data.

Reference
Question
Answer Guidelines & Examples
H5
How do you ensure the integrity of your computing infrastructure?
•	Anti-virus software installed on servers and PCs where appropriate.
•	Anti virus software kept up to date with latest anti virus signatures.
•	How is anti-virus/malware updated – process and frequency
•	Integrity checking software (e.g. Tripwire) implemented
CoreOS + central logging makes this a simple exercise.

Reference
Question
Answer Guidelines & Examples
H6
What procedures are in place for remaining up to date with system and security fixes, performing adequate testing and applying to production servers?
•	Subscribe to vendor and security mailing lists.
•	CERT with responsibility for risk assessing security vulnerabilities and developing actions plans.
•	Documented Security Vulnerability Management process.
•	Emergency change management process.
•	Test systems available to verify patches and system upgrades.
•	Technical staff on 24x7 standby
We don’t “patch” servers. We build new ones, replacing the old. This happens continuously.

Reference
Question
Answer Guidelines & Examples
H7
Are system configurations maintained in accordance with vendor recommendations?
•	Documented system configuration maintained, with version & change control.
•	Backup of initial system install and subsequent upgrades maintained offsite.
Configuration is stored in git.

Reference
Question
Answer Guidelines & Examples
H8
What integrity checking over critical system is   implemented to detect/prevent malicious and/or accidental changes?
•	Host based Intrusion detection system.
•	Integrity checking software.
•	System monitoring scripts.
All hosts have low-level monitoring, centrally aggregating all activity.

Reference
Question
Answer Guidelines & Examples
H9
If you are hosting applications, is the hosting environment physically separate from your corporate environment?
•	Separate Internet connectivity
•	No shared equipment such as firewalls
•	If not, please describe the combined environment in detail?
Yes, completely separate.



1.8.	Application Security

Reference
Question
Answer Guidelines & Examples
I1
Is access to information and business processes controlled on the basis of business and security requirements?
•	Users permissions have been restricted to only the systems they need to do their jobs
•	Application access is via authorised accounts and subject to strict password controls
•	User accounts are regularly reviewed to ensure access is still required and is appropriate to the job role
Yes, with regular review.

Reference
Question
Answer Guidelines & Examples
I2
Are appropriate password controls implemented?
•	Password complexity controls implemented (mix of alpha and numeric characters, upper and lower case, special characters etc)
•	Minimum password length
•	Password expiry
•	Password history
•	Lockout after n attempts
Yes, with regular review.

Reference
Question
Answer Guidelines & Examples
I3
What mechanisms are in place to ensure all significant activity is logged, stored and reviewed?
•	What system logs are kept and reviewed
•	Restricted access to the audit logs produced
•	Separate audit log server
•	Access not available to audit logs for end users
•	Full audit trail maintained for all additions/amendments/deletions and/or other transactional activity
•	Independent review of audit logs
•	Audit trails retained according to business and/or regulatory requirements
All server and application logs are centrally aggregated, with secure access.

Reference
Question
Answer Guidelines & Examples
I4
What controls ensure that all data is secured and only accessible by authorised parties?
•	Strict Database controls
•	Strict operating system file/directory level controls
•	Encryption of authentication data and all other high risk data
•	Recognised encryption algorithms with appropriate key lengths
•	Encrypted backups
All of the above recommendations apply.

Reference
Question
Answer Guidelines & Examples
I5
What procedures are in place for remaining up to date with application and security fixes, performing adequate testing and applying to production servers?
•	How are application security vulnerabilities identified & tracked?
•	How frequently are security fixes and application / operating system patches applied?
•	How do you ensure a patch/fix will not adversely impact the live service?
•	Do you subscribe to vendor and security mailing lists?
•	Is a documented Vulnerability Management process followed?
•	Is a documented emergency change management followed?
•	Are patches and system upgrades tested prior to implementation onto production systems?
•	Are support staff available 24x7?
All of the above recommendations apply. Applications are shipped as docker containers (ie. They bring their own system dependencies, rebuilt on every deploy). As such, all system dependencies are tested as part of PR > merge > staging > qa etc.)

Reference
Question
Answer Guidelines & Examples
I6
How do you ensure the integrity of your application software?
•	Are secure web site design and coding principles applied?
•	Is there a QA function/process?
•	Is there a documented testing methodology
•	Is there a change and version control mechanism?
•	Is there an escrow agreement in place for bespoke-developed software?
•	What approval and authorisation process for package software?
•	Do you follow a documented system development methodology
•	Where is application testing – in a test environment or production
•	How do you handle test accounts in production?
•	Are test accounts allowed in production
•	How are test accounts monitored
All code goes through peer-review, then automated AND manual QA. Testing is in a separate environment (complete network isolation) from production. There are no test accounts in production.



1.9.	Network Security

Reference
Question
Answer Guidelines & Examples
J1
A secure and robust infrastructure must exist to protect Fairfax services for external connectivity.

Describe your security model for defense of your host environment.
•	Describe firewall infrastructure, use of DMZs, VLANS etc.
•	Do Routers enforce ACL’s
•	Will dedicated firewalls be implemented for Fairfax’s use?
•	Do the firewalls perform stateful inspection?
•	Are reputable firewall software/systems utilised?  
•	Is the firewall appliance based (hardware) or application based (software) – if software, how has the firewall platform been hardened?
•	Have all unnecessary services/ports, source and destination addresses been removed
•	Has all access been prohibited by default and only authorised access enabled?
•	Is the customer database non-internet addressable, located on the internal network, behind multiple firewalls, physically and logically segregated from ‘3rd party’ facing DMZ
•	What process is there in place to include Fairfax in the authorisation process for any changes to firewall configuration?
•	Do firewalls have real-time logging and alerting capabilities?
•	Is 24x7 monitoring implemented with immediate response when unauthorised activity is detected?
We utilize 4 subnets, isolated by ACLs at a network layer. At the host layer, AWS security groups control the allowable ports. The public footprint of the platform is isolated to the ELBs on port 443 (80 is redirected back to 443). Only the bastion node has SSH access publicly, and is hardened and rotated regularly. 90% of our infrastructure has no public IP.

Reference
Question
Answer Guidelines & Examples
J2
Please explain your Intrusion Detection strategy?

•	Describe network and/or host based IDS/IPS implemented.
•	Have intrusion detection systems (IDS) / Intrusion Prevention Systems (IPS) been implemented where Internet services are provided?
•	Does anyone monitor the network for possible intrusion?
•	Who is responsible for monitoring?
The tooling team and devops guild are responsible for network monitoring.

Reference
Question
Answer Guidelines & Examples
J3
What security testing has been performed on the infrastructure?
•	Are systems scanned by internal security prior to production release?
•	Has network penetration testing been conducted by independent external specialists?
•	What frequency are these tests conducted?
•	What tools are used for the security testing?
•	What results were obtained from the most recent tests?
We have regular automated penetration testing, with a more thorough manual test scheduled later in the year.

Reference
Question
Answer Guidelines & Examples
J4
Access for remote support and troubleshoot access should be strictly controlled.

•	Strong two-factor authentication (e.g. tokens)
•	Access via a secure gateway
•	Encrypted sessions
•	Restricted network access
•	Support ids only enabled for duration of troubleshooting activity.
•	All troubleshooting activity logged and subject to independent review
AWS console access is via 2 factor, with logged API audit trail. All activity on every server is centrally logged.

Reference
Question
Answer Guidelines & Examples
J5
Third party support access to your systems should be controlled?
•	Third party support access should be governed by a contract detailing security requirements.
•	Access should be given with minimum privileges and revoked on completion.
•	Authorisation of remote access sessions
•	Granting and revocation of temporary access
•	Remote access authentication using secure mechanisms
•	Logical access controls based on 'least privilege' principle
•	Encryption/integrity checking
•	Logging and review of actions performed during remote access sessions
3rd parties are treated as part of the team – subject to fortnightly review. Access is not revoked – the machine is destroyed, and a replacement built to which they do not have access.



1.10.	Incident Response

Reference
Question
Answer Guidelines & Examples
K1
What method do you use to detect security events
•	What mechanisms do you have in place to identify and deal with a security breach outside of core business hours in a timely manner?
•	Intrusion Detection System (Host &/or Network) with auto call out (e.g. pager system or SMS) of standby security personnel.
•	24/7 Network Operations Centre with appropriately skilled monitoring staff.
•	Documented security incident handling procedure.
Anomaly detection by our low-level logging and system monitoring services (FluentD / Sysdig) -> Pagerduty.

Reference
Question
Answer Guidelines & Examples
K2
What process do you use to handle security incidents? Is this part of a wider incident management process?

Same as all incidents – Pagerduty wakes the on-call devops-guild member.

Reference
Question
Answer Guidelines & Examples
K3
Does the incident management process address the following?
•	Please tick the appropriate items


[x] Activities of intruders
[x] Mechanisms in place to detect incidents
[x] Denial of Service (via AWS)
[x] Process of incident response
[x] Contact persons
[x] Timely notification to management
[x] Timely notification to business partners
[x] Legal issues

[x] Procedure for containment/isolation
[x] Assessment process to determine cause, nature and extent
[x] Removal of cause, backups and preparation for future
[x] Post-incident review



Reference
Question
Answer Guidelines & Examples
K4
How will you notify Fairfax if a significant attack is identified against your organisation?
Incidents affecting security should be reported through appropriate management channels as quickly as possible.
•	Who will be responsible for incident escalation and notifying Fairfax of the incident?
•	Who at Fairfax will you escalate to, who would you expect to escalate to?
•	Documented Incident Response and Threat Management process

Yes, via our account-manager relationship. 
