# 🛰️ Task 1: Static Routing Configuration

In this task, we configured **static routes** in FortiGate to allow it to reach workload VPCs (frontend and backend) through `port2`.

---

## 🎯 Objective

Enable FortiGate to reach internal virtual machines (`frontend-vm` and `backend-vm`) located in peered VPCs by defining static routing entries.

---

## ⚙️ Configuration Steps

1. Login to the **primary FortiGate GUI**
2. Navigate to:  
   `Network > Static Routes`
3. Click **Create New**
4. Enter the following:
   - **Destination:** `10.0.0.0/23`  
     (Aggregated subnet covering both frontend and backend VPCs)
   - **Gateway:** `172.20.1.1`  
     (Internal next hop towards the peered networks)
   - **Interface:** `port2`
5. Ignore the warning about gateway unreachability
6. Click **OK**

---

## 📌 Why This Step Was Critical

Without this route, FortiGate would not know how to reach the internal VMs, and any firewall policy allowing access would fail at the routing level.

This static route is the **foundation for enabling outbound and east-west traffic inspection** in later tasks.

---

## 🔍 Verification

After configuring the route, traffic originating from internal VMs (10.0.0.2, 10.0.1.2) was successfully intercepted and logged by FortiGate in Task 2 and Task 4.

