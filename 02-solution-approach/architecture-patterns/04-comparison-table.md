# Comparison Table (Trade-offs)

## Purpose
This table provides a **trade-off analysis** of the three architectural patterns for invoice automation. Each pattern solves the same business problem but optimizes for different priorities such as **volume handling, storage choice, cost, scalability, and operational complexity**.

Use this table to understand **which pattern best fits your scenario** based on business and technical constraints.

| **Criteria**        | **Pattern 1:<br/>High Volume – Azure Blob & Functions** | **Pattern 2:<br/>Mid Volume – Balanced (SharePoint + Dataverse)** | **Pattern 3:<br/>Low Volume – SharePoint-Based** |
|----------------------|----------------------------------------------------------|--------------------------------------------------------------------|---------------------------------------------------|
| **Volume Range**     | >500K/year (up to millions)                             | <500K/year                                                     | <2K/year                                          |
| **Storage**          | Azure Blob Storage                                      | SharePoint (binaries) + Dataverse (metadata)                     | SharePoint Lists & Document Libraries             |
| **Integration**      | Azure Functions + Power Platform                        | Power Platform native (Flows + PAD)                               | Power Platform native                              |
| **Cost**             | High (cloud infra + scaling)                            | Moderate (SP storage + DV metadata + RPA)                        | Low                                               |
| **Scalability**      | Very High                                               | Medium                                                            | Low                                               |
| **Complexity**       | High (multi-component orchestration)                    | Medium (governance + archival strategy)                          | Low                                               |
| **Recommended Use**  | Global operations, high SLA compliance                  | Regional/mid-scale ops needing governance & audit                | Small teams, minimal infra footprint              |


## **Rationale for Volume Caps**
- **Pattern 3 (Low Volume – SharePoint-Based)**  
  - SharePoint lists throttle at **5,000 items per view** and Power Apps delegation limits make complex queries inefficient.  
  - Document libraries with tens of thousands of files require heavy indexing and archiving.  
  - **Industry practice**: Suitable for **<2K invoices/year** (or <200/month) for pilots or small teams.

- **Pattern 2 (Balanced – SharePoint + Dataverse)**  
  - Dataverse handles relational metadata and scales well for hundreds of thousands of records.  
  - SharePoint manages binaries economically with foldering and archival tiers.  
  - **Industry sweet spot**: **10K–500K invoices/year** for regional or multi-country operations needing compliance and audit.

- **Pattern 1 (High Volume – Blob & Functions)**  
  - Blob storage and Azure Functions scale infinitely and handle millions of files with low latency.  
  - Required for global AP operations with strict SLA and concurrency needs.  
  - **Industry norm**: **>500K invoices/year**, often millions/year.


## Why This Is a Trade-Off Table
- **Pattern 3** minimizes cost but sacrifices scalability and advanced features.
- **Pattern 2** (Balanced) provides **structured governance, better performance than SP-only, and cost control without Blob complexity**.
- **Pattern 1** offers maximum scalability but at higher cost and complexity.

## Notes
- All patterns use **Power Platform as the core** (Power Automate flows, Power Apps, PAD).
- Differentiators are primarily **storage strategy**, **scalability**, and **cost optimization**.

> [!NOTE]
> **Pattern 1 is recommended for Accounts Payable (AP) team at GobalCrop Ltd with high invoice volumes and strict SLA/compliance requirements.**

> [!NOTE]
> **Pattern 2 is ideal for organizations that need compliance, audit trails, and better performance than SharePoint-only, without the engineering overhead of Blob-based architecture.**

---

| Previous: [Low Volume - SharePoint-Based Architecture](03-p3-low-volume-sharepoint-based-architecture.md) | Next: [ROI Analysis](/03-roi-analysis/README.md) |
|-|-|
