# 🔁 Task 4: East-West Traffic Inspection (Frontend ↔ Backend)

In this task, we configured FortiGate to inspect **internal (east-west) traffic** between `frontend-vm` and `backend-vm`, both located in different VPCs and connected via VPC peering.

---

## 🎯 Objective

Enable secure communication **between internal application tiers** by:

- Redirecting traffic through FortiGate (`port2`)
- Applying IPS and Antivirus
- Using **dynamic addresses** based on GCP instance metadata (tags)

---

## ⚙️ Configuration Steps

### 1. Create Dynamic Address for Frontend
![Frontend Tag Address](../screenshots/task4-gcp-frontend-address.png)

- Go to: `Policy & Objects > Addresses` → `Create New`
- Name: `gcp-frontend`
- Type: `Dynamic`
- SDN Connector: `gcp`
- Filter: `Tag=frontend`
- Click **OK**

### 2. Create Dynamic Address for Backend
![Backend Tag Address](../screenshots/task4-gcp-backend-address.png)
- Repeat as above
- Name: `gcp-backend`
- Filter: `Tag=backend`

### 3. Create East-West Firewall Policy
![East-West Policy](../screenshots/task4-east-west-policy.png)
- Go to: `Policy & Objects > Firewall Policy` → `Create New`
- Name: `frontend-to-backend`
- Incoming Interface: `port2`
- Outgoing Interface: `port2`
- Source: `gcp-frontend`
- Destination: `gcp-backend`
- Service: `HTTP`
- NAT: ❌ Disabled
- Security Profiles:
  - ✅ IPS
  - ✅ Antivirus
- Log Allowed Traffic: `All Sessions`
- Click **OK**

---

## 🔒 Why This Step Is Critical

- **Google Cloud VPC Peering is non-transitive**  
  So all traffic must be routed explicitly via FortiGate
- Dynamic addresses using **GCP Tags** avoid hardcoding IPs
- Traffic is inspected for threats between internal systems

---

## 🧪 Test Performed
![It Works Result](../screenshots/task4-it-works.png)
1. From browser, visit:  
   `http://<ELB_PUBLIC_IP>/`
2. Web application on `frontend-vm` contacts `backend-vm`
3. Backend responds → frontend returns:  
   ✔️ **"It works!"**

---

## 🧪 EICAR Threat Test
![EICAR Blocked Alert](../screenshots/task4-eicar-alert.png)
1. Clicked "Try getting EICAR" from frontend
2. FortiGate intercepted and **blocked the download**
3. Alert appeared in:  
   `Log & Report > Forward Traffic`
   - Threat Name: `EICAR_TEST_FILE`
   - Action: `Blocked`
   - Source: `10.0.0.2` → `10.0.1.2`

✅ This confirmed that **internal threat prevention is functioning correctly**.

---

## ✅ Summary

- Full internal inspection of east-west traffic established
- IPS and Antivirus successfully blocked a simulated threat
- Internal communication policies are now strictly controlled

