# Azure Multi-Site Network Monitoring Solution

### Overview
This project demonstrates the deployment of a centralized network monitoring application on an Azure Virtual Machine, moving a third-party service in-house to achieve significant cost savings and improve administrative control.

### The Problem
The company was paying a third-party provider R 1800 per month for a hosted network monitoring service. This was an unnecessary operational cost and, due to a lack of a proper handover, we had **no administrative access to the existing controller,** which created a significant migration challenge.

### The Solution
I leveraged existing Microsoft partner Azure credits to deploy a new monitoring server on an Azure VM. This move brought the hosting service in-house, eliminated the recurring third-party expense, and allowed for full administrative control of the system, despite the initial lack of access.

### Key Technologies Used
- Microsoft Azure Virtual Machines (e.g., B2ms with 2 vCPUs and 8GB RAM)
- Azure Networking (VNet, Subnets, Network Security Groups, Firewall Rules)
- Ubiquiti UniFi Network Controller
- PowerShell for VM configuration
- SSH and UniFi Layer 2 discovery
- Routing and Remote Access Service (RRAS)

### Implementation/Architecture
- Selected and provisioned a `B2ls v2` Azure Virtual Machine, chosen for its cost-effective performance and sufficient resources.
- **Justification for VM Choice:** The UniFi Network Controller's system requirements are modest, typically needing at least 2 GB of RAM. The `B2ls v2` VM, with its 2 vCPUs and 4 GB of RAM, provides a comfortable performance buffer for a small to medium-sized network while remaining a highly cost-effective "burstable" solution.
- Configured Azure networking and Network Security Group (NSG) rules to allow secure access.
- Installed and configured the UniFi Network Controller application.
- Deployed and configured the Routing and Remote Access Service (RRAS) on the Azure VM to provide secure remote access.
- Due to the lack of a proper handover, I had to manually adopt all existing UniFi equipment. I successfully used a combination of **SSH to restore devices to factory default** and the **UniFi Layer 2 discovery tool on a mobile device** to discover and adopt all hardware.
- The new controller was then configured as per all clients' existing configurations to ensure a seamless and transparent transition with no loss of service.
- Configured the controller to monitor and manage all remote sites.

### Secure Azure VM Architecture (Diagram)
The following diagram illustrates the secure remote access architecture implemented for this project. Clients connect via a VPN to the Azure VM, which acts as a secure jump box, allowing them to access resources via RDP.

              Internet
                 │
         [Client Device]
          ┌─────────────┐
          │ VPN Client  │
          │ (FortiClient│
          │ or Windows) │
          └─────┬───────┘
                │ VPN SSTP (443)
                │
                ▼
       ┌────────────────────┐
       │ Azure VM (Windows) │
       │  - RRAS VPN Server │
       │  - RDP Server      │
       │  - UniFi Controller│
       └─────┬──────────────┘
             │
      VPN Subnet: 20.20.1.0/24
             │
             ▼
         Private RDP IP
             │
             ▼
      Client Device connects to
      UniFi Controller via RDP
      or browser securely

### Outcome/Business Impact
Successfully deployed a resilient and scalable cloud-based monitoring solution while **reducing monthly operational costs by R 1800,** leveraging existing company resources to achieve the same functionality for free.
