# Azure Load Balancer — High Availability Web Application

## Project Overview
Configured a highly available web application on Microsoft Azure using Azure Load Balancer to distribute traffic across multiple virtual machines. The solution included secure VM administration via Azure Bastion and automated deployment using Azure CLI.

---

## Architecture

```
                         Internet Traffic
                               │
                               ▼
                    ┌─────────────────────┐
                    │   Azure Load        │
                    │   Balancer          │
                    │   (Health Probes)   │
                    └────────┬────────────┘
                             │
               ┌─────────────┼─────────────┐
               │             │             │
               ▼             ▼             ▼
        ┌──────────┐  ┌──────────┐  ┌──────────┐
        │   VM 1   │  │   VM 2   │  │   VM 3   │
        │(Backend) │  │(Backend) │  │(Backend) │
        └──────────┘  └──────────┘  └──────────┘
               │
               ▼
        ┌──────────────────────┐
        │    Azure Bastion     │
        │  (Secure Admin Only) │
        └──────────────────────┘
```

---

## Technologies Used

| Technology | Purpose |
|------------|---------|
| Azure Virtual Machines | Backend application servers |
| Azure Load Balancer (Standard) | Traffic distribution across VMs |
| Health Probes | Detect and route away from unhealthy VMs |
| Azure Bastion | Secure VM administration |
| Azure Virtual Network | Network isolation |
| Azure CLI | Infrastructure deployment automation |

---

## Problem Statement
A client required a web application on Azure that could:
- Handle traffic across multiple servers without manual intervention
- Automatically detect and stop sending traffic to failed servers
- Allow secure VM administration without exposing management ports
- Be deployed consistently through automation rather than manual portal clicks

---

## Implementation Steps

### Step 1 — Virtual Network Setup
- Created Azure Virtual Network with appropriate subnet configuration
- Configured Network Security Groups to control inbound and outbound traffic

### Step 2 — Virtual Machine Deployment
- Deployed multiple Azure VMs using Azure CLI for automation
- Placed all VMs in the same backend pool for load balancing
- Configured VMs without public IP addresses for security

### Step 3 — Azure Load Balancer Configuration
- Created Standard Azure Load Balancer
- Configured backend pool with all VM instances
- Set up health probes to monitor VM availability
- Created load balancing rules to distribute incoming traffic
- Health probes ensure traffic only routes to healthy backend servers

### Step 4 — Azure Bastion Setup
- Deployed Azure Bastion in the Virtual Network
- Enabled secure RDP/SSH access through Azure portal
- Removed need for public IP addresses on VMs
- Reduced attack surface significantly

### Step 5 — Azure CLI Automation
- Automated all resource deployment using Azure CLI commands
- Scripted configuration to enable repeatable deployments
- Reduced manual effort and eliminated configuration inconsistencies

### Step 6 — Testing and Validation
- Tested load balancing by stopping one VM and confirming traffic rerouted
- Validated health probes correctly identified unhealthy instances
- Confirmed Bastion access functioning correctly

---

## Outcome
- ✅ High availability — traffic automatically reroutes if a VM becomes unhealthy
- ✅ Security hardened — no public RDP or SSH ports exposed on VMs
- ✅ Automated deployment — Azure CLI scripts enable consistent redeployment
- ✅ Scalable architecture — additional VMs can be added to backend pool easily

---

## Key Learnings
- Health probes are critical — without them load balancer cannot detect failed instances
- Azure Bastion removes the need for jump boxes or public IPs on VMs
- Azure CLI automation is faster and more reliable than manual portal configuration
- Standard Load Balancer required for zone-redundant high availability

---

## Azure Services Reference

| Service | Documentation |
|---------|--------------|
| Azure Load Balancer | https://learn.microsoft.com/en-us/azure/load-balancer/ |
| Azure Bastion | https://learn.microsoft.com/en-us/azure/bastion/ |
| Azure Virtual Network | https://learn.microsoft.com/en-us/azure/virtual-network/ |
| Azure CLI | https://learn.microsoft.com/en-us/cli/azure/ |
