## Pattern 1 : High Volume - Azure Blob & Functions Architecture

This pattern is designed for **large-scale operations** where invoice volumes are extremely high (millions per year) across multiple countries. It focuses on **scalability, resilience, and performance**, ensuring the solution can handle global workloads efficiently while maintaining compliance and auditability.

The architecture leverages **Microsoft Power Platform** and **AI Builder** for intelligent document processing, integrated with ERP systems for seamless posting and robust audit trails.


## Business Context
- Directly addresses the **business problem** of scaling Accounts Payable for a global FMCG enterprise.
- Ensures **cost efficiency at scale** while meeting SLA and compliance requirements.
- Provides **future-ready architecture** for growth and automation maturity.

## Technical Overview
The solution pilots invoice processing automation in **one region**, with plans to **scale globally**.

**Key Components:**
- **Platform:** Microsoft Power Platform, Azure Functions, Azure Blob Storage
- **Data Ingestion:** Invoices received via email, saved to Azure Blob Storage; Azure Functions split multi-page invoices
- **Extraction:** AI Builder extracts invoice data; low-confidence cases routed for human review through Power Apps
- **Validation & Posting:** Data validated and posted to ERP via Power Automate Desktop (legacy UI automation)
- **Storage & Security:** Canonical data in Dataverse; files in Blob; credentials in Azure Key Vault
- **Observability:** Power Automate Queues and Logs, custom Power BI dashboards

## Architecture Diagram

*End-to-end invoice automation leveraging Microsoft Power Platform and Azure services.*
<img src='https://github.com/Sriram6000/SA1-Invoice-Processing-Automation-Solution-Architecture/blob/main/05-assets/daigrams/Invoice%20Processing%20Solution%20Archeitecture-P1.png'/>


## Detailed Solution Design

*   **Ingestion:** Power Automate (Outlook trigger) saves attachments to Blob and creates Dataverse: Invoice\_Staging with metadata.
*   **Split & Filter:** Azure Function (Blob trigger) splits multi-invoice PDFs, filters non-invoice pages, writes split pages to Blob, and updates Dataverse.
*   **Extraction:** Power Automate cloud flow picks ReadyForExtraction invoices, calls AI Builder, writes extracted fields and confidence scores back to Dataverse.
*   **Validation & Routing:** Business rules validate Vendor, PO match, tax math, duplicates. Low-confidence or failed rules are routed for human review.
*   **Human-in-the-loop:** Reviewer uses Power Apps to correct and approve data.
*   **ERP Posting:** PAD queue flow posts to ERP UI, updates status in Dataverse.
*   **Monitoring & Reconciliation:** Dashboards surface throughput, exceptions, confidence histograms, and latency; alerts routed to ops channel.

## Well-Architected Framework Alignment

| Pillar                  | Implementation Example                                                                  |
| ----------------------- | --------------------------------------------------------------------------------------- |
| Reliability             | Event-driven Azure Functions, Power Platform Queues, PAD redundancy, Blob durability    |
| Security                | Azure Key Vault, DLP policies, RBAC, audit logs, encryption                             |
| Operational Excellence  | Admin analytics, Power BI dashboards, flow logs, runbooks, ALM with Solutions           |
| Performance Efficiency  | Auto-scaling Azure Functions, page pre-filtering, concurrency controls, cost monitoring |
| Experience Optimization | Power Apps reviewer app, SLAs, feedback loop, status tracking, notifications            |

## Cost & Licensing Analysis

### **Scope**: 
- Pilot implementation for one region (Country A)
- Average annual invoice volume: 240,000 invoices / year
- Average monthly invoice volume: 20,000 invoices / month
- Processing assumption: 1 page per invoice (prefiltered before AI)
- Execution model (best fit): 3 customer-managed VMs with PAD

**Assumptions**
- Invoice PDFs are prefiltered so that only invoice-relevant pages are sent to AI
- AI costs are page-based, not invoice-based
- PDFs are stored in Azure Blob Storage; Dataverse stores metadata only
- 3 PAD bots are used to achieve parallelism and resilience
- Power Automate Process licenses are required per bot
- Power Apps Premium cost shown is for 1 user only and scales linearly with number of users

| Component                                  |           Monthly Cost (USD) |             Annual Cost (USD) | Rationale                                           |
| ------------------------------------------ | ---------------------------: | ----------------------------: | --------------------------------------------------- |
| Azure Blob Storage                         |                       ~$0.30 |                        ~$3.60 | PDF storage (Hot tier), negligible footprint        |
| Azure Key Vault                            |                       ~$0.06 |                        ~$0.72 | Secure storage of credentials and secrets           |
| **Copilot / AI Credits**                   |                  **~$1,600** |                  **~$19,200** | 20,000 pages/month × ~$0.08 per page                |
| Dataverse                                  |                     Included |                      Included | Used only for invoice metadata and logs             |
| Azure Functions                            |                          ~$0 |                           ~$0 | Event-driven orchestration within free grant limits |
| **Power Automate Process Licenses (3×)**   |                     **$450** |                    **$5,400** | Required per unattended PAD bot                     |
| **Azure Virtual Machines (3×)**            |              **$120 – $240** |           **$1,440 – $2,880** | Customer-managed VMs for PAD execution              |
| Power Automate Premium (1 automation user) |                          $15 |                          $180 | Service account for cloud flow orchestration        |
| Power Apps Premium (1 app user)            |                          $15 |                          $180 | Per-user license; **multiplies by app users**       |
| **Estimated Total (Pilot)**                | **~$2,200 – $2,320 / month** | **~$26,400 – $27,850 / year** |                       |

