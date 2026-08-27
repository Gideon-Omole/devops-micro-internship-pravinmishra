# Assignment 6 — Capstone: Deploy Book Review App (Three-Tier Architecture) on Azure

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

This is the most important assignment of the course. You will deploy the Book Review App in a production-ready, best-practice-compliant three-tier architecture on Azure: separated presentation, application, and database tiers, least-privilege network access, a controlled public entry point, protected secrets, and availability/monitoring evidence.

---

# Task 1 — Design the Azure Three-Tier Architecture

## Goal

Create an architecture diagram and implementation plan identifying the presentation, application, and database components, the chosen Azure services, the public entry point, and the internal traffic paths.

### Evidence

#### Screenshot 1 — Architecture diagram showing the public entry point, three tiers, network boundaries, and traffic flow

![screenshot-1](screenshots/gideon-omole-as6-scr1.jpg)

---

#### Screenshot 2 — Written architecture assumptions and selected Azure services

![screenshot-2](screenshots/gideon-omole-as6-scr2.jpg)

---

# Task 2 — Create the Azure Network Foundation

## Goal

Create a dedicated Resource Group and VNet with separate subnets for the web, application, and database tiers, keeping the application and database tiers without direct public access.

### Evidence

#### Screenshot 3 — Resource Group overview showing the assignment resources

![screenshot-3](screenshots/gideon-omole-as6-scr3.png)

---

#### Screenshot 4 — VNet overview showing the address space and all required subnets

![screenshot-3](screenshots/gideon-omole-as6-scr4.png)

---

#### Screenshot 5 — Route-table or Private DNS evidence where applicable

![screenshot-5](screenshots/gideon-omole-as6-scr5.png)

---

# Task 3 — Configure Security and Secret Management

## Goal

Apply least-privilege NSG rules so traffic flows Internet → public entry point → web tier → application tier → database tier, and store credentials in Azure Key Vault or another approved secure mechanism.

### Evidence

#### Screenshot 6 — NSG rules proving least-privilege access between the tiers

![screenshot-6](screenshots/gideon-omole-as6-scr6.1.png)
![screenshot-6](screenshots/gideon-omole-as6-scr6.2.png)

---

#### Screenshot 7 — Key Vault or approved secret-management configuration (without displaying secret values)

![screenshot-7](screenshots/gideon-omole-as6-scr7.png)

---

# Task 4 — Deploy the Presentation (Web) Tier

## Goal

Deploy the Book Review App presentation layer on the approved web-tier compute service, configured to route requests to the internal application-tier endpoint, and not directly exposed except through the public entry service.

### Evidence

#### Screenshot 8 — Web-tier compute overview showing subnet and availability configuration

![screenshot-8](screenshots/gideon-omole-as6-scr8.png)

---

#### Screenshot 9 — Terminal or service output proving the presentation layer is running

![screenshot-9](screenshots/gideon-omole-as6-scr9.png)

---

# Task 5 — Deploy the Business (Application) Tier

## Goal

Deploy the Book Review App backend privately in the application subnet, configured to use the private database endpoint and secured environment values, reachable only through its internal endpoint.

### Evidence

#### Screenshot 10 — Application-tier compute overview showing private subnet placement

![screenshot-10](screenshots/gideon-omole-as6-scr10.png)

---

#### Screenshot 11 — Backend process, service, or listening-port evidence

![screenshot-11](screenshots/gideon-omole-as7-scr11.png)

---

#### Screenshot 12 — Internal health-check or API response (without exposing secrets)

![screenshot-12](screenshots/gideon-omole-as6-scr12.png)

---

# Task 6 — Deploy the Managed Database Tier

## Goal

Create a private Azure managed database (public access disabled), with availability/backup/retention settings, the Book Review App schema imported, and access restricted to the application tier only.

### Evidence

#### Screenshot 13 — Database overview showing private connectivity and public access disabled

