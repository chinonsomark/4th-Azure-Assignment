# Provider Justification Summary

## Business Context: Linux-Based Startup

For a Linux-based startup hosting a small web application, this analysis
compares AWS and Azure across compute, storage, and data transfer costs
to determine the most cost-effective cloud provider.

## Cost Findings

After configuring equivalent services on both platforms, Azure emerges as
the more affordable option for this specific workload profile. The Azure
estimate totals **$72.16 per month** ($865.92 annually), covering a DC1s v3
Virtual Machine (1 Core, 8 GB RAM) and Blob Storage (50 GB). In contrast,
the AWS estimate totals **$14,179.51 per month** ($170,154.12 annually),
covering an EC2 t2.small instance and S3 storage with 200 TB outbound
data transfer.

The primary cost driver on AWS is egress. Transferring 200 TB per month
outbound to the internet costs $14,131.20 on AWS due to its tiered pricing
model starting at $0.09 per GB. Azure's Blob Storage egress pricing, combined
with CDN integration, offers a significantly lower alternative for
data-intensive applications.

## Recommended Provider: Azure

For a Linux-based startup with moderate-to-high data transfer requirements,
**Microsoft Azure is the recommended platform**. Azure's storage costs are
dramatically lower ($2.08/month for 50 GB vs AWS S3), and its egress pricing
is more competitive for internet-bound workloads. Additionally, Azure's
Pay-as-you-go model requires no upfront commitment, which suits early-stage
startups managing cash flow carefully.

## Cost Optimization Strategies

1. **Azure Reserved Instances (1 or 3 year):** Switching from Pay-as-you-go
   to a reserved VM reduces compute costs by up to 44%, bringing the DC1s v3
   VM from $70.08 to approximately $39.24 per month.

2. **Azure CDN Integration:** Routing outbound traffic through Azure CDN
   instead of directly from Blob Storage reduces egress costs by 40–70%,
   which is critical for applications serving large media or data files
   to global users.

## Final Verdict

While AWS offers a mature ecosystem and competitive Spot Instance pricing
(up to 71% discount for t2.small), the high egress costs make it
financially unsustainable for data-heavy workloads at this scale. Azure
provides a leaner, more predictable monthly bill for Linux-based startups
prioritizing storage and data transfer efficiency.
