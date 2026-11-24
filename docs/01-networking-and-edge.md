# 01 · Networking & Edge

> 🏠 [Back to README](README.md) · 🎯 [Pick your level](README.md#-pick-your-level) · 🌱 [Start here](README.md#-start-here)

How traffic reaches your app on Azure, and how your resources talk to each other privately. Start with **Virtual Network**, **Azure DNS** and **Front Door**; the rest you can learn when you need them.

**In this file**

- [1.1 Azure Virtual Network (VNet) and Network Security Groups](#11-azure-virtual-network-vnet-and-network-security-groups) 🟡
- [1.2 Azure DNS](#12-azure-dns) 🟢
- [1.3 Azure Front Door](#13-azure-front-door) 🟢
- [1.4 Azure Application Gateway (with WAF)](#14-azure-application-gateway-with-waf) 🟡
- [1.5 Azure Load Balancer](#15-azure-load-balancer) 🟡
- [1.6 Azure NAT Gateway](#16-azure-nat-gateway) 🟡
- [1.7 Azure Private Link and Private Endpoints](#17-azure-private-link-and-private-endpoints) 🟡
- [1.8 VPN Gateway and ExpressRoute](#18-vpn-gateway-and-expressroute) 🔴
- [1.9 Azure Firewall](#19-azure-firewall) 🔴
- [1.10 Azure DDoS Protection](#110-azure-ddos-protection) 🟡
- [1.11 Azure Bastion](#111-azure-bastion) 🟢
- [1.12 Azure Traffic Manager](#112-azure-traffic-manager) 🔴

---

## 1.1 Azure Virtual Network (VNet) and Network Security Groups

> 🟡 **Intermediate** · AWS equivalent: VPC + security groups / NACLs

**What it is:** Your private network in Azure. Every VM, private endpoint and many PaaS services (App Service, Functions, Container Apps, AKS) can join or integrate with a VNet.

**Use it for:** isolating workloads, connecting app tiers privately, linking Azure to your office or data centre.

**Key concepts**
- A **VNet** has an address space (e.g. `10.0.0.0/16`) and lives in **one region**; split it into **subnets** (e.g. `10.0.1.0/24`)
- **Network Security Groups (NSGs)**: allow/deny rules by priority, attached to a subnet or a NIC. Stateful, like AWS security groups
- **Application Security Groups (ASGs)**: name groups of VMs ("web", "db") and use them in NSG rules instead of IP addresses
- **VNet peering**: connect VNets (same or different regions) over the Microsoft backbone
- **Hub-and-spoke**: shared services (firewall, VPN, DNS) in a hub VNet, workloads in spoke VNets. **Azure Virtual WAN** is the managed version for large estates
- **Service endpoints** vs **Private Endpoints** (see [1.7](#17-azure-private-link-and-private-endpoints)): prefer Private Endpoints for new designs
- **Subnet delegation**: some services (App Service VNet integration, Container Apps, SQL Managed Instance) need a dedicated, delegated subnet
- **Private subnets by default (2026):** after 31 March 2026, new VNets created with the current API get subnets with no default outbound internet access. VMs need an explicit outbound method (NAT Gateway, a load balancer outbound rule, or a public IP)

**Pricing:** VNets and NSGs are free. You pay for peering traffic, gateways, NAT, public IPs and data leaving Azure.

**Gotchas:** Plan address spaces up front; overlapping ranges can't be peered. If a new VM "has no internet", check that the subnet has an explicit outbound path (usually a [NAT Gateway](#16-azure-nat-gateway)).

---

## 1.2 Azure DNS

> 🟢 **Beginner** · AWS equivalent: Route 53 (hosting zones)

**What it is:** Hosts your DNS zones on Microsoft's global name servers.

**Key concepts**
- **Public DNS zones**: records for `example.com` that the internet resolves
- **Private DNS zones**: names that resolve only inside linked VNets (used heavily by Private Endpoints, e.g. `privatelink.blob.core.windows.net`)
- **Alias records**: point the zone apex (`example.com`) at Front Door, Traffic Manager or a public IP
- **DNS Private Resolver**: forward DNS between Azure and on-premises networks
- Azure DNS does **not** sell domain names. Buy the domain elsewhere (or via App Service Domains) and delegate it to Azure DNS

**Pricing:** per zone per month + per million queries.

**Gotcha:** When you add a Private Endpoint, link the matching private DNS zone to every VNet that needs to resolve it, or apps will still resolve the public IP.

---

## 1.3 Azure Front Door

> 🟢 **Beginner** · AWS equivalent: CloudFront + Global Accelerator (+ WAF)

**What it is:** Microsoft's global CDN and application entry point: caching, global load balancing, TLS and Web Application Firewall at the edge, in one service.

**Use it for:** serving static sites and APIs worldwide, routing users to the nearest healthy region, protecting public apps with WAF.

**Key concepts**
- Tiers: **Standard** (CDN + routing) and **Premium** (adds managed WAF rule sets, bot protection and **Private Link to origins**)
- **Endpoints**, **routes**, **origin groups** (with health probes and priority/weight)
- **Rules engine**: rewrite URLs, add headers, redirect HTTP → HTTPS
- Free managed TLS certificates for custom domains
- **Private Link origins** (Premium) let Front Door reach an App Service, storage account or internal load balancer that has no public access

**Pricing:** base fee per profile + per request + data transfer out; WAF and Premium features cost more.

**Retirements to know:** Front Door (classic) retires **31 March 2027** and Azure CDN Standard from Microsoft (classic) retires **30 September 2027**. New classic profiles can no longer be created. Use Front Door Standard/Premium for everything new; tutorials that say "Azure CDN profile" are usually describing a classic tier.

**Gotcha:** Lock your origin so it only accepts Front Door traffic (Private Link, or the `X-Azure-FDID` header check plus the `AzureFrontDoor.Backend` service tag). Otherwise attackers can bypass the WAF.

---

## 1.4 Azure Application Gateway (with WAF)

> 🟡 **Intermediate** · AWS equivalent: Application Load Balancer + AWS WAF

**What it is:** A regional layer-7 (HTTP/HTTPS) load balancer that lives inside your VNet.

**Key concepts**
- Path-based and host-based routing, TLS termination, end-to-end TLS, autoscaling (**v2 SKU**)
- **WAF policy** with OWASP core rule sets and custom rules
- Backends can be VMs, scale sets, App Service, or IP addresses
- **Application Gateway for Containers**: a newer, Kubernetes-native layer-7 load balancer for AKS that uses the Gateway API

**Use it for:** regional web apps that must stay private to a VNet, or as the regional layer behind Front Door.

**Pricing:** per gateway-hour + capacity units; WAF SKU costs more.

**Front Door or Application Gateway?** Front Door is global and sits at the edge; Application Gateway is regional and sits in your VNet. Big apps often use both.

---

## 1.5 Azure Load Balancer

> 🟡 **Intermediate** · AWS equivalent: Network Load Balancer

**What it is:** A layer-4 (TCP/UDP) load balancer, public or internal, with very high throughput and low latency.

**Key concepts**
- **Standard SKU** only: the Basic SKU (and Basic public IPs) retired on 30 September 2025
- Frontend IP, backend pool, health probes, load-balancing rules, **outbound rules**
- **Gateway Load Balancer**: inserts third-party network virtual appliances transparently
- Zone-redundant frontends survive an availability-zone failure

**Use it for:** non-HTTP traffic, VM scale sets, internal TCP services, outbound internet for VMs.

**Pricing:** per rule-hour + data processed.

---

## 1.6 Azure NAT Gateway

> 🟡 **Intermediate** · AWS equivalent: NAT Gateway

**What it is:** Managed outbound internet access for a subnet, using static public IPs you own.

**Why it matters now:** with private subnets as the default for new VNets (see [1.1](#11-azure-virtual-network-vnet-and-network-security-groups)), NAT Gateway is the usual way to let VMs and containers call external APIs, download packages or reach SaaS services.

**Key concepts:** attach one NAT Gateway to one or more subnets; add public IPs or a public IP prefix; it scales automatically and handles far more connections than default outbound access.

**Pricing:** per gateway-hour + data processed.

**Gotcha:** Third parties often allow-list your outbound IP. NAT Gateway gives you a stable one, unlike default outbound access.

---

## 1.7 Azure Private Link and Private Endpoints

> 🟡 **Intermediate** · AWS equivalent: PrivateLink / VPC interface endpoints

**What it is:** Gives a PaaS service (Storage, SQL, Cosmos DB, Key Vault, Foundry, …) a **private IP address inside your VNet**, so traffic never crosses the public internet.

**Key concepts**
- **Private Endpoint**: a NIC in your subnet that maps to one service instance (e.g. one storage account's blob endpoint)
- **Private DNS zone** so the normal hostname resolves to the private IP
- After the endpoint works, **disable public network access** on the service
- **Private Link service**: publish your own service to other VNets or tenants

**Pricing:** per endpoint-hour + data processed.

**Gotcha:** 90% of Private Endpoint problems are DNS. Check `nslookup <service-host>` from inside the VNet returns a `10.x` address.

---

## 1.8 VPN Gateway and ExpressRoute

> 🔴 **Advanced** · AWS equivalent: Site-to-Site VPN / Client VPN and Direct Connect

**What they are:** Ways to connect Azure to networks outside it.

- **VPN Gateway**: encrypted tunnels over the internet
  - **Site-to-site** (office/data centre ↔ VNet) and **point-to-site** (laptops ↔ VNet, with Entra ID sign-in)
- **ExpressRoute**: a private, dedicated circuit through a connectivity provider, with predictable latency and higher bandwidth
  - **ExpressRoute Global Reach** links your sites through Microsoft's backbone

**Pricing:** VPN Gateway per gateway-hour (by SKU) + egress; ExpressRoute per circuit (port speed) + gateway + optional metered data.

**Gotcha:** Gateways take 30–45 minutes to create. Use a zone-redundant SKU in production.

---

## 1.9 Azure Firewall

> 🔴 **Advanced** · AWS equivalent: AWS Network Firewall

**What it is:** A managed, stateful network firewall, usually placed in the hub VNet so all spoke traffic flows through it.

**Key concepts**
- SKUs: **Basic** (small businesses), **Standard**, **Premium** (TLS inspection, IDPS, URL filtering)
- Network rules, application rules (FQDN filtering), DNAT rules
- **Firewall Manager** and firewall policies to manage many firewalls
- Route spoke traffic to it with **user-defined routes (UDRs)**

**Pricing:** per deployment-hour (by SKU) + per GB processed. It is not cheap; Basic exists for smaller budgets.

---

## 1.10 Azure DDoS Protection

> 🟡 **Intermediate** · AWS equivalent: Shield Standard / Shield Advanced

**What it is:** Protection against distributed denial-of-service attacks.

- **Infrastructure (basic) protection** is always on and free for all Azure public IPs
- **DDoS Network Protection** (per VNet plan) and **DDoS IP Protection** (per public IP) add adaptive tuning, attack telemetry, rapid response support and cost protection

**Pricing:** Network Protection has a high fixed monthly fee covering up to 100 public IPs; IP Protection is billed per protected IP.

**Gotcha:** For HTTP apps, Front Door + WAF already absorbs most layer-7 attacks; DDoS Network Protection is mostly for large estates with many public IPs.

---

## 1.11 Azure Bastion

> 🟢 **Beginner** · AWS equivalent: Systems Manager Session Manager / EC2 Instance Connect Endpoint

**What it is:** Browser-based (or native client) RDP and SSH to your VMs **without giving the VMs public IPs** or opening ports 22/3389 to the internet.

**Key concepts:** SKUs **Developer** (free, shared, for dev/test), **Basic**, **Standard**, **Premium** (session recording, private-only deployment); Entra ID sign-in to VMs.

**Pricing:** per hour by SKU + outbound data (Developer SKU is free).

**Gotcha:** Never expose SSH/RDP with a public IP "just for testing". Use Bastion's Developer SKU instead.

---

## 1.12 Azure Traffic Manager

> 🔴 **Advanced** · AWS equivalent: Route 53 routing policies (latency, failover, weighted, geolocation)

**What it is:** DNS-based global traffic routing. It answers DNS queries with the endpoint that best fits a routing method.

**Routing methods:** priority (failover), weighted, performance (lowest latency), geographic, multivalue, subnet.

**Use it for:** non-HTTP global failover, or routing between Azure and other clouds/on-premises. For HTTP apps, Front Door is usually the better choice, because it routes per request rather than per DNS lookup.

**Pricing:** per million DNS queries + per monitored endpoint.

---

🏠 [README](README.md) · [02 · Identity, Security & Governance](02-identity-security-governance.md) →