![screenshot-13](screenshots/gideon-omole-as6-scr13.png)

---

#### Screenshot 14 — Availability, backup, and retention configuration

![screenshot-14](screenshots/gideon-omole-as6-scr14.png)

---

#### Screenshot 15 — Successful schema or connectivity verification (without exposing credentials)

![screenshot-15](screenshots/gideon-omole-as6-scr15.png)
---

# Task 7 — Configure Traffic Management, Availability, and Monitoring

## Goal

Configure the approved public entry service with health probes and backend pools, internal routing for the application tier where required, and enable Azure Monitor/diagnostics/logs/alerts for the key resources.

### Evidence

#### Screenshot 16 — Public entry service showing listener, frontend endpoint, and healthy web targets

![screenshot-16](screenshots/gideon-omole-as6-scr16.1.png)
![screenshot-16](screenshots/gideon-omole-as6-scr16.2.png)

---

#### Screenshot 17 — Internal application-tier load-balancing or routing configuration where applicable

![screenshot-17](screenshots/gideon-omole-as6-scr17.png)

---

#### Screenshot 18 — Azure Monitor, diagnostic settings, logs, metrics, or alert evidence

![screenshot-18](screenshots/gideon-omole-as6-scr18.1.png)
![screenshot-18](screenshots/gideon-omole-as6-scr18.2.png)

---

# Task 8 — Validate the Production-Style Deployment

## Goal

Confirm the Book Review App works end to end through the public endpoint, with at least one database read and one write, confirm private tiers are not internet-reachable, and complete a safe availability test.

### Evidence

#### Screenshot 19 — Browser showing the Book Review App through the public endpoint

![screenshot-19](screenshots/gideon-omole-as6-scr19.png)

---

#### Screenshot 20 — Proof of successful database-backed read and write operations

![screenshot-20](screenshots/gideon-omole-as6-scr20.png)

---

#### Screenshot 21 — Evidence that private tiers are not publicly accessible

![screenshot-21](screenshots/gideon-omole-as6-scr21.png)

---

#### Screenshot 22 — Availability-test and healthy-target evidence

![screenshot-22](screenshots/gideon-omole-as6-scr22.1.png)
![screenshot-22](screenshots/gideon-omole-as6-scr22.2.png)

---

#### Public Endpoint

Paste your public endpoint URL here:

`http://20.164.205.125/`

---

### Notes

Summarize what worked, issues encountered and how they were fixed, and the availability/security/secrets/monitoring/backup choices made.

### What Worked

Deployed the Book Review App (Next.js frontend, Node/Express backend, MySQL database) on Azure using a fully private three-tier architecture: only the Application Gateway holds a public IP; the web tier, app tier, and database are all unreachable directly from the internet. Traffic flows Internet → Application Gateway → Web tier (Nginx + Next.js) → internal Load Balancer → App tier (Express) → MySQL Flexible Server (private access). Both web and app tiers run two instances each behind health-probed load balancers, and an availability test (stopping one web instance) confirmed traffic automatically failed over to the healthy instance with no visible downtime. Registration, login, and review read/write all work end to end through the public endpoint.

### Issues Encountered and How They Were Fixed

