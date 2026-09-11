# Capstone Assignment — Deploy the Book Review App Using Terraform and Claude Code Agentic AI

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Student Details

**Full Name:** Add your full name here  
**Cloud Platform:** AWS or Azure  
**GitHub Repository URL:** Add your repository URL here  
**Public Application URL / Load-Balancer DNS:** Add the public URL or DNS here

---

## Purpose

Deploy the Book Review App using Terraform on AWS or Azure in a secure, highly available, production-style three-tier architecture. Use Claude Code, specialized subagents, Terraform MCP, and validation hooks to support the engineering workflow while keeping all infrastructure-changing operations under human control.

---

# Task 0 — Prepare the Project and Agentic AI Environment

## Goal

Prepare the Book Review App project and configure the provided Claude Code Agentic AI starter kit with project context, specialized subagents, Terraform MCP, validation hooks, and safety guardrails.

## Evidence

### Screenshot 1 — Project `CLAUDE.md`

Add a screenshot of the project `CLAUDE.md` showing the three-tier architecture, security boundaries, Terraform requirements, and human-approval rules.

![screenshot-1](screenshots/gideon-omole-as5-scr1.png)

---

### Screenshot 2 — Terraform Engineer Subagent

Add a screenshot showing the Terraform Engineer subagent configuration.

![screenshot-1](screenshots/gideon-omole-as5-scr2.png)

---

### Screenshot 3 — Architecture and Security Reviewer Subagent

Add a screenshot showing the Architecture and Security Reviewer subagent configuration.

![screenshot-3](screenshots/gideon-omole-as5-scr3.png)

---

### Screenshot 4 — Terraform MCP Connection

Add a screenshot showing Terraform MCP connected and available.

![screenshot-4](screenshots/gideon-omole-as5-scr4.png)

---

### Screenshot 5 — Validation Hooks

Add a screenshot showing the configured Claude Code validation hooks.

![screenshot-5](screenshots/gideon-omole-as5-scr5.png)

---

# Task 1 — Design the Three-Tier Architecture

## Goal

Design the required secure, highly available three-tier architecture and create an architecture diagram before building the infrastructure.

The diagram must show:

- VPC or VNet
- Availability Zones or equivalent availability locations
- Six subnets
- Internet connectivity
- NAT or outbound design
- Public load balancer
- Web Tier
- Internal load balancer
- Application Tier
- Managed MySQL
- Read replica
- Main traffic flow

## Architecture Diagram

![screenshot](screenshots/gideon-omole-as5-architecture.jpg)

---

# Task 2 — Build the Terraform Networking and Security Layers

## Goal

Create the modular Terraform project and implement the network and security layers across the required public and private subnets.

## Evidence

### Screenshot 6 — Modular Terraform Project Structure

Add a screenshot showing the modular Terraform project structure.

![screenshot-6](screenshots/gideon-omole-as5-scr6.png)

---

### Screenshot 7 — Six-Subnet Architecture

Add a screenshot showing the six-subnet architecture across two availability locations.

![screenshot-7](screenshots/gideon-omole-as5-scr7.png)

---

### Screenshot 8 — Public and Private Tier Separation

Add a screenshot showing the public and private tier separation, including routing and security boundaries.

![screenshot-8](screenshots/gideon-omole-as5-scr8.1.png)
![screenshot-8](screenshots/gideon-omole-as5-scr8.2.png)

---

# Task 3 — Build the Load-Balancing and Compute Layers

## Goal

Deploy the public and internal load balancers and the Web and Application compute resources required by the Book Review App.

## Evidence

### Screenshot 9 — Web and Application Compute

Add a screenshot showing the Web and Application compute resources in their required subnets.

![screenshot-9](screenshots/gideon-omole-as5-scr9.1.png)
![screenshot-9](screenshots/gideon-omole-as5-scr9.2.png)

---

### Screenshot 10 — Public Load Balancer

Add a screenshot showing the internet-facing public load balancer.

![screenshot-10](screenshots/gideon-omole-as5-scr10.png)

---

### Screenshot 11 — Internal Load Balancer

Add a screenshot showing the private internal load balancer.

