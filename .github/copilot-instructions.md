## Copilot instructions for NetApp Keystone STaaS documentation

### Repository overview
Product: NetApp Keystone STaaS (Storage-as-a-Service)

NetApp Keystone is a pay-as-you-go, subscription-based storage service that provides file, block, and object storage at predefined performance service levels under an OpEx model, supporting both on-premises deployments and hybrid cloud scenarios. The repository documents the Keystone STaaS offering version 3, including collector installation, subscription monitoring, billing concepts, and add-on services.

### Repository structure
- `concepts/` – Core product concepts: storage types, performance service levels, billing model, add-on services, SLOs, metrics, and infrastructure
- `installation/` – Keystone Collector and ITOM Collector installation, configuration, monitoring, upgrade, and security
- `integrations/` – Keystone dashboard documentation for NetApp Console and Active IQ Digital Advisor, covering subscription insights, usage, assets, alerts, and performance views
- `dark-sites/` – Private mode deployment for organizations with limited or no internet connectivity (also called dark sites or restricted environments)
- `rest-api/` – Digital Advisor REST API documentation for retrieving Keystone subscription and consumption data programmatically
- `release-notes/` – Release notes, fixed issues, known issues, and known limitations
- `_whatsnew/` – Individual what's new entries (one file per release date) used to generate the what's new page
- `_include/` – Reusable AsciiDoc content snippets shared across multiple pages
- `media/` – Images and diagrams referenced by content pages
- `store-redirects/` – URL redirect configuration for moved or renamed pages

### Product-specific context

**Architecture and components:**
- *Storage platforms*: ONTAP on AFF A-Series, AFF C-Series, ASA A-Series, ASA C-Series, FAS, and AFX systems; StorageGRID for object storage; Cloud Volumes ONTAP for cloud-based storage
- *Keystone Collector*: On-premises software agent (deployed as VMware OVA or on Linux) that collects usage and performance data from ONTAP and StorageGRID controllers, then sends billing data to the Keystone cloud platform via HTTPS; leverages Active IQ Unified Manager (AIQUM) for controller connectivity
- *ITOM Collector*: Local agent installed on Linux or Windows at the customer site; communicates with the cloud-based ITOM monitoring solution for infrastructure health monitoring
- *ITOM monitoring solution*: Cloud-based SaaS application for monitoring Keystone infrastructure; receives data from the ITOM Collector and provides alerting and reporting
- *Keystone dashboard*: Available in NetApp Console (*Storage > Keystone > Overview*) and Active IQ Digital Advisor (*General > Keystone Subscriptions*); used to monitor subscriptions, track capacity usage, view assets, and manage alerts
- *AutoSupport*: Mechanism used by Keystone Collector to send metadata to the Active IQ data lake; also available as an alternative to Keystone Collector when the standard collector cannot be supported
- *Zuora*: External billing platform that receives processed consumption data to generate invoices

**Key concepts:**
- *Performance service level (PSL)*: A predefined storage tier with target IOPS, throughput, and latency characteristics; tiers include Extreme, Premium, Standard, and Value for unified storage; Extreme, Premium, and Standard for block-optimized; Extreme only for AFX; Object for StorageGRID
- *Performance service level instance (PSLI)*: An individual storage system assigned to a PSL; for unified and block-optimized storage, a PSLI is an HA pair; for AFX storage, a PSLI is a single AFX controller
- *Data storage type (DST)*: The storage category for a subscription — unified (file, block, S3 on ONTAP AFF/FAS), block-optimized (SAN on ONTAP ASA), AFX (file and object on AFX systems), object (StorageGRID), or cloud (Cloud Volumes ONTAP)
- *Committed capacity*: The minimum capacity contracted per PSL and billed in full each billing period regardless of actual usage
- *Consumed capacity*: Actual storage capacity in use at any point, measured every five minutes
- *Burst consumption*: Usage exceeding committed capacity, billed at a burst rate; default burst limit is 20% above committed capacity (configurable to 40% or 60%)
- *Advanced data protection (ADP)*: Optional add-on using NetApp MetroCluster for synchronous site-to-site mirroring with RPO=0; available for unified storage on AFF arrays at Extreme, Premium, and Standard PSLs only
- *Private mode*: Keystone Collector deployment for restricted environments (dark sites) without internet connectivity; usage files are generated locally and manually forwarded to NetApp
- *Standard mode*: Keystone Collector deployment with outbound internet connectivity to the Keystone cloud

**Naming conventions and terminology:**
- *STaaS* – Storage-as-a-Service (the full offering name is NetApp Keystone STaaS)
- *PSL* – Performance Service Level
- *PSLI* – Performance Service Level Instance
- *DST* – Data Storage Type
- *ADP* – Advanced Data Protection (add-on service using MetroCluster)
- *ITOM* – IT Operations Management (the monitoring solution and its collector)
- *AIQUM* – Active IQ Unified Manager (used by Keystone Collector for controller access)
- *NRNVC* – Non-returnable, non-volatile components (an add-on service for compliance; uses SnapLock technology)
- *USPS* – U.S. Protected Support (a US-citizen support add-on; unrelated to the postal service)
- *DII* – Data Infrastructure Insights (an add-on service for monitoring, troubleshooting, and optimizing storage)
- *KSM* – Keystone Success Manager (the dedicated NetApp account manager for each Keystone customer)
- *FabricPool* – NetApp automated tiering technology used by the data tiering add-on service
- *SnapLock* – NetApp compliance technology used with the NRNVC add-on service
- *Dark site* – An isolated environment with restricted internet access; Keystone's private mode targets these deployments
- *Operational models*: Partner-operated (service provider or reseller) and customer-operated — these determine who manages the infrastructure and performs administrative tasks
- *AFX* – A disaggregated storage platform with independent controller (compute) and drive shelf (storage) scaling; AFX subscriptions are standalone and cannot be combined with other storage types in the same subscription

### Typical user workflows

**Subscribe and deploy Keystone:** Agree on PSLs and committed capacity with NetApp → NetApp deploys storage infrastructure on-premises → Customer installs Keystone Collector and ITOM Collector → Collector connects to storage controllers and billing platform → Subscription becomes active

**Install Keystone Collector (standard mode):** Meet virtual infrastructure or Linux prerequisites → Deploy OVA on VMware vSphere or install `.rpm`/`.deb` package on Linux → Configure Collector (storage controller credentials, AIQUM connection) → Validate installation → Configure AutoSupport if required

**Install Keystone Collector (private mode):** Meet prerequisites → Install Collector in isolated environment → Configure for offline operation → Collector generates local usage files → Manually forward usage files to NetApp for billing processing

**Install ITOM Collector:** Meet Linux or Windows prerequisites → Install ITOM Collector on server → Collector registers with cloud-based ITOM monitoring solution → Infrastructure health monitoring becomes active

**Monitor subscriptions via dashboard:** Access Keystone dashboard in NetApp Console or Digital Advisor → View subscription details and PSLs → Check current consumption against committed capacity → Review consumption trends and burst usage → View assets and performance metrics → Manage alerts and monitors

**Access Keystone data via REST API:** Generate refresh and access tokens in Digital Advisor → Call Digital Advisor REST API endpoints → Retrieve customer list, subscriptions, or consumption details programmatically
