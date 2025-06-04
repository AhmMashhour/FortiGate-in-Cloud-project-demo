# 🔄 Traffic Flow Scenarios

This document illustrates how traffic flows through the FortiGate NGFW in three directions: **inbound**, **outbound**, and **east-west**.

---

## 🌐 Inbound Traffic Flow (Internet → Frontend VM)

[ Internet ]
↓
[ External Load Balancer (ELB) ]
↓ (port1)
[ FortiGate NGFW ]
↓ (VIP NAT: ELB_IP → 10.0.0.2)
[ frontend-vm ]


🛡️ FortiGate inspects the request using:
- Explicit inbound firewall policy (port1 ➝ port2)
- IPS and logging
- No NAT (real source IP is preserved)

---

## 🚀 Outbound Traffic Flow (Frontend/Backend VM → Internet)

[ frontend-vm / backend-vm ]
↓ (default route)
[ FortiGate NGFW (port2) ]
↓
[ FortiGate NGFW (port1) ]
↓
[ Internet ]


🛡️ FortiGate applies:
- Outbound policy (port2 ➝ port1)
- Application Control, SSL Inspection
- NAT is enabled

---

## 🔁 East-West Traffic Flow (Frontend ↔ Backend)

[ frontend-vm (10.0.0.2) ]
↓
[ FortiGate NGFW (port2) ]
↓
[ backend-vm (10.0.1.2) ]


🛡️ FortiGate applies:
- Dynamic source/destination addresses via GCP tags
- East-west policy (port2 ➝ port2)
- IPS, Antivirus (EICAR test triggered here)
- No NAT

---

Each flow is **fully inspected and logged** at the FortiGate, providing visibility and control over cloud 