![screenshot-11](screenshots/gideon-omole-as6-scr11.png)

---

### Screenshot 12 — Healthy Targets

Add a screenshot showing healthy target groups or backend pools.

![screenshot-12](screenshots/gideon-omole-as5-scr12.png)

---

# Task 4 — Build the Managed MySQL Database Layer

## Goal

Deploy a private, highly available managed MySQL database with a read replica and restrict database connectivity to the Application Tier.

## Evidence

### Screenshot 13 — Managed MySQL Database

Add a screenshot showing the managed MySQL database deployment.

![screenshot-13](screenshots/gideon-omole-as5-scr13.png)

---

### Screenshot 14 — High Availability

Add a screenshot showing the Multi-AZ or high-availability configuration.

![screenshot-14](screenshots/gideon-omole-as5-scr14.png)

---

### Screenshot 15 — Read Replica

Add a screenshot showing the read replica configuration.

![screenshot-15](screenshots/gideon-omole-as5-scr15.png)

---

### Screenshot 16 — Private Database Access

Add a screenshot showing that the database is private and accepts MySQL traffic only from the Application Tier.

![screenshot-16](screenshots/gideon-omole-as5-scr16.png)

---

# Task 5 — Validate, Review, and Apply the Terraform Configuration

## Goal

Validate the Terraform configuration, review the execution plan using both Agentic AI and human judgment, and apply the infrastructure changes only after all required checks pass.

## Evidence

### Screenshot 17 — Terraform Validation

Add a screenshot showing successful `terraform validate` output.

![screenshot-17](screenshots/gideon-omole-as5-scr17.png)

---

### Screenshot 18 — Terraform Plan

Add a screenshot showing the Terraform plan output.

![screenshot-18](screenshots/gideon-omole-as5-scr18.png)

---

### Screenshot 19 — Terraform Apply

Add a screenshot showing successful `terraform apply` completion.

![screenshot-19](screenshots/gideon-omole-as5-scr19.png)

---

# Task 6 — Deploy and Configure the Book Review Application

## Goal

Deploy and configure the Book Review App across the Web, Application, and Database tiers and verify the complete application functionality.

## Evidence

### Screenshot 20 — Homepage

Add a screenshot showing the Book Review App homepage through the public endpoint.

![screenshot-20](screenshots/gideon-omole-as5-scr20.png)

---

### Screenshot 21 — Login or Authentication

Add a screenshot showing successful login or authentication.

![screenshot-21](screenshots/gideon-omole-as5-scr21.png)

---

### Screenshot 22 — Book Data

Add a screenshot showing the book listing or book details.

![screenshot-22](screenshots/gideon-omole-as5-scr22.png)

---

### Screenshot 23 — Review Functionality

Add a screenshot showing the review functionality working successfully.

![screenshot-23](screenshots/gideon-omole-as5-scr23.png)

---

### Screenshot 24 — Backend or API Evidence

Add a screenshot showing that the backend or API is working successfully.

![screenshot-24](screenshots/gideon-omole-as5-scr24.png)

---

### Screenshot 25 — Database Reads and Writes

Add a screenshot showing successful database reads and writes.

![screenshot-25](screenshots/gideon-omole-as5-scr25.png)

## Public Application URL

**Public Application URL / DNS:** http://bookreview-dev-alb-public-571321512.us-east-1.elb.amazonaws.com

---

# Task 7 — Demonstrate the Agentic AI Workflow

## Goal

Demonstrate how Claude Code assisted with Terraform generation, architecture and security review, and evidence-based troubleshooting while infrastructure-changing decisions remained under human control.

You do not need to submit your complete Claude Code conversation history. Include only focused evidence.

## Evidence

### Screenshot 26 — AI-Assisted Terraform Generation

Add a screenshot showing one useful example of AI-assisted Terraform generation or improvement.

![screenshot-26](screenshots/gideon-omole-as5-scr26.png)

---

### Screenshot 27 — Architecture or Security Review

Add a screenshot showing one structured architecture or security review result.

![screenshot-27](screenshots/gideon-omole-as5-scr27.png)

---

### Screenshot 28 — AI-Assisted Troubleshooting

