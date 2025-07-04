# HSRP – CCNP ENCOR Study Notes

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

## 🔹 Priority & Preemption

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

## 🔹 Timers

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

## 🔹 Virtual IP & MAC

* Hosts use **virtual IP** as their gateway
* Virtual MAC used by the active router

  * Format: `0000.0C07.ACxx` (v1)
  * Format: `0000.0C9F.Fxxx` (v2)

---

## 🔹 Interface Tracking

* Reduces router's priority if tracked interface goes down:

  ```
  standby 1 track GigabitEthernet0/2 20
  ```

---

## 🔹 Multicast Details

* HSRP Hellos use:

  * IP: `224.0.0.2`
  * Port: `UDP 1985`

---

## 🔹 HSRP vs VRRP vs GLBP

| Feature         | HSRP            | VRRP        | GLBP            |
| --------------- | --------------- | ----------- | --------------- |
| Vendor          | Cisco Only      | Open Std.   | Cisco Only      |
| Load Sharing    | ❌               | ❌           | ✅               |
| Preempt Default | ❌ (must enable) | ✅ (enabled) | ✅ (enabled)     |
| Election Basis  | Priority/IP     | Priority/IP | Priority/Weight |
