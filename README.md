# Azure Services Guide

A free, beginner-friendly guide to the **~85 Microsoft Azure services** that power most real-world systems: websites, APIs, mobile and web back ends, data platforms and AI apps.

Every service follows the same simple layout: **what it is**, **when to use it**, **key concepts**, **pricing**, **common mistakes**, its **AWS equivalent**, and a **Python example** for many of them.

> 📅 Last reviewed: **November 2026**. Azure changes fast; see [What changed in 2025–2026](#-what-changed-in-20252026) and please open a pull request if you spot something outdated.
>
> ☁️ Coming from AWS? There's a companion [AWS Services Guide](https://github.com/kazimsadique/aws-services-guide) with the same structure, and an [AWS → Azure service map](12-comparisons-and-architectures.md#aws--azure-service-map).

---

## 📑 Contents

- [🎯 Pick your level](#-pick-your-level)
- [📂 Services by category](#-services-by-category)
- [🌱 Start here](#-start-here)
- [💸 Avoid surprise bills](#-avoid-surprise-bills)
- [🐍 Running the Python examples](#-running-the-python-examples)
- [🔀 How a request flows through Azure](#-how-a-request-flows-through-azure)
- [🆕 What changed in 2025–2026](#-what-changed-in-20252026)
- [📖 How to use this guide](#-how-to-use-this-guide)
- [🤝 Contributing](#-contributing)
- [📄 License](#-license)

---

## 🎯 Pick your level

Every service has a level badge, so you know what to learn now and what can wait.

| Badge | Level | Who it's for | Start with |
|---|---|---|---|
| 🟢 | **Beginner** | New to Azure or to cloud | [Start here](#-start-here), then every 🟢 service |
| 🟡 | **Intermediate** | You've deployed something and want production skills | Networking, messaging, databases, IaC, plus the [comparisons](12-comparisons-and-architectures.md) |
| 🔴 | **Advanced** | Platform, data or security engineers | AKS, Firewall, ExpressRoute, Sentinel, Machine Learning, Deployment Stacks |

---

## 📂 Services by category

| # | Category | Services |
|---|---|---|
| 01 | [🌐 Networking & Edge](01-networking-and-edge.md) | [Virtual Network + NSGs](01-networking-and-edge.md#11-azure-virtual-network-vnet-and-network-security-groups) 🟡 · [DNS](01-networking-and-edge.md#12-azure-dns) 🟢 · [Front Door](01-networking-and-edge.md#13-azure-front-door) 🟢 · [Application Gateway](01-networking-and-edge.md#14-azure-application-gateway-with-waf) 🟡 · [Load Balancer](01-networking-and-edge.md#15-azure-load-balancer) 🟡 · [NAT Gateway](01-networking-and-edge.md#16-azure-nat-gateway) 🟡 · [Private Link](01-networking-and-edge.md#17-azure-private-link-and-private-endpoints) 🟡 · [VPN Gateway + ExpressRoute](01-networking-and-edge.md#18-vpn-gateway-and-expressroute) 🔴 · [Firewall](01-networking-and-edge.md#19-azure-firewall) 🔴 · [DDoS Protection](01-networking-and-edge.md#110-azure-ddos-protection) 🟡 · [Bastion](01-networking-and-edge.md#111-azure-bastion) 🟢 · [Traffic Manager](01-networking-and-edge.md#112-azure-traffic-manager) 🔴 |
| 02 | [🔐 Identity, Security & Governance](02-identity-security-governance.md) | [Resource hierarchy](02-identity-security-governance.md#21-azure-resource-hierarchy-tenants-management-groups-subscriptions-and-resource-groups) 🟢 · [Entra ID](02-identity-security-governance.md#22-microsoft-entra-id) 🟢 · [Azure RBAC](02-identity-security-governance.md#23-azure-role-based-access-control-azure-rbac) 🟢 · [Managed identities](02-identity-security-governance.md#24-managed-identities) 🟢 · [Entra External ID](02-identity-security-governance.md#25-microsoft-entra-external-id) 🟡 · [Key Vault](02-identity-security-governance.md#26-azure-key-vault) 🟢 · [Defender for Cloud](02-identity-security-governance.md#27-microsoft-defender-for-cloud) 🟡 · [Sentinel](02-identity-security-governance.md#28-microsoft-sentinel) 🔴 · [Policy](02-identity-security-governance.md#29-azure-policy) 🟡 |
| 03 | [⚙️ Compute & Containers](03-compute-and-containers.md) | [Virtual Machines](03-compute-and-containers.md#31-azure-virtual-machines) 🟢 · [Virtual Machine Scale Sets](03-compute-and-containers.md#32-virtual-machine-scale-sets) 🟡 · [App Service](03-compute-and-containers.md#33-azure-app-service) 🟢 · [Functions](03-compute-and-containers.md#34-azure-functions) 🟢 · [Container Apps](03-compute-and-containers.md#35-azure-container-apps) 🟢 · [AKS](03-compute-and-containers.md#36-azure-kubernetes-service-aks) 🔴 · [Container Instances](03-compute-and-containers.md#37-azure-container-instances-aci) 🟡 · [Container Registry](03-compute-and-containers.md#38-azure-container-registry-acr) 🟢 · [Static Web Apps](03-compute-and-containers.md#39-azure-static-web-apps) 🟢 · [Batch](03-compute-and-containers.md#310-azure-batch) 🔴 |
| 04 | [🔄 APIs & Integration](04-apis-and-integration.md) | [API Management](04-apis-and-integration.md#41-azure-api-management-apim) 🟡 · [Service Bus](04-apis-and-integration.md#42-azure-service-bus) 🟢 · [Event Grid](04-apis-and-integration.md#43-azure-event-grid) 🟡 · [Event Hubs](04-apis-and-integration.md#44-azure-event-hubs) 🟡 · [Queue Storage](04-apis-and-integration.md#45-azure-queue-storage) 🟢 · [Logic Apps](04-apis-and-integration.md#46-azure-logic-apps) 🟢 · [Web PubSub + SignalR](04-apis-and-integration.md#47-azure-web-pubsub-and-azure-signalr-service) 🟡 · [App Configuration](04-apis-and-integration.md#48-azure-app-configuration) 🟡 |
| 05 | [🗄️ Storage & Caching](05-storage-and-caching.md) | [Blob Storage](05-storage-and-caching.md#51-azure-blob-storage) 🟢 · [Data Lake Storage Gen2](05-storage-and-caching.md#52-azure-data-lake-storage-gen2) 🟡 · [Managed Disks](05-storage-and-caching.md#53-azure-managed-disks) 🟢 · [Files](05-storage-and-caching.md#54-azure-files) 🟡 · [Managed Redis](05-storage-and-caching.md#55-azure-managed-redis) 🟡 |
| 06 | [🧮 Databases](06-databases.md) | [SQL Database](06-databases.md#61-azure-sql-database) 🟢 · [SQL Managed Instance](06-databases.md#62-azure-sql-managed-instance) 🔴 · [PostgreSQL](06-databases.md#63-azure-database-for-postgresql-flexible-server) 🟢 · [MySQL](06-databases.md#64-azure-database-for-mysql-flexible-server) 🟢 · [Cosmos DB](06-databases.md#65-azure-cosmos-db) 🟡 · [DocumentDB](06-databases.md#66-azure-documentdb-with-mongodb-compatibility) 🟡 |
| 07 | [🤖 AI & Machine Learning](07-ai-and-machine-learning.md) | [Foundry](07-ai-and-machine-learning.md#71-microsoft-foundry) 🟢 · [Foundry Models / Azure OpenAI](07-ai-and-machine-learning.md#72-foundry-models-and-azure-openai) 🟢 · [Foundry Agent Service](07-ai-and-machine-learning.md#73-foundry-agent-service) 🟡 · [AI Search](07-ai-and-machine-learning.md#74-azure-ai-search) 🟡 · [Foundry Tools](07-ai-and-machine-learning.md#75-foundry-tools-prebuilt-ai-services) 🟢 · [Machine Learning](07-ai-and-machine-learning.md#76-azure-machine-learning) 🔴 |
| 08 | [📊 Data & Analytics](08-data-and-analytics.md) | [Fabric](08-data-and-analytics.md#81-microsoft-fabric) 🟡 · [Data Factory](08-data-and-analytics.md#82-azure-data-factory) 🟡 · [Databricks](08-data-and-analytics.md#83-azure-databricks) 🟡 · [Synapse Analytics](08-data-and-analytics.md#84-azure-synapse-analytics) 🔴 · [Stream Analytics](08-data-and-analytics.md#85-azure-stream-analytics) 🟡 · [Data Explorer](08-data-and-analytics.md#86-azure-data-explorer) 🔴 · [Power BI](08-data-and-analytics.md#87-power-bi) 🟢 · [Purview](08-data-and-analytics.md#88-microsoft-purview) 🟡 |
| 09 | [📈 Observability & Operations](09-observability-and-operations.md) | [Monitor](09-observability-and-operations.md#91-azure-monitor) 🟢 · [Log Analytics + KQL](09-observability-and-operations.md#92-log-analytics-workspaces-and-kql) 🟢 · [Application Insights](09-observability-and-operations.md#93-application-insights) 🟢 · [Managed Prometheus + Grafana](09-observability-and-operations.md#94-azure-managed-prometheus-and-azure-managed-grafana) 🟡 · [Cost Management](09-observability-and-operations.md#95-microsoft-cost-management) 🟢 · [Advisor](09-observability-and-operations.md#96-azure-advisor) 🟢 · [Backup + Site Recovery](09-observability-and-operations.md#97-azure-backup-and-azure-site-recovery) 🟡 · [Service Health](09-observability-and-operations.md#98-azure-service-health-and-resource-health) 🟢 |
| 10 | [🏗️ Infrastructure as Code & DevOps](10-infrastructure-as-code-and-devops.md) | [ARM + Bicep](10-infrastructure-as-code-and-devops.md#101-azure-resource-manager-arm-and-bicep) 🟡 · [Deployment Stacks](10-infrastructure-as-code-and-devops.md#102-deployment-stacks-and-template-specs) 🔴 · [Terraform](10-infrastructure-as-code-and-devops.md#103-terraform-on-azure) 🟡 · [Developer CLI (azd)](10-infrastructure-as-code-and-devops.md#104-azure-developer-cli-azd) 🟢 · [DevOps](10-infrastructure-as-code-and-devops.md#105-azure-devops) 🟡 · [GitHub Actions](10-infrastructure-as-code-and-devops.md#106-github-actions-for-azure) 🟢 · [Load Testing + Chaos Studio](10-infrastructure-as-code-and-devops.md#107-azure-load-testing-and-azure-chaos-studio) 🔴 |
| 11 | [🧰 Developer Tools, AI Assistants & Hybrid](11-developer-tools-and-hybrid.md) | [Portal, CLI + Cloud Shell](11-developer-tools-and-hybrid.md#111-azure-portal-azure-cli-and-cloud-shell) 🟢 · [SDK for Python](11-developer-tools-and-hybrid.md#112-azure-sdk-for-python) 🟢 · [Azure MCP Server + Copilot for Azure](11-developer-tools-and-hybrid.md#113-azure-mcp-server-and-github-copilot-for-azure) 🟡 · [Arc](11-developer-tools-and-hybrid.md#114-azure-arc) 🔴 · [Migrate](11-developer-tools-and-hybrid.md#115-azure-migrate) 🟡 · [Virtual Desktop](11-developer-tools-and-hybrid.md#116-azure-virtual-desktop) 🟡 · [Communication Services](11-developer-tools-and-hybrid.md#117-azure-communication-services) 🟡 |
| 12 | [⚖️ Comparisons & Reference Architectures](12-comparisons-and-architectures.md) | Side-by-side “which one do I pick?” tables, a full AWS → Azure service map, and 6 reference architectures from a static site to a GenAI/RAG app |
| 13 | [📚 Glossary](13-glossary.md) | ~80 cloud and Azure terms in plain language, plus a table of old service names → current names |

---

## 🌱 Start here

New to Azure? Learn these **six things first**, in this order. Together they cover enough to deploy a real app.

1. **[Resource groups and subscriptions](02-identity-security-governance.md#21-azure-resource-hierarchy-tenants-management-groups-subscriptions-and-resource-groups)**: how everything is organised (and how to delete it all at once)
2. **[Entra ID, RBAC and managed identities](02-identity-security-governance.md#22-microsoft-entra-id)**: who can do what, and how apps sign in without passwords
3. **[Blob Storage](05-storage-and-caching.md#51-azure-blob-storage)**: store files and images
4. **[App Service](03-compute-and-containers.md#33-azure-app-service)** or **[Container Apps](03-compute-and-containers.md#35-azure-container-apps)**: run your web app or API
5. **[Azure Database for PostgreSQL](06-databases.md#63-azure-database-for-postgresql-flexible-server)** or **[Cosmos DB](06-databases.md#65-azure-cosmos-db)**: store your data
6. **[Azure Monitor and Application Insights](09-observability-and-operations.md#93-application-insights)**: see what your app is doing

Then add **[Azure Functions](03-compute-and-containers.md#34-azure-functions)** for background jobs and **[Microsoft Foundry](07-ai-and-machine-learning.md#71-microsoft-foundry)** when you're ready to add AI. Unsure about a word? Check the **[glossary](13-glossary.md)**.

**Suggested first project:** deploy a small FastAPI app to Container Apps with a PostgreSQL database, using `azd up` ([10.4](10-infrastructure-as-code-and-devops.md#104-azure-developer-cli-azd)). When you're done, run `azd down --purge`.

---

## 💸 Avoid surprise bills

- **Create a budget on day one** in [Cost Management](09-observability-and-operations.md#95-microsoft-cost-management), with an email alert at a small amount
- **Use one resource group per experiment** and delete the group when you finish: `az group delete --name <rg> --yes`
- **Deallocate VMs** you're not using (stopping inside the OS isn't enough), and delete leftover **disks** and **public IPs**
- **Prefer serverless and free tiers while learning:** Functions Flex Consumption, Container Apps (scale to zero), Azure SQL serverless with auto-pause, the Cosmos DB free tier, Static Web Apps Free, AI Search Free
- **Watch the usual surprises:** Log Analytics ingestion, NAT/Firewall/VPN gateways that run 24/7, Premium tiers left on after a demo, Defender plans turned on everywhere, and data transfer out of Azure
- Check **[Azure Advisor](09-observability-and-operations.md#96-azure-advisor)** cost recommendations weekly

---

## 🐍 Running the Python examples

The examples use the official **Azure SDK for Python** and sign in with **Microsoft Entra ID**, not keys or connection strings.

1. **Install the Azure CLI** and sign in:
   ```bash
   az login
   az account show          # check you're in the right subscription
   ```
2. **Create a virtual environment and install packages** (each example lists the package in its heading):
   ```bash
   python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
   pip install azure-identity azure-storage-blob        # plus the package for your example
   ```
3. **Give yourself the data-plane role** the example needs, e.g. *Storage Blob Data Contributor* on the storage account. Being *Owner* or *Contributor* is **not** enough for most data operations (see [RBAC](02-identity-security-governance.md#23-azure-role-based-access-control-azure-rbac)). New role assignments can take a few minutes to work.
4. **Replace the placeholders** (`<your-subscription-id>`, `stshopdev`, `kv-shop-dev`…) with your own resource names.
5. **Delete what you create** when you're done.

`DefaultAzureCredential` uses your `az login` session on your laptop and the app's **managed identity** in Azure, so the same code works in both places.

---

## 🔀 How a request flows through Azure

A typical production web app, from the user's browser to the database:

```
1. DNS          User looks up shop.example.com ──► Azure DNS
2. Edge         Front Door: TLS, caching, WAF, picks the nearest healthy region
3. Network      Private Link into a Virtual Network (the app has no public endpoint)
4. Compute      Container Apps / App Service runs the API (scales on traffic)
5. Identity     The app's managed identity gets Entra ID tokens; users sign in via Entra External ID
6. Data         PostgreSQL / Cosmos DB / Blob Storage / Managed Redis over Private Endpoints
7. Async work   Service Bus queue ──► Functions or a Container Apps job
8. Secrets      Key Vault (only for what can't use managed identity)
9. Observe      Application Insights + Log Analytics; alerts to an action group
10. Ship        Bicep or Terraform + GitHub Actions (OIDC, no stored secrets)
```

See all six [reference architectures](12-comparisons-and-architectures.md#architecture-1-static-website) for more patterns.

---

## 🆕 What changed in 2025–2026

Many tutorials still miss these:

| Change | What to do | Details |
|---|---|---|
| **Azure AI Foundry → Microsoft Foundry**; Azure AI services → **Foundry Tools** (Nov 2025) | Use the new names when searching docs | [7.1](07-ai-and-machine-learning.md#71-microsoft-foundry) |
| **Cosmos DB for MongoDB (vCore) → Azure DocumentDB** (Nov 2025) | Same service, new name | [6.6](06-databases.md#66-azure-documentdb-with-mongodb-compatibility) |
| **Azure Cache for Redis is retiring** (Enterprise Mar 2027; Basic/Standard/Premium Sep 2028) | Use Azure Managed Redis | [5.5](05-storage-and-caching.md#55-azure-managed-redis) |
| **Functions Linux Consumption plan retires** Sep 2028 | Use Flex Consumption | [3.4](03-compute-and-containers.md#34-azure-functions) |
| **New VNets get private subnets by default** (after Mar 2026) | Add a NAT Gateway for outbound internet | [1.1](01-networking-and-edge.md#11-azure-virtual-network-vnet-and-network-security-groups) |
| **Key Vault defaults to RBAC** (API 2026-02-01; old APIs retire Feb 2027) | Use RBAC and update template API versions | [2.6](02-identity-security-governance.md#26-azure-key-vault) |
| **Sentinel moves to the Defender portal** (Azure portal ends Mar 2027) | Learn Sentinel in the Defender portal | [2.8](02-identity-security-governance.md#28-microsoft-sentinel) |
| **Azure Blueprints retires** Jan 2027 | Use Deployment Stacks + template specs | [10.2](10-infrastructure-as-code-and-devops.md#102-deployment-stacks-and-template-specs) |
| **Front Door (classic) and CDN (classic) retire** in 2027 | Use Front Door Standard/Premium | [1.3](01-networking-and-edge.md#13-azure-front-door) |
| **Azure MCP Server** reached 1.0 (Oct 2025) and 3.0 (Oct 2026) | Connect AI coding agents to Azure | [11.3](11-developer-tools-and-hybrid.md#113-azure-mcp-server-and-github-copilot-for-azure) |
| **Application Insights → OpenTelemetry** | Use the Azure Monitor OpenTelemetry Distro | [9.3](09-observability-and-operations.md#93-application-insights) |
| **Fabric is Microsoft's analytics direction** | Start new analytics on Fabric or Databricks, not Synapse | [8.4](08-data-and-analytics.md#84-azure-synapse-analytics) |

The full list of renamed services is in the [glossary](13-glossary.md#old-names--current-names).

---

## 📖 How to use this guide

1. **Pick a level** or **browse by category** using the tables above.
2. Read a service's **What it is** and **Use it for** first; come back to **Key concepts** when you build with it.
3. Use **[12 · Comparisons](12-comparisons-and-architectures.md)** whenever two services look alike.
4. Every file has a **table of contents** at the top and **previous / next** links at the bottom, so you can also read it like a book, from 01 to 13.

---

## 🤝 Contributing

Found something outdated, wrong or missing? Pull requests and issues are welcome. Please:
- keep the same layout for each service (level badge, AWS equivalent, what it is, key concepts, pricing, gotchas)
- link to official Microsoft Learn pages for retirement dates and renames
- keep explanations short and beginner-friendly

## 📄 License

This guide is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE). You're free to share and adapt it, including commercially, as long as you give credit and link back to this repository.

---

Not affiliated with or endorsed by Microsoft. Azure and the service names are trademarks of Microsoft. Prices and features change; always check the official pricing pages before you commit to anything.