Add a screenshot showing one AI-assisted troubleshooting interaction based on collected evidence.

![screenshot-28](screenshots/gideon-omole-as5-scr28.png)


---

# Task 8 — Complete the Final Architecture Review

## Goal

Review the completed infrastructure against the original capstone requirements and resolve significant architecture, security, reliability, and cost issues.

Confirm that the final review covers:

- Tier separation
- Availability
- Public exposure
- Routing
- Security rules
- Load balancing
- Database privacy
- Secrets
- Terraform quality
- Module structure
- Reliability
- Obvious cost risks

Use Screenshot 27 as the focused evidence for the structured architecture or security review.

---

# Task 9 — Answer the Reflection Questions

## Goal

Reflect on the architecture, Terraform implementation, and Agentic AI workflow. Answer each question briefly in your own words.

## Architecture

### 1. Why did you separate the Web, Application, and Database tiers?

Separating the tiers limits how much damage a single compromised or misconfigured resource can do. If the web tier were compromised, an attacker would still have to get through the internal load balancer and the app tier's security group before ever reaching the database. Each tier only talks to the tier directly next to it (Web → App → DB), so there's no direct path from the internet to sensitive data.

### 2. Why is the Application Tier private?

The app tier's only job is to receive requests from the internal load balancer and talk to the database — it never needs to accept traffic directly from the internet. I removed its public IP entirely rather than just relying on security group rules, so there's no public address to even attempt to reach, even if a firewall rule were ever misconfigured. This is "defense in depth" — two independent controls instead of one.

### 3. Why is MySQL private?

The database holds the actual user and review data — it's the most sensitive part of the system. I set publicly_accessible = false on the RDS instance and configured its security group to only accept traffic on port 3306 from the app tier's security group, never from 0.0.0.0/0. I also gave the database's route table no route to the internet gateway or NAT gateway at all, so there's no outbound path either, not just no inbound one.

### 4. Why are multiple Availability Zones used?

An Availability Zone is basically an independent physical data center within a region. If I only used one, and that single AZ had an outage, the entire application would go down. By spreading subnets, compute, and the database across two AZs (us-east-1a and us-east-1b), the app can keep running even if one AZ fails.

### 5. What is the difference between Multi-AZ/high availability and a read replica?

Multi-AZ is about surviving failure: it keeps a synchronous standby copy of the database in a second AZ, and if the primary goes down, AWS automatically fails over to the standby. A read replica is about handling more traffic: it's an asynchronous copy you can send read-only queries to, so the primary isn't overloaded. I learned this distinction the hard way — I originally planned to deploy both, but hit a real AWS limitation where Secrets Manager–managed master credentials aren't compatible with read replicas on MySQL. I kept Multi-AZ (the availability guarantee) and removed the replica, documenting the tradeoff rather than weakening the credential security to keep it.

## Terraform

### 6. How did you divide your Terraform into modules?

I used five modules, one per architectural concern: network (VPC, subnets, routing, NAT), security (security groups and their rules), load-balancer (public and internal ALBs, listeners, target groups), compute (the EC2 instances for the web and app tiers), and database (RDS, subnet group, credentials). Each module only knows about its own resources — it doesn't reach into another module's files directly.

### 7. How do the modules communicate through variables and outputs?

Each module exposes only the specific values other modules actually need as outputs — for example, the network module outputs subnet IDs and the VPC ID, and the security module outputs security group IDs. The root main.tf then passes those outputs into the next module as input variables — for instance, the compute module receives web_subnet_ids and web_security_group_id from the network and security modules rather than redefining or guessing them. This kept every module's real, current state as the single source of truth instead of having multiple modules independently reference the same resource.

### 8. What did you specifically check in `terraform plan`?

On every phase, I checked for: any 0.0.0.0/0 rule outside the two intended public ALB rules (port 80/443), publicly_accessible settings on the database, map_public_ip_on_launch matching the correct tier (true only for web subnets), and whether any plan showed an unexpected destroy or replace on resources that were already applied. I also specifically counted the number of resources being added against what I expected, so an unexpectedly large or small plan would stand out immediately.

## Agentic AI