- **System-Assigned Managed Identity blocked by subscription policy** — switched to a User-Assigned Managed Identity, attached to the app-tier VMSS, granted `Key Vault Secrets User` access.
- **Private subnets had no outbound internet access**, causing `apt-get`/`npm install` to time out — resolved by adding a NAT Gateway attached to the web and app subnets, preserving inbound isolation while allowing outbound package installs.
- **VMSS used Flexible orchestration**, so instances have generated names (e.g. `vmss-app_34965459`) rather than numeric IDs — all administrative commands used `az vm run-command invoke` targeting instance names directly, since the VMs have no public IP for SSH.
- **MySQL Flexible Server creation required an exact version string** (`8.0.21`) and rejected the `--public-access` flag when combined with `--vnet`/`--subnet` (private access is implied by subnet placement, not a separate flag).
- **Database password containing a `#` character was silently truncated by dotenv**, which treats `#` as a comment delimiter — caused an "Access denied" error even though the password was correct. Fixed by quoting sensitive `.env` values.
- **App's ORM (Sequelize) required DDL privileges** (`CREATE`, `ALTER`, `INDEX`, `REFERENCES`) for its auto-migration-on-startup pattern, beyond the initial least-privilege grant of `SELECT/INSERT/UPDATE/DELETE`. Granted the additional privileges, scoped only to the application database.
- **Key Vault RBAC role assignments were blocked by an ABAC condition** on the account's Owner role restricting which roles could be delegated — even to itself. Resolved by reconfiguring the Owner role assignment to remove the constraint.
- **Frontend initially failed with "No books available"** because `NEXT_PUBLIC_API_URL` was set to a private Load Balancer IP unreachable from a public browser — client-side JavaScript runs in the visitor's browser, not inside the VNet. Fixed by routing API calls through the same public-facing Nginx proxy via a relative path instead of a private IP.
- **Registration failed with 404s and inconsistent routing** due to two files in the repo (`page.js` and `services/api.js`) using different conventions for whether `/api` should be included in the base URL. Standardized `api.js` to match the pattern already used by `page.js`.
- **Registration then failed with CORS errors** — `ALLOWED_ORIGINS=*` was not treated as a wildcard by the app's custom CORS check, which expects a literal origin string. Fixed by setting the real Application Gateway public IP as the explicit allowed origin. This bug was invisible to `curl`-based testing since CORS is enforced by browsers, not servers.
- Several Azure CLI commands failed on Windows/Git Bash due to automatic path conversion of strings starting with `/` (e.g. `/subscriptions/...`) — resolved by prefixing affected commands with `MSYS_NO_PATHCONV=1`.

### Availability, Security, Secrets, Monitoring, and Backup Choices

- **Availability:** Two instances per tier (web, app) behind health-probed load balancing; Application Gateway (public) and an internal Standard Load Balancer (app tier) both configured with HTTP health probes and automatic unhealthy-instance removal. Verified via a live stop/restart test.
- **Security:** Least-privilege NSGs restrict each tier to only the specific port and source subnet it needs (web tier: port 80 from the gateway subnet only; app tier: port 3001 from the web subnet only; database: port 3306 from the app subnet only). No public IPs on web, app, or database tiers.
- **Secrets:** Database credentials stored in Azure Key Vault; application-level database user (`bookapp`) scoped to least-privilege permissions on a single database, distinct from the server admin account.
- **Monitoring:** Centralized Log Analytics workspace with diagnostic settings enabled on the Application Gateway (access, firewall, and performance logs) and MySQL server (slow query and audit logs), plus an alert rule on Application Gateway unhealthy backend count.
- **Backup:** MySQL Flexible Server configured with 14-day automated backup retention.

---

# Submission Instructions

- Add all required screenshots and links in your submission
- Do not expose passwords, keys, connection strings, or subscription IDs

---

# Completion Checklist

- [ ] Task 1: Architecture diagram and assumptions documented (Screenshots 1–2)
- [ ] Task 2: Network foundation created with isolated tiers (Screenshots 3–5)
- [ ] Task 3: Least-privilege security and secret management configured (Screenshots 6–7)
- [ ] Task 4: Presentation tier deployed (Screenshots 8–9)
- [ ] Task 5: Application tier deployed privately (Screenshots 10–12)
- [ ] Task 6: Managed database tier deployed privately (Screenshots 13–15)
- [ ] Task 7: Public entry, internal routing, and monitoring configured (Screenshots 16–18)
- [ ] Task 8: End-to-end validation and availability test completed (Screenshots 19–22, Public Endpoint, Notes)
- [ ] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