**Execution Model Consideration**
| Option                                  | Included                               | Monthly Cost (3 bots) | Notes                                              |
| --------------------------------------- | -------------------------------------- | --------------------: | -------------------------------------------------- |
| **Hosted Machine (RPA Hosted license)** | Process license + Microsoft-managed VM |                 ~$645 | Lower ops overhead |
| **Customer-managed VMs (Chosen)**       | Process license + Azure VM (separate)  |          ~$570 – $690 | Better throughput control and cost optimisation    |

**Disclaimer**
All costs presented above are indicative estimates only, based on publicly available pricing and architectural assumptions at the time of design.
Actual costs may vary depending on:
  - Final invoice volume and page count
  - AI model pricing changes
  - Azure region and VM sizing
  - Licensing agreements and enterprise discounts

This analysis is intended solely for solution design and comparative evaluation during the pilot phase, and should not be treated as a final commercial quote.

## Pros & Cons

| Area                 | Pros                                                        | Cons                                  |
| -------------------- | ----------------------------------------------------------- | ------------------------------------- |
| Power Platform stack | Faster development & deployment, low-code, secure, scalable | Licensing changes, capacity limits    |
| AI Builder           | Fast to pilot, native integration                           | May need custom models, credit cost   |
| PAD for ERP          | Enables legacy automation                                   | Fragile to UI changes & app stability |
| Blob Storage         | Cheap, scalable, offloads Dataverse storage                 | Extra step for file management        |

## Risks & Mitigations

| Risk Area                 | Risk Description                                             | Mitigation Strategy                                                                            |
| ------------------------- | ------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| ERP UI changes            | ERP UI changes break PAD scripts, causing posting failures   | Use robust selectors, modular scripts, regular regression testing, machine groups for failover |
| AI Builder confidence     | Low extraction accuracy on new/complex invoice formats       | Human-in-the-loop review, periodic model retraining, custom AI models                          |
| Flow throttling/limits    | Power Automate flow runs hit service limits/throttling       | Queue-based decoupling, concurrency controls, batch processing, monitor usage proactively      |
| Licensing/capacity        | Exceeding Copilot credits, Dataverse, or PAD license limits  | Monitor usage, set up alerts, review license needs quarterly                                   |
| Data privacy & compliance | Sensitive data exposure or non-compliance                    | Enforce DLP policies, use Azure Key Vault, RBAC, audit logs, compliance reviews                |
| Blob storage durability   | Data loss or corruption in Blob storage                      | Use LRS/GRS as required, enable soft delete, regular backup/restore testing                    |
| Function/Flow failures    | Azure Functions or Power Automate flows fail                 | Retry policies, alerting, runbooks, monitor with App Insights/Admin Analytics                  |
| Manual review backlog     | Human-in-the-loop queue grows, causing SLA breaches          | Reviewer SLAs, auto-escalate aged items, monitor backlog in Power BI, add reviewers as needed  |
| Security threats          | Unauthorized access, credential leaks, privilege escalation  | Azure Key Vault, least privilege, security reviews, MFA                                        |
| Cost overruns             | Unexpected Azure or Power Platform consumption costs         | Cost alerts, usage dashboards, optimize AI/Function usage, right-size licenses                 |
| Change management         | Uncontrolled changes impact production stability             | ALM with Solutions, Dev/Test/Prod separation, document changes, peer review deployments        |
| Vendor lock-in            | Heavy reliance on Power Platform or Azure-specific features  | Document integration points, design for modularity, monitor alternatives                       |
| Knowledge/skills gap      | Team lacks expertise in Power Platform, Azure, or AI Builder | Training, documentation, Microsoft Learn resources, assign mentors                             |

## Dataverse Data Model (Core Tables)

*   **Invoice\_Staging:** StagingId (GUID), BlobUri, Checksum, Sender, ReceivedAt, Status, ErrorDetail
*   **Invoice\_Split:** SplitId (GUID), OriginalId (Lookup), BlobUriSplit, PageNumber, SplitStatus
*   **Invoice:** StagingId (Lookup), SplitId (Lookup), InvoiceId, VendorId, VendorName, InvoiceNumber, InvoiceDate, PONumber, Currency, Subtotal, Tax, Total, DueDate, Confidence, State, BlobUriSplit, AIModelVersion
*   **InvoiceLine:** LineId, InvoiceId (lookup), Description, Quantity, UnitPrice, TaxRate, Amount, ExtractConfidence
*   **Processing Log:** LogId, Entity, EntityId, EventType, Timestamp, Message, Severity, CorrelationId
*   **Posting\_Result:** InvoiceId, PostingNumber, PostedAt, ErpCompany, ErrorsJson

---

|Back to : [Solution Approach](/02-solution-approach) | Next : [Mid Volume - Dataverse-Centric Architecture](02-p2-mid-volume-dataverse-centric-architecture.md)|
|-|-|

