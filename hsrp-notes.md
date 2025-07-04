# HSRP, VRRP & GLBP – CCNP ENCOR Study Notes

## 🔹 What is HSRP?

* **Hot Standby Router Protocol**
* Cisco proprietary FHRP (First Hop Redundancy Protocol)
* Provides default gateway redundancy using **virtual IP and MAC**

---

## 🔹 HSRP Versions

| Feature     | HSRP v1                | HSRP v2          |
| ----------- | ---------------------- | ---------------- |
| IP Support  | IPv4 only              | IPv4 & IPv6      |
| Group Range | 0–255                  | 0–4095           |
| MAC Format  | `0000.0C07.ACxx`       | `0000.0C9F.Fxxx` |
| Multicast   | `224.0.0.2` (UDP 1985) | Same             |

---

## 🔹 HSRP Roles

* **Active**: Forwards traffic for virtual IP
* **Standby**: Takes over if Active fails
* **Listen**: Aware but not participating

---

## 🔹 HSRP Priority & Preemption

* Default priority: **100**
* Higher priority = more likely to become **Active**
* Priority 0 = **Disqualifies** router from Active role

### 📌 Preemption

* **NOT enabled by default**
* Must enable explicitly to allow higher priority routers to take over:

  ```
  standby 1 preempt
  ```

---

## 🔹 HSRP Timers

* **Hello**: Default 3 sec
* **Hold**: Default 10 sec
* Tunable:

  ```
  standby 1 timers 1 3
  ```

---

## 🔹 HSRP States

1. **Initial**
2. **Learn**
3. **Listen**
4. **Speak**
5. **Standby**
6. **Active**

> Most exam-relevant: `Listen`, `Speak`, `Standby`, `Active`

---

## 🔹 HSRP Virtual IP & MAC

* Hosts use **virtual IP** as their gateway
* Virtual MAC used by the active router

  * Format: `0000.0C07.ACxx` (v1)
  * Format: `0000.0C9F.Fxxx` (v2)

---

## 🔹 HSRP Interface Tracking

* Reduces router's priority if tracked interface goes down:

  ```
  standby 1 track GigabitEthernet0/2 20
  ```

---

## 🔹 HSRP Multicast Details

* HSRP Hellos use:

  * IP: `224.0.0.2`
  * Port: `UDP 1985`

---

## 🔹 What is VRRP?

* **Virtual Router Redundancy Protocol**
* **Open standard** FHRP (RFC 5798)
* Provides gateway redundancy via a virtual IP and MAC (similar to HSRP)
* Supported by most vendors

---

## 🔹 VRRP Basics

* One **Master** router (forwards traffic)
* One or more **Backup** routers
* All share a **virtual IP**

---

## 🔹 VRRP Priority & Election

* Priority range: 1–254 (default: **100**)
* Highest priority becomes **Master**
* Tie-breaker: Highest real IP address

### 📌 Preemption

* **Enabled by default** in VRRP

---

## 🔹 VRRP Timers

* Default advertisement interval: **1 second**
* Backup considers Master dead after **3 missed hellos** (default hold = 3 sec)
* Can be tuned:

  ```
  vrrp 1 timers advertise 1
  ```

---

## 🔹 VRRP Virtual MAC Address

* Format: `00:00:5E:00:01:<group>`

  * Example: `00:00:5E:00:01:14` for group 20

---

## 🔹 VRRP Multicast Details

* VRRP Hellos sent to:

  * IP: `224.0.0.18`
  * Protocol: **IP protocol 112** (not UDP or TCP)

---

## 🔹 What is GLBP?

* **Gateway Load Balancing Protocol**
* Cisco proprietary FHRP
* Provides default gateway redundancy **and** load balancing
* Clients use **one virtual IP**, but different routers reply with different MACs

---

## 🔹 GLBP Roles

* **Active Virtual Gateway (AVG)**: Assigns virtual MACs to group members
* **Active Virtual Forwarder (AVF)**: Forwards traffic using a virtual MAC
* Multiple AVFs = Load Balancing

---

## 🔹 GLBP Load Balancing Methods

* **Round-robin** (default)
* **Weighted**: Higher weight = more clients
* **Host-dependent**: MAC-based consistency

---

## 🔹 GLBP Configuration

* Uses `glbp <group>` commands
* Example:

  ```
  interface GigabitEthernet0/1
    ip address 192.168.30.2 255.255.255.0
    glbp 30 ip 192.168.30.1
    glbp 30 priority 120
    glbp 30 preempt
  ```

---

## 🔹 GLBP Virtual MAC Format

* Format: `0007.b4xx.xxyy`

  * `xx.xxyy` = GLBP group and AVF ID

---

## 🔹 GLBP Timers

* Default hello: 3 sec
* Default hold: 10 sec
* Configurable with:

  ```
  glbp 1 timers 1 3
  ```

---

## 🔹 GLBP Multicast Details

* Hellos sent to:

  * IP: `224.0.0.102`
  * UDP port: **3222**

---

## 🔹 Summary: HSRP vs VRRP vs GLBP

| Feature         | HSRP        | VRRP          | GLBP            |
| --------------- | ----------- | ------------- | --------------- |
| Vendor          | Cisco Only  | Open Standard | Cisco Only      |
| Load Balancing  | ❌           | ❌             | ✅               |
| Preempt Default | ❌ (manual)  | ✅             | ✅               |
| Election Basis  | Priority/IP | Priority/IP   | Priority/Weight |
| Virtual MACs    | 1 MAC total | 1 MAC total   | 1 per AVF       |
| Multicast IP    | 224.0.0.2   | 224.0.0.18    | 224.0.0.102     |
| Port / Protocol | UDP 1985    | Protocol 112  | UDP 3222        |
