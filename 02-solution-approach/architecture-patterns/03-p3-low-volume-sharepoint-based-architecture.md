# Pattern 3 : Low Volume - SharePoint-Based Architecture

## **Technical Overview**
This architecture leverages **Microsoft 365 components (SharePoint, Power Automate, Power Apps, AI Builder)** and a **Virtual Machine for RPA** to process vendor invoices in a **low-volume scenario**.  
This pattern is intended for organizations seeking a quick, cost-effective solution that avoids investment in highly scalable cloud-native services by leveraging existing Microsoft 365 licenses and minimal infrastructure overhead.

**Key Characteristics:**
- Suitable for **low-volume invoice processing**.
- **Limited scalability** and **basic security controls**.
- Heavy reliance on **SharePoint lists and document libraries**.

## **Architecture Diagram**

<img src='https://github.com/Sriram6000/SA1-Invoice-Processing-Automation-Solution-Architecture/blob/main/05-assets/daigrams/Sharepoint Based Architecture-P3.png'/>

## **Detailed Solution Design (Differences from High-Volume Pattern)**
- **Document Storage**: Uses **SharePoint** instead of Azure Blob.
- **Processing Logic**: Relies on **Power Automate flows** and **Virtual Machine-based RPA (PAD)** instead of Azure Functions.


## **Cost & Licensing Analysis**

### **Scope**
- Low-volume invoice processing (200 invoices per month).
- Microsoft 365 ecosystem with minimal Azure components.

### **Assumptions**
- Existing Microsoft 365 licenses for SharePoint, PowerApps and Power Automate license.
- Single Virtual Machine for RPA.

### **Important Note**
- _AI Builder requires Power Automate Premium license even if AI credits are purchased. Copilot credits assist in development but do not replace licensing for execution._
  
### **Cost/Licensing Table**
| Component                     | Licensing Model                              | Monthly Cost | Annual Cost |
|------------------------------|---------------------------------------------|-------------:|------------:|
| SharePoint Online           | Included in M365                            | N/A          | N/A |
| Power Apps                  | **N/A** (SharePoint only, no premium connectors) | N/A          | N/A |
| Power Automate (Cloud Flows)| Per-user or per-flow                        | $15          | $180 |
| Copilot credit              | PAYG Meter                                  | $40          | $480 |
| Hosted RPA Machine          | Process license + Hosted Machine            | $215         | $2,580 |
| Azure Key Vault             | Pay-per-operation                           | Minimal      | Minimal |
| **Estimated Total (Pilot)** |                                             | **~$270**     | **~$3,240** |

### **Execution Model Consideration**
| Option                                  | Included                               | Monthly Cost (1 bot) | Annual Cost | Notes                                              |
| --------------------------------------- | -------------------------------------- | --------------------: | ----------: | -------------------------------------------------- |
| **Hosted Machine (RPA Hosted license) (Chosen)** | Process license + Microsoft-managed VM | ~$215                | ~$2,580     | Lower ops overhead |
| **Customer-managed VMs**               | Process license + Azure VM (separate)  | ~$190 – $230         | ~$2,280 – $2,760 | Better throughput control and cost optimisation    |

### **Observation:**  
- Hosted Machine is simpler and cost-predictable for single bot scenarios.  
- Azure VM option offers flexibility but adds extra cost and management overhead.

### **Disclaimer**
All costs presented above are indicative estimates only, based on publicly available pricing and architectural assumptions at the time of design.
Actual costs may vary depending on:
  - Final invoice volume and page count
  - AI model pricing changes
  - VM sizing
  - Licensing agreements and enterprise discounts

This analysis is intended solely for solution design and comparative evaluation during the pilot phase, and should not be treated as a final commercial quote.

## **Pros & Cons**
**Pros**
- Quick to implement using existing Microsoft ecosystem.
- Lower cost compared to high-volume architecture.
- Familiar tools for business users (Power Apps, SharePoint).
- Minimal incremental cost if Microsoft 365 licenses already exist.

**Cons**
- Not scalable for large invoice volumes.
- Limited security compared to enterprise-grade solutions.
- Manual intervention introduces delays and errors.
- SharePoint list performance degrades as data grows.

## **Risks & Mitigations**
| **Risk** | **Mitigation** |
|----------|-----------------|
| **SharePoint Exposure / Security** – Sensitive data may be exposed due to misconfigured permissions or external sharing. | Enforce strict permissions, disable external sharing, enable MFA, use Azure Key Vault, apply DLP policies. |
| **SharePoint List Growth → Performance Issues** – Lists exceeding 5,000 items cause throttling and slow queries. | Regular archiving to Azure Storage or secondary site, indexed columns, filtered views, monitor thresholds. |
| **Power Apps Delegation Limitations** – Non-delegable queries lead to incomplete data retrieval. | Use delegable queries, pagination, offload heavy logic to Power Automate or Azure Functions. |
| **Manual Intervention Risk** – Finance team marking invoices manually introduces human error and delays. | Add validation checks in Power Apps, use AI Builder confidence scoring, implement approval workflows. |
| **Audit Trail Gaps** – Limited logging compared to enterprise-grade solutions. | Enable Power Automate flow logs, export logs to Azure Monitor for retention and compliance. |
| **VM Dependency** – If VM running RPA is down, processing halts. | Use Azure Automation or VM availability sets for resilience. |

## **Why This Matters**
- Provides a **cost-effective alternative** for organizations with low invoice volumes.
- Highlights **audit trail limitations** and **manual intervention risks** for compliance awareness.
- Serves as a **baseline pattern** for quick deployment without heavy infrastructure investment.

---

|Previous : [Mid Volume - Dataverse-Centric Architecture](02-p2-mid-volume-dataverse-centric-architecture.md) | Next : [Comparison Table (Trade-offs)](04-comparison-table.md)|
|-|-|
