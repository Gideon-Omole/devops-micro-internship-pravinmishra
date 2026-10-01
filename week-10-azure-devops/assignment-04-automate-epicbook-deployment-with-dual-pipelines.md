# Assignment 4 — Automate EpicBook Deployment with Dual Pipelines

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will automate the EpicBook infrastructure and application deployment on Microsoft Azure using two repositories and two Azure DevOps pipelines. Terraform will provision the infrastructure, and Ansible will configure the servers, connect EpicBook to Azure Database for MySQL, and deploy the application through Nginx.

---

# Task 0 — Verify Accounts, Tools, and Pipeline Capacity

## Goal

Confirm that the required accounts, tools, pipeline agent, SSH key pair, and Terraform remote-state location are ready.

No submission screenshot is required for this task.

---

# Task 1 — Prepare the Two Repositories

## Goal

Create separate Infrastructure and Application Repositories for the EpicBook deployment.

No submission screenshot is required for this task.

---

# Task 2 — Configure Azure DevOps Connections, Secure Files, and Secrets

## Goal

Configure controlled pipeline access to GitHub, Microsoft Azure, the virtual machines, and Azure Database for MySQL.

No submission screenshot is required for this task.

> Do not expose the Azure Client Secret, SSH private key, MySQL password, access token, subscription ID, or another sensitive value.

---

# Task 3 — Author the Terraform Infrastructure Configuration

## Goal

Define the complete EpicBook Azure infrastructure using Terraform and an Azure Storage remote backend.

No submission screenshot is required for this task.

---

# Task 4 — Author and Run the Infrastructure Pipeline

## Goal

Validate, plan, approve, and apply the Terraform configuration through the Infrastructure Pipeline.

## Evidence

### Screenshot 1 — Successful Infrastructure Pipeline

Add a screenshot of the Infrastructure Pipeline run showing:

* Successful `terraform apply` completion
* `app_public_ip`
* `backend_ansible_host`
* `backend_private_ip`
* `mysql_fqdn`

![screenshot-1](screenshots/gideon-omole-as4-scr1.png)

> Do not expose the MySQL password, Client Secret, Terraform state, SSH private key, or another sensitive value.

---

### Screenshot 2 — Provisioned Azure Resources

Add a screenshot of the Azure Portal Resource Group overview showing:

* Virtual Network and related networking resources
* Frontend VM
* Backend VM
* Azure Database for MySQL Flexible Server
* Related EpicBook resources

![screenshot-2](screenshots/gideon-omole-as4-scr2.png)
![screenshot-1](screenshots/gideon-omole-as4-scr2.1.png)
![screenshot-1](screenshots/gideon-omole-as4-scr2.2.png)

> Hide sensitive IDs, credentials, and database details.

---

# Task 5 — Complete the Manual Terraform-to-Ansible Handoff

## Goal

Transfer the required non-sensitive Terraform outputs to the Application Repository for use by Ansible.

No submission screenshot is required for this task.

---

# Task 6 — Author the Ansible Application Configuration

## Goal

Create idempotent Ansible automation that configures the frontend and backend VMs, prepares the database, configures Nginx, and deploys EpicBook.

No submission screenshot is required for this task.

---

# Task 7 — Author and Run the Application Pipeline

## Goal

Run the Application Pipeline to configure the VMs, deploy EpicBook, and verify the application environment.

## Evidence

### Screenshot 3 — Successful Application Pipeline

Add a screenshot of the Application Pipeline run summary showing all required stages or jobs succeeded.

![screenshot-3](screenshots/gideon-omole-as4-scr3.png)
![screenshot-3](screenshots/gideon-omole-as4-scr3.1.png)

---

### Screenshot 4 — Successful Ansible Play Recap

Add a screenshot of the Application Pipeline log showing:

* Ansible play recap
* Successful configuration or verification
* Zero failed hosts
* Zero unreachable hosts

![screenshot-4](screenshots/gideon-omole-as4-scr4.png)

> Do not expose the SSH private key, MySQL password, Client Secret, or complete database connection string.

---

# Task 8 — Verify the Complete EpicBook Workflow

## Goal

Verify that the frontend, backend, and Azure Database for MySQL work together correctly.

## Evidence

### Screenshot 5 — Running EpicBook Application

Add a browser screenshot showing:

* Running EpicBook application
* Frontend public IP address in the browser address bar
* Your Full Name
* Deployment date

The screenshot may show a product, cart, or successful order view.

![screenshot-5](screenshots/gideon-omole-as4-scr5.png)
![screenshot-5](screenshots/gideon-omole-as4-scr5.1.png)
![screenshot-5](screenshots/gideon-omole-as4-scr5.2.png)

> Do not expose credentials or sensitive information.

---

# Required URLs

## Frontend Application URL

