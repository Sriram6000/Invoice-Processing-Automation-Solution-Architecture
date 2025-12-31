
# Comparison Table (Trade-offs)

## Purpose

This table provides a **trade-off analysis** of the three architectural patterns for invoice automation. Each pattern solves the same business problem but optimizes for different priorities such as **volume handling, storage choice, cost, scalability, and operational complexity**.  

Use this table to understand **which pattern best fits your scenario** based on business and technical constraints.

| **Criteria**        | **Pattern 1: <br/>High Volume – Azure Blob & Functions** | **Pattern 2: <br/>Mid Volume – Dataverse-Centric** | **Pattern 3: <br/>Low Volume – SharePoint-Based** |
|----------------------|------------------------------------------------------|-----------------------------------------------|----------------------------------------------|
| **Volume Range**     | Millions/year                                       | 100k–1M/year                                 | <100k/year                                   |
| **Storage**          | Azure Blob Storage                                  | Dataverse                                    | SharePoint Lists & Document Libraries       |
| **Integration**      | Azure Functions + Power Platform                    | Power Platform native                        | Power Platform native                        |
| **Cost**             | High (cloud infra + scaling)                        | Moderate                                     | Low                                          |
| **Scalability**      | Very High                                           | Medium                                       | Low                                          |
| **Complexity**       | High (multi-component orchestration)                | Medium                                       | Low                                          |
| **Recommended Use** | Global operations, high SLA compliance              | Regional operations, structured data needs   | Small teams, minimal infra footprint         |

## Why This Is a Trade-Off Table
- **Pattern 1** offers maximum scalability but at higher cost and complexity.
- **Pattern 3** minimizes cost but sacrifices scalability and advanced features.
- **Pattern 2** balances cost and performance for mid-scale operations.

## Notes
- All patterns use **Power Platform as the core** (Power Automate flows, Power Apps, PAD).
- Differentiators are primarily **storage strategy**, **scalability**, and **cost optimization**.

> [!NOTE]
> **Pattern 1 is recommended for Accounts Payable (AP) team at GobalCrop Ltd with high invoice volumes and strict SLA/compliance requirements.**

---

|Previous : [Low Volume - SharePoint-Based Architecture](p3-low-volume-sharepoint-based-architecture.md) | Next : [ROI Analysis](/03-roi-analysis/README.md)|
|-|-|

