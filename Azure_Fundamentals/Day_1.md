# Day 1: Azure Fundamentals

**Date:** February 4, 2026

## What I Learned Today

### Cloud Service Models
- **IaaS (Infrastructure as a Service):** You manage VMs, storage, networking. Azure provides hardware.
- **PaaS (Platform as a Service):** Azure manages the infrastructure. You just deploy your app/database.
- **SaaS (Software as a Service):** Fully managed apps. You just use them.

### Azure Services Covered

**Compute:**
- Virtual Machines (VMs) — IaaS compute

**Databases:**
- **Azure SQL Database** — PaaS relational DB. Good for transactional workloads, ACID guarantees.
- **Cosmos DB** — PaaS NoSQL. Globally distributed, multiple consistency models. Use when you need massive scale and can trade strict consistency.

**Networking:**
- **VNet (Virtual Network):** Isolated network in Azure. Like your own private cloud network.
- **Subnet:** Segments inside a VNet. Used to separate resources and apply security rules.

**Storage:**
- **Blob Storage:** For unstructured data (files, logs, images). Massive scale. Used heavily in data lakes.
- **Azure Files:** SMB file shares. Use when you need Windows-compatible file storage.

## What Actually Clicked

1. **Azure SQL vs Cosmos DB:** It's not just SQL vs NoSQL. It's about when you need strict consistency (SQL) vs when you need global distribution and can accept eventual consistency (Cosmos).

2. **VNets as security boundaries:** VNets aren't just networking. They control which services can talk to each other. That's an architecture decision, not just a technical setup.

3. **Blob vs Files:** Blobs = data lake scale. Files = when you need SMB protocol or Windows apps. Choosing wrong = pain later.

## What I'm Building Toward
- Understanding these basics so I can design real data pipelines
- Next: Azure Data Factory (orchestration)

## Questions I Still Have
- When exactly do you use Cosmos DB over Azure SQL in a data pipeline?
- How do you secure data moving between VNets?

## Resources
- [Azure documentation](https://docs.microsoft.com/azure)

---

**Status:** ✅ Fundamentals covered. Moving to ADF next.