### 9. What was the purpose of `CLAUDE.md`?

CLAUDE.md gave Claude Code persistent context and rules that it reads automatically at the start of every session, so I never had to re-explain the architecture, the required ports, or the safety rules (like never auto-approving apply or destroy) each time I opened a new session. It's the single place where the project's non-negotiable constraints live.

### 10. What work did the Terraform Engineer subagent perform?

This subagent did all of the hands-on Terraform work: designing the module structure, writing the actual .tf files for networking, security groups, load balancers, compute, and the database, running fmt/validate/plan, and researching current AWS provider syntax through the Terraform MCP server before writing resources it wasn't fully certain about (for example, confirming the newer aws_vpc_security_group_ingress_rule resource style instead of the older inline security group blocks).

### 11. What did the Architecture and Security Reviewer identify?

Across the phases, the reviewer caught a few real issues before I applied anything — most notably a count/index mismatch in the network module where subnet counts and Availability Zone counts came from two different variables, which would have caused a deployment failure if I'd ever changed the AZ count without also updating the subnet CIDR lists. It also flagged (WARN, not FAIL) things like the lack of enable_deletion_protection on the load balancers, which I accepted as a reasonable tradeoff for a capstone project rather than production.

### 12. Why did you use Terraform MCP instead of relying only on Claude's existing Terraform knowledge?

AWS provider syntax changes fairly often — arguments get renamed, deprecated, or replaced with newer resource types. Relying purely on Claude's training data risked generating Terraform based on a syntax that AWS had already changed. Terraform MCP let Claude look up the current, live provider documentation before writing resources, which is exactly why it caught things like the shift toward standalone aws_vpc_security_group_ingress_rule/egress_rule resources instead of the older inline security group block style.

### 13. What was the purpose of your validation hooks?

The hooks made terraform fmt run automatically every time a .tf file was edited, regardless of whether I remembered to run it myself. This is a deterministic check rather than something that depends on the AI (or me) remembering to do it — it guarantees consistent formatting on every change instead of being a "best effort" habit.

### 14. Describe one real issue Claude helped you troubleshoot.

When I tried to apply the database module, terraform apply failed with a real AWS error: FreeTierRestrictionError: The specified backup retention period exceeds the maximum available to free tier customers. I traced the value through two layers of indirection — the resource referenced a module-level variable, which itself was being overridden by a root-level variable with a default of 7 days. I found this by grepping for backup_retention_period across every file until I found where the actual value of 7 was coming from, changed the root variable's default to 1, and reconfirmed the fix by generating a fresh plan and checking the value directly with terraform show.

### 15. Describe one recommendation you reviewed, modified, or rejected instead of accepting blindly.

When manage_master_user_password (Secrets Manager–managed RDS credentials) turned out to be incompatible with MySQL read replicas, I had two options: switch to a traditional plaintext-adjacent password just to keep the replica, or drop the replica and keep the safer credential handling. I chose to keep manage_master_user_password and remove the replica, documenting the reasoning directly in the Terraform file as a comment rather than silently dropping it. I prioritized the stronger security posture over a "nice to have" architecture feature, which is exactly the kind of judgment call the assignment's "human remains responsible for every infrastructure decision" rule is meant to test.

---

# Task 10 — Publish the Mandatory LinkedIn Post

## Goal

Publish a LinkedIn post describing the capstone, the technical work completed, the Agentic AI workflow, and the lessons learned.

Write the post in your own words, include at least one project image or other proof, and ensure that it can be viewed by the submission reviewer.

## LinkedIn Post URL

**LinkedIn Post URL:** https://www.linkedin.com/posts/gideon-omole-5ba318180_aws-terraform-devops-activity-7504124155638845441-jL2t/?utm_source=share&utm_medium=member_desktop&rcm=ACoAACrC7l4BK-z0pGwSRQMO8ZJ5pFZyqybbIk4

---

# Submission Instructions

