# 📊 Logs Analysis – FortiGate Traffic & Threat Visibility

This document presents key observations and threat detections gathered from **FortiGate's traffic logs** during various stages of the lab.

---

## 🗂️ Log Source

All logs were viewed via the FortiGate web interface:

---

Log & Report > Forward Traffic


---

## 🔍 Outbound Traffic Log (Task 2)

- **Source IPs:** `10.0.0.2` (frontend), `10.0.1.2` (backend)
- **Destination:** Ubuntu update mirrors, DNS, time servers
- **Service:** HTTP, HTTPS, DNS
- **Action:** Allowed
- **Profiles Applied:**
  - Application Control (classified as "System Updates")
  - SSL Certificate Inspection

✅ This confirms outbound policies are functioning and logging all connections.

---

## 🔐 Inbound Access Log (Task 3)

- **Source:** Public IP (user browser)
- **Destination:** ELB IP (mapped via VIP to `10.0.0.2`)
- **Service:** HTTP
- **Action:** Allowed
- **Profiles Applied:** IPS, SSL Inspection
- **Result:** Response = `"It works!"` → frontend served the request

✅ Inbound access successfully passed through VIP and policy.

---

## 🔁 East-West Traffic Log (Task 4)

- **Source:** `frontend-vm` (10.0.0.2)
- **Destination:** `backend-vm` (10.0.1.2)
- **Service:** HTTP
- **Action:** Allowed
- **Profiles Applied:** IPS, Antivirus

✅ East-west policy matched and sessions inspected

---

## 🚨 Threat Detection – EICAR File Test

**Test Description:**  
The EICAR test file is a harmless, industry-standard file used to test antivirus software detection.

**Log Result:**
- **Source IP:** `10.0.0.2`
- **Destination IP:** `10.0.1.2`
- **Threat Name:** `EICAR_TEST_FILE`
- **Action:** `Blocked`
- **Severity:** `High`
- **Detection Method:** Antivirus engine
- **Policy Matched:** `frontend-to-backend`

✅ This confirms FortiGate AV is correctly identifying and stopping known test threats.

---

## 📈 Summary

| Type         | Source        | Destination     | Action   | Profile Matched   |
|--------------|---------------|------------------|----------|-------------------|
| Outbound     | 10.0.0.2/1.2  | Internet         | Allowed  | App Control, SSL  |
| Inbound      | Public IP     | 10.0.0.2 (via VIP) | Allowed | IPS, SSL          |
| East-West    | 10.0.0.2      | 10.0.1.2         | Allowed  | IPS, AV           |
| EICAR Test   | 10.0.0.2      | 10.0.1.2         | **Blocked** | AV               |

---

FortiGate logging enables full visibility into every flow, decision, and threat — a critical feature for cloud network security.
