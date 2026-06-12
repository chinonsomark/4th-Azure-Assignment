# AWS vs Azure Cost Comparison Report

## 1. Application Specifications

A small web application with the following requirements:
- **Compute:** 1 vCPU, 8 GB RAM
- **Storage:** 50 GB
- **Data Transfer:** 200 TB/month outbound
- **OS:** Linux
- **Region:** US East (N. Virginia) for AWS / East US for Azure

---

## 2. AWS Cost Breakdown

### Compute — Amazon EC2 (t2.small, Linux)
| Pricing Model | Upfront | Monthly |
|---|---|---|
| Compute Savings Plan (3yr, No Upfront) | $0.00 | $8.40 |
| EC2 Instance Savings Plan (3yr, No Upfront) | $0.00 | $7.30 |
| On-Demand | $0.00 | $16.79 |
| Spot Instance | $0.00 | $16.96 |

> Selected: **On-Demand** at **$16.79/month** for maximum flexibility.

### Storage — Amazon S3
| Component | Cost |
|---|---|
| S3 Standard Storage (50 GB) | included |
| Outbound Data Transfer (200 TB/month) | $14,131.20 |
| **Total S3 Monthly** | **$14,132.35** |

#### Data Transfer Breakdown (Tiered Pricing):
- 10,240 GB × $0.09 = $921.60
- 40,960 GB × $0.085 = $3,481.60
- 102,400 GB × $0.07 = $7,168.00
- 51,200 GB × $0.05 = $2,560.00

### AWS Total Estimate
| Service | Monthly Cost |
|---|---|
| Amazon EC2 (Web App Server) | $16.79 |
| Amazon S3 (Storage + Transfer) | $14,132.35 |
| Amazon EC2 (additional) | $30.37 |
| **Total Monthly** | **$14,179.51** |
| **Total 12 Months** | **$170,154.12** |

---

## 3. Azure Cost Breakdown

### Compute — Azure Virtual Machine (DC1s v3)
| Component | Details |
|---|---|
| Region | East US |
| OS | Linux (Ubuntu) |
| Size | DC1s v3 — 1 Core, 8 GB RAM |
| Hours | 730 hours/month |
| Pricing Model | Pay as you go |
| **Monthly Cost** | **$70.08** |

### Storage — Azure Blob Storage (Storage Accounts)
| Component | Details |
|---|---|
| Type | Block Blob, General Purpose V2 |
| Redundancy | LRS (Locally Redundant Storage) |
| Access Tier | Hot |
| Capacity | 50 GB |
| Operations | 10×10,000 Write / Read / List |
| Data Retrieval | 1,000 GB |
| **Monthly Cost** | **$2.08** |

### Azure Total Estimate
| Service | Monthly Cost | Upfront |
|---|---|---|
| Virtual Machine (DC1s v3) | $70.08 | $0.00 |
| Storage Account (Blob) | $2.08 | $0.00 |
| Support | $0.00 | $0.00 |
| **Total Monthly** | **$72.16** | **$0.00** |

---

## 4. Networking Cost Comparison

### AWS Inter-Zone / Egress Costs
AWS uses tiered outbound pricing from US East (N. Virginia) to the internet:
- First 10 TB/month: $0.09/GB
- Next 40 TB/month: $0.085/GB
- Next 100 TB/month: $0.07/GB
- Beyond 150 TB/month: $0.05/GB

For 200 TB/month, total egress cost = **$14,131.20/month**

### Azure Egress Costs
Azure charges outbound data transfer from East US to East Asia (inter-region):
- 5 GB outbound included in VM estimate
- Azure's egress rates are comparable to AWS for internet-bound traffic but
  offer lower inter-region transfer costs within the Azure backbone network.

> **Key Insight:** For high-egress workloads (200 TB/month), AWS egress costs
> dominate the total bill. Azure's Blob Storage egress pricing can be more
> competitive, especially with Azure CDN integration.

---

## 5. Discount Mechanisms

### AWS Savings Plans
| Plan Type | Discount | Term |
|---|---|---|
| Compute Savings Plan | Up to 66% | 1 or 3 years |
| EC2 Instance Savings Plan | Up to 72% | 1 or 3 years |
| Payment Options | No Upfront / Partial / All Upfront | — |

- Compute Savings Plan (3yr, No Upfront): **$8.40/month** vs On-Demand $16.79
- EC2 Instance Savings Plan (3yr, No Upfront): **$7.30/month**
- Spot Instances: historical average discount of **71%** for t2.small

### Azure Reserved Instances & Savings Plans
| Option | Discount | Term |
|---|---|---|
| Azure Savings Plan | ~15% | 1 or 3 years |
| Azure Reserved Instances | Up to 72% | 1 or 3 years |
| Azure Hybrid Benefit | Up to 49% savings | Windows Server only |

- **Azure Hybrid Benefit** allows organizations with existing Windows Server
  licenses (via Software Assurance) to reuse them on Azure VMs, significantly
  reducing licensing costs for Windows-based workloads.
- For Linux workloads, Azure Reserved Instances provide the best savings.

---

## 6. Side-by-Side Service Equivalency

| Feature | AWS | Azure |
|---|---|---|
| Virtual Machine | EC2 t2.small (1 vCPU, 2 GB RAM) | DC1s v3 (1 Core, 8 GB RAM) |
| VM Monthly (On-Demand) | $16.79 | $70.08 |
| VM Monthly (Best Discount) | $7.30 (EC2 Savings Plan 3yr) | ~$59.57 (15% Savings Plan) |
| Object Storage | Amazon S3 | Azure Blob Storage |
| Storage Monthly (50 GB) | included in S3 | $2.08 |
| Egress (200 TB/month) | $14,131.20 | lower with CDN integration |
| Discount Program | Savings Plans / Spot | Reserved Instances / Hybrid Benefit |
| Free Tier | 12 months | 12 months + always free services |
| Billing Granularity | Per second (min 60s) | Per second |

---

## 7. Cost Optimization Strategies

### Strategy 1: Commitment-Based Discounts
Switch from On-Demand to a 3-year Savings Plan or Reserved Instance:
- AWS EC2 On-Demand: $16.79/month → EC2 Savings Plan: **$7.30/month** (saves ~56%)
- Azure Pay-as-you-go: $70.08/month → Reserved Instance: **~$39.24/month** (saves ~44%)

### Strategy 2: Reduce Egress Costs
The largest cost driver in this estimate is outbound data transfer ($14,131.20/month on AWS).
- Use a **CDN (CloudFront on AWS / Azure CDN)** to cache content closer to users,
  dramatically reducing origin egress.
- CDN egress rates are significantly lower than direct S3/Blob outbound rates.
- Estimated savings: **40–70% reduction** in data transfer costs.

---

## 8. Conclusion

| Metric | AWS | Azure |
|---|---|---|
| Monthly Total (On-Demand) | $14,179.51 | $72.16 |
| Annual Total | $170,154.12 | $865.92 |
| Best for | Large-scale egress, Linux startups | Microsoft-centric orgs, lower storage cost |
| Key Advantage | Mature ecosystem, spot pricing | Hybrid Benefit, lower storage egress |

For this specific workload (small VM + 50 GB storage + 200 TB egress), **Azure is
significantly more cost-effective** at $72.16/month vs AWS at $14,179.51/month,
primarily because AWS egress fees dominate the bill. However, for workloads with
low egress and high compute needs, AWS Spot Instances offer compelling value.