- Complete Tasks 0–10 in sequence.
- Include all Screenshots 1–28 exactly as specified.
- Ensure that your full name is visible in the required screenshots.
- Include the selected cloud platform.
- Include the completed architecture diagram.
- Include the modular Terraform project structure.
- Include the working public application URL or public load-balancer DNS.
- Include all required Agentic AI workflow evidence.
- Answer all 15 reflection questions briefly in your own words.
- Include the published LinkedIn post URL.
- Do not expose cloud credentials, database passwords, SSH private keys, JWT secrets, access tokens, account IDs, Terraform state containing sensitive values, or other confidential information.
- Review all screenshots and project files carefully before submitting through GitHub.

---

# Completion Checklist

- [ ] Selected AWS or Azure
- [ ] Added and reviewed the Agentic AI starter files
- [ ] Configured `CLAUDE.md`
- [ ] Configured the Terraform Engineer subagent
- [ ] Configured the Architecture and Security Reviewer subagent
- [ ] Connected Terraform MCP
- [ ] Configured validation hooks and safety guardrails
- [ ] Created the architecture diagram
- [ ] Created the six-subnet design
- [ ] Configured public Web Tier routing
- [ ] Kept the Application Tier private
- [ ] Kept the Database Tier private
- [ ] Configured tier-specific Security Groups or NSGs
- [ ] Restricted backend port `3001`
- [ ] Restricted MySQL port `3306` to the Application Tier
- [ ] Created the public load balancer
- [ ] Created the internal load balancer
- [ ] Configured listeners and health checks
- [ ] Deployed the Web Tier compute resources
- [ ] Deployed the private Application Tier compute resources
- [ ] Provisioned private managed MySQL
- [ ] Configured Multi-AZ or high availability
- [ ] Configured a read replica
- [ ] Created the modular Terraform project
- [ ] Used variables, outputs, and module dependencies
- [ ] Used current Terraform documentation through MCP
- [ ] Used hooks for deterministic validation
- [ ] Completed `terraform fmt`
- [ ] Completed `terraform validate`
- [ ] Reviewed `terraform plan`
- [ ] Completed the Terraform Engineer review
- [ ] Completed the Architecture and Security review
- [ ] Applied the infrastructure only after human approval
- [ ] Deployed and configured the backend
- [ ] Deployed and configured the frontend
- [ ] Configured Nginx where required
- [ ] Configured the internal backend endpoint
- [ ] Configured the public frontend endpoint
- [ ] Verified the homepage
- [ ] Verified login or authentication
- [ ] Verified book data
- [ ] Verified review functionality
- [ ] Verified the backend API
- [ ] Verified database reads and writes
- [ ] Verified healthy load-balancer targets
- [ ] Included AI-assisted Terraform generation evidence
- [ ] Included one architecture or security review
- [ ] Included one AI-assisted troubleshooting example
- [ ] Completed the final architecture review
- [ ] Answered all 15 reflection questions
- [ ] Published the mandatory LinkedIn post
- [ ] Added the LinkedIn post URL
- [ ] Captured all 28 required screenshots
- [ ] Confirmed that my full name is visible in the required screenshots
- [ ] Checked that no secrets or sensitive information are exposed

---

## About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory), focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations through hands-on experience.

---

## Resources

- Book Review App Repository: [https://github.com/pravinmishraaws/book-review-app](https://github.com/pravinmishraaws/book-review-app)
- DMI Official Website: [https://dmi.pravinmishra.com](https://dmi.pravinmishra.com)
- University: [https://university.pravinmishra.com](https://university.pravinmishra.com)
- Discord Community: [https://discord.pravinmishra.com](https://discord.pravinmishra.com)
- Blog: [https://dmi.pravinmishra.com/blog](https://dmi.pravinmishra.com/blog)
- YouTube Playlist: [https://www.youtube.com/playlist?list=PLFeSNDtI4Cho](https://www.youtube.com/playlist?list=PLFeSNDtI4Cho)
- Pravin Mishra on LinkedIn: [https://www.linkedin.com/in/pravin-mishra-aws-trainer/](https://www.linkedin.com/in/pravin-mishra-aws-trainer/)
- CloudAdvisory on LinkedIn: [https://www.linkedin.com/company/thecloudadvisory/](https://www.linkedin.com/company/thecloudadvisory/)

---

*This submission is part of the DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
