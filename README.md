# 🛡️ FortiGate in Cloud – Project Demo

This repository documents a complete cloud security lab that demonstrates how to deploy and configure a **FortiGate Next-Generation Firewall (NGFW)** in **Google Cloud Platform (GCP)** to inspect and control **inbound**, **outbound**, and **east-west** traffic between Virtual Private Clouds (VPCs).

---

## 📌 Overview

In modern cloud deployments, centralizing and enforcing network security policies is crucial. This project implements:

- A **hub-and-spoke VPC architecture**
- A highly available **FortiGate firewall cluster**
- Secure inspection of traffic flows using **IPS, Antivirus, SSL inspection**
- Fine-grained firewall policies for each traffic direction

---

## 📐 Architecture

![Architecture](architecture/task4-final-architecture.png)

**Key Components:**

- `frontend-vm` in its own VPC
- `backend-vm` in another VPC
- FortiGate cluster in hub VPC
- External Load Balancer (ELB) for inbound traffic
- FortiGate policies control:
  - Outbound internet access
  - Inbound access to frontend app
  - Internal (east-west) access between frontend and backend

---

## 🔧 Technologies Used

- **Google Cloud Platform (GCP)**
  - VPC Networks, Peering
  - Compute Engine
  - Load Balancing
- **Fortinet FortiGate NGFW**
  - High Availability (HA)
  - SDN Connector
  - IPS, Antivirus, SSL Inspection
- **Security Policies**
  - VIPs (Virtual IP)
  - Explicit firewall policies per traffic direction

---

## ✅ Tasks Completed

| Task | Description |
|------|-------------|
| **Task 1** | Configure static routes in FortiGate to reach workload VPCs |
| **Task 2** | Allow and inspect outbound traffic from VMs to Internet |
| **Task 3** | Redirect and inspect inbound traffic from Internet to frontend |
| **Task 4** | Secure and inspect east-west traffic between frontend and backend |

Screenshots and logs available in `/screenshots` and `/documentation`.

---

## 🧪 Security Tests

- ✔️ Verified HTTP access to frontend via ELB
- ✔️ Attempted to download `EICAR` test virus (blocked by FortiGate)
- ✔️ Logs recorded in FortiGate showing attack detection

---

## 📸 Sample Screenshots

| Event | Screenshot |
|-------|------------|
| Outbound logs | ![Outbound](screenshots/task2-logs.png) |
| VIP configuration | ![VIP](screenshots/task3-vip-policy.png) |
| Blocked EICAR alert | ![EICAR](screenshots/task4-eicar-alert.png) |
| Web response | ![Web](screenshots/task4-it-works.png) |

---

## 👨‍💻 What I Learned

- How to deploy and operate FortiGate NGFW in GCP
- Configuring traffic inspection using firewall policies
- Enforcing security between cloud application tiers
- Troubleshooting routing and NAT behavior in GCP
- Using SDN connectors for dynamic object-based firewall rules

---

## 📎 License

This project is for educational and demonstration purposes only.
