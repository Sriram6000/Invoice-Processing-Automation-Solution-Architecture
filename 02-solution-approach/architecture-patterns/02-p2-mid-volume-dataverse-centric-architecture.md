# Pattern 2: **Mid Volume – Balanced Dataverse‑Centric Architecture**  
*(SharePoint for Binary Storage • Dataverse for Metadata & Governance)*

## **Technical Overview**
This pattern is designed for **mid‑volume invoice processing** where you want **Power Platform–first delivery**, **strong governance**, and **predictable costs**—without jumping to a high‑volume, Azure‑heavy stack.

- **Binary storage**: **SharePoint Online** holds all PDFs, page splits, and CSV staging. This leverages pooled tenant storage (**1 TB + 10 GB per licensed user**) and low‑cost top‑ups (≈ **$0.20/GB/month**) with optional **Microsoft 365 Archive** for cold data (~**$0.05/GB/month**)—keeping file costs under control.
- **Metadata & app state**: **Microsoft Dataverse** stores master records, page metadata, extracted fields, statuses, approvals, and audit history. You avoid punitive Dataverse storage for binaries (DB add‑ons ~**$40/GB/month**).
- **Automation**: **Power Automate** cloud flows orchestrate ingestion, extraction, validation, staging, and ERP upload hand‑off. **Unattended PAD** runs the ERP UI because no API exists; **Hosted Process** adds a Microsoft‑managed VM for lower ops overhead.
- **AI extraction**: **AI Builder/Copilot Credits** for invoice OCR with confidence‑based human‑in‑loop validation (note the transition from AI Builder credits to Copilot Credits through **2025–2026**).
- **Audit & telemetry**: **Dataverse Auditing** enabled; **export audit logs to Azure via Synapse Link** for long‑term retention and Power BI; **Managed Environments/App Insights** consolidate telemetry for Power Apps/Flows.

> [!note]
> Dataverse is excellent for **metadata & governance**. It is **not** economical for **binaries** at mid volume. Keep all documents in SharePoint; keep Dataverse lean.

## **Architecture Diagram**
*(Use your repo diagram)*  
<img src='https://github.com/Sriram6000/SA1-Invoice-Processing-Automation-Solution-Architecture/blob/main/05-assets/daigrams/Dataverse-Centric Architecture.png'/>

## **Detailed Solution Design**
**Data Flow**
1. **Email ingestion** → Power Automate saves PDF to SharePoint and creates a Dataverse MasterInvoice record with SP URL.
2. **Page splitting** → PAD bot or PDF connector splits pages into SharePoint folders; Dataverse tracks page metadata.
3. **Validation** → Finance uses Power Apps (Dataverse backend) to triage and approve.
4. **Extraction** → AI Builder/Copilot extracts fields; writes structured data to Dataverse InvoiceData table.
5. **ERP upload** → PAD bot uploads via ERP UI; updates Dataverse status.
6. **Retention** → Move processed files to SharePoint Archive; export Dataverse audit logs to Azure for compliance.

**Differences vs Other Patterns**
- **vs Low Volume (SP-only)**: Adds Dataverse for relational metadata, better delegation, and governance.
- **vs High Volume (Blob)**: Avoids Blob and Azure Functions complexity; stays Power Platform-centric.

## **Cost & Licensing Analysis**

### **Scope**
- Mid-volume invoice processing (~**5k invoices/month**).
- Power Platform-centric solution with SharePoint for storage and Dataverse for metadata.
- Includes AI Builder/Copilot credits and RPA for ERP upload.

### **Assumptions**
- Microsoft 365 tenant exists (E3/E5 or equivalent).
- Power Automate Premium for makers/runners; Hosted Process for bots.
- Dataverse used only for metadata (minimal DB footprint).
- PDFs remain entirely in SharePoint (active and archive tiers).

| Component | Licensing Model | Monthly Cost | Annual Cost | Notes |
|-----------|-----------------|-------------:|------------:|-------|
| SharePoint Online | Included in M365 | N/A | N/A | Extra storage ≈ $0.20/GB/mo; Archive ≈ $0.05/GB/mo |
| Dataverse | Default 3GB Database capacity | NA |  N/A | Within Environment default capacity; can be scalled |
| Power Automate Premium | Per user | $15 | $180 | For makers/approvers |
| Power Automate Hosted Process | Per bot | $215 | $2,580 | Includes VM |
| Copilot Credits | PAYG | $400 | $4800 |  |
| Azure Key Vault | Pay-per-operation | Minimal | Minimal | Secure secrets |

**Execution Model**
| Option | Monthly Cost | Notes |
|--------|-------------:|-------|
| Hosted Machine (chosen)| $215 | Lower ops overhead |
| Customer-managed VM | $150 + VM | More control |

**Observation:**
- Hosted Machine is simpler and cost-predictable for single bot scenarios.
- Azure VM option offers flexibility but adds extra cost and management overhead.

**Disclaimer**
All costs presented above are indicative estimates only, based on publicly available pricing and architectural assumptions at the time of design. Actual costs may vary depending on:
- Final invoice volume and page count
- AI model pricing changes
- VM sizing
- Licensing agreements and enterprise discounts
  
This analysis is intended solely for solution design and comparative evaluation during the pilot phase, and should not be treated as a final commercial quote.

## **Pros & Cons**
**Pros**
- Scales better than SP-only; simpler than Blob-based.
- Strong governance: Dataverse auditing, RBAC.
- Cost-effective storage: SharePoint active + archive tiers.
- Power Platform-native → faster delivery.

**Cons**
- RPA fragility (ERP UI changes).
- SharePoint throttling risk (5,000-item view threshold).
- AI confidence variance → manual validation needed.

## **Risks & Mitigations**
| Risk | Mitigation |
|------|------------|
| Dataverse cost creep | Enforce “no binaries in DV”; monitor PPAC |
| SharePoint throttling | Indexed columns, filtered views, foldering |
| RPA fragility | Hosted Process, retries, robust selectors |
| Audit gaps | DV auditing + Synapse Link export |
| AI credit shock | Plan Copilot Credits transition |

## **Why This Matters**
- **Performance & governance**: Dataverse solves SharePoint list limits and adds RBAC + auditing.
- **Cost control**: SharePoint storage tiers beat Dataverse file/database costs; avoids Blob complexity.
- **Audit-ready**: DV auditing + Azure export ensures compliance.


### **Model-Driven App Opportunity**
A **Model-Driven App** can be built on Dataverse tables to provide a structured, secure, and auditable interface for finance teams:
- **Invoice Master View**: Invoice ID, Vendor, Date, Total, Status, Confidence Score, SharePoint File URL.
- **Child Page Details**: Page No, Include/Exclude, AI Confidence, SharePoint Page URL.
- **Validation Dashboard**: Queue for low-confidence invoices; editable fields with audit trail.
- **Integration Log**: Stage, Status, Error Code, Correlation ID for troubleshooting.
- **Security & Compliance**:
  - Role-based access (Finance, Approver, Admin).
  - Field-level security for sensitive data.
  - Dataverse auditing enabled for all changes.

> Unlike SharePoint-only solutions, this pattern enables a **governed, relational UI** that scales with volume and compliance needs.


## **Disclaimer**
Costs are indicative list prices; validate with Microsoft pricing and your agreements.

---

|Previous : [High Volume - Azure Blob & Functions Architecture](01-p1-high-volume-azure-blob-functions-architecture.md) | Next : [Low Volume - SharePoint-Based Architecture](03-p3-low-volume-sharepoint-based-architecture.md)|
|-|-|