[http://32.196.142.3/]

## Infrastructure Repository URL

[https://github.com/Gideon-Omole/infra-epicbook]

## Application Repository URL

[https://github.com/Gideon-Omole/theepicbook]

---

# Two-Repository Model

Write a short explanation of why separate Infrastructure and Application Repositories were used.

[I split the project into two separate repositories: infra-epicbook for Terraform infrastructure and theepicbook for application code and Ansible automation. I did this to follow a structure commonly used in real organizations, where infrastructure and application teams work separately.

Infrastructure changes and application changes can have different levels of risk and usually go through different review and release processes. For example, changing a security group or increasing the size of a database can affect the entire environment, so those changes should have their own review and approval process. A normal application code change does not usually need the same level of review.

Keeping the repositories separate also helps with security. Each pipeline only gets access to the resources and secrets it needs. The Infrastructure Pipeline has access to the credentials required to manage the infrastructure, while the Application Pipeline only needs the access required to deploy and configure the application. This reduces unnecessary access to sensitive information.

It also keeps the Git history easier to understand. Someone reviewing infra-epicbook can focus on infrastructure changes without having application features mixed into the same repository.]

---

# Manual Terraform-to-Ansible Handoff

Write a short explanation of how the following non-sensitive Terraform outputs were transferred to the Application Repository:

* `app_public_ip`
* `backend_ansible_host`
* `backend_private_ip`
* `mysql_fqdn`

[The Infrastructure Pipeline creates the AWS resources with Terraform. After `terraform apply` completes, it outputs four non-sensitive values: `app_public_ip`, `backend_ansible_host`, `backend_private_ip`, and `mysql_fqdn`.

I manually copied these values into the Application Repository. The IP addresses were added to the Ansible inventory, while the backend private IP and MySQL hostname were added to `group_vars`.

Sensitive information such as database passwords and AWS credentials remained in Azure DevOps secret variables and was not stored in either repository.

This manual handoff keeps the infrastructure and application repositories separate. It could later be automated using a secure shared location for these non-sensitive values.
]

---

# LinkedIn Requirement

## Evidence

### Screenshot 6 — LinkedIn Post

Add a screenshot of your LinkedIn post showing:

* Post text
* At least one image or link

![screenshot-6](screenshots/gideon-omole-as4-scr6.png)

## LinkedIn Post URL

[https://www.linkedin.com/posts/gideon-omole-5ba318180_azuredevops-terraform-ansible-ugcPost-7509582278201589760-vQcN/?utm_source=share&utm_medium=member_desktop&rcm=ACoAACrC7l4BK-z0pGwSRQMO8ZJ5pFZyqybbIk4]

Your post must include:

* What you automated
* Why separate Infrastructure and Application Repositories were used
* A brief explanation of the Terraform and Ansible pipelines
* How Azure credentials, the SSH key, and database secrets were protected
* One challenge you encountered
* How you solved the challenge
* Relevant technologies and skills

> Do not expose credentials, SSH keys, database passwords, subscription details, or other sensitive information.

---

# Submission Instructions

* Complete all tasks in sequence.
* Include your Full Name.
* Include a short explanation of the two-repository model.
* Include a short explanation of the manual Terraform-to-Ansible handoff.
* Include the Infrastructure Repository URL.
* Include the Application Repository URL.
* Include the final EpicBook application URL.
* Include Screenshots 1–6.
* Include the public LinkedIn post URL.
* Confirm that all screenshots are readable.
* Do not include Terraform state.
* Do not expose the Azure Client Secret, MySQL password, SSH private key, access token, complete connection string, subscription ID, tenant ID, account ID, or another sensitive value.
* Follow the Assignment Submission Guidelines.

---

# Completion Checklist

* [ ] Two separate repositories were created
* [ ] The Infrastructure Repository contains Terraform and its pipeline
* [ ] The Application Repository contains EpicBook, Ansible, and its pipeline
* [ ] Your Full Name and deployment date are visible in EpicBook
* [ ] Both pipelines use the intended `main` branch
* [ ] The Azure Resource Manager Service Connection works
* [ ] The Azure Client Secret is not stored in Git or YAML
* [ ] Terraform uses an Azure Storage remote backend
* [ ] Terraform state was not published or committed
* [ ] Separate frontend, backend, and database subnets were created
* [ ] The frontend VM accepts public HTTP traffic on port 80
* [ ] SSH access is restricted
* [ ] The backend application port is not publicly accessible
* [ ] Azure Database for MySQL uses private access
* [ ] The Infrastructure Pipeline validates, plans, applies, and displays non-sensitive outputs
* [ ] The reviewed Terraform plan was used during Apply
* [ ] Approval or manual validation occurred before Apply
* [ ] `app_public_ip` is available
* [ ] `backend_ansible_host` is available
* [ ] `backend_private_ip` is available
* [ ] `mysql_fqdn` is available
* [ ] Only non-sensitive Terraform outputs were transferred to the Application Repository
* [ ] The SSH private key is stored in Azure DevOps Secure Files
* [ ] The SSH private key was not committed or published
* [ ] The MySQL password is stored as a secret pipeline variable
* [ ] Ansible reaches both frontend and backend VMs
* [ ] Ansible completes with zero failed and zero unreachable hosts
* [ ] Nginx proxies requests to the backend private IP
* [ ] EpicBook runs as a persistent service
* [ ] The database schema and seed data are available
* [ ] The application displays database-backed products
* [ ] Cart or checkout actions are recorded in MySQL
* [ ] The final application displays your Full Name and deployment date
* [ ] Screenshots 1–6 are included and readable
* [ ] The Infrastructure Repository URL is included
* [ ] The Application Repository URL is included
* [ ] The final EpicBook application URL is included
* [ ] The LinkedIn post is published
* [ ] The LinkedIn post URL is included
* [ ] No secret or sensitive identifier is exposed

---

*This submission is part of the DevOps Micro Internship (DMI) — Agentic AI Track.*
