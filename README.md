# SPEC-2-Homelab-4300-Isolated-Router-Catalyst-3560-2960-
## README.md              
[📄 View the full Spec-10 Lab Manual](SPEC10-2025.md)


# SPEC-2025-LAB

[📄 View the PDF](./network-topology%20homelab-pdf.pdf)

##  PHYSICAL HOMELAB

<p align="center">
  <img src="https://i.postimg.cc/t4qvxQFG/lab-pic-rasberry-v1.jpg" alt="CCNA Two-Tier Home Lab Rack with Cisco 4331 Router, Catalyst 3560 Distribution Switch, and Catalyst 2960 Access Switch" width="650"/>
</p>




## VLAN Plan + Topology Diagrams (Master + per-VLAN)

### VLAN Plan

| VLAN ID | Name         | Purpose                  |
|---------|--------------|--------------------------|
| 10      | Management   | Switches, core mgmt      |
| 20      | Workstations | PCs, laptops             |

---

### Topology Diagrams (Master + per-VLAN)

```mermaid
graph TD
    ISP([🌐 Spectrum ISP\nWAN]) -->|WAN - DHCP| R

    R["📡 ISR4331\nGi0/0/0 WAN\nGi0/0/1 LAN\n192.168.30.1/30"]

    R -->|"P2P Link\n192.168.30.0/30\nGi0/0/1 ↔ Gi0/1"| DIST

    DIST["🔀 Core: Catalyst 3560-12PC-S\nSVI: 192.168.10.1 (VLAN10)\nSVI: 192.168.20.1 (VLAN20)\n192.168.30.2/30"]

    DIST -->|"Trunk (10,20)\nGi0/2 ↔ Gi0/1"| ACC

    ACC["🔌 Access: Catalyst 2960-24PC-L\nAccess ports: VLAN20"]

    ACC -->|VLAN20\nGi0/2| PC1
    ACC -->|VLAN20\nGi0/3| PC2

    PC1["🖥️ PC1 VLAN20\n192.168.20.10"]
    PC2["🖥️ PC2 VLAN20\n192.168.20.11"]

    style ISP fill:#d97706,color:#fff,stroke:#f59e0b
    style R fill:#0369a1,color:#fff,stroke:#38bdf8
    style DIST fill:#15803d,color:#fff,stroke:#4ade80
    style ACC fill:#a16207,color:#fff,stroke:#fbbf24
    style PC1 fill:#1d4ed8,color:#fff,stroke:#60a5fa
    style PC2 fill:#1d4ed8,color:#fff,stroke:#60a5fa
```

---

### VLAN 10 (Management) Topology

```mermaid
graph LR
    R10["📡 ISR4331\nGi0/0/1\n192.168.30.1/30"]
    DIST10["🔀 Core 3560\nSVI Vlan10\n192.168.10.1/24"]
    MGMT["💻 Management\nDevices / SSH"]

    R10 -->|"VLAN10 (routed via 3560)"| DIST10
    DIST10 -->|"VLAN10 access"| MGMT

    style R10 fill:#0369a1,color:#fff,stroke:#38bdf8
    style DIST10 fill:#15803d,color:#fff,stroke:#4ade80
    style MGMT fill:#6b21a8,color:#fff,stroke:#a78bfa
```

---

### VLAN 20 Topology

```mermaid
graph LR
    R20["📡 ISR4331\nGi0/0/1\n192.168.30.1/30\nDHCP Server"]
    DIST20["🔀 Core 3560\nSVI Vlan20\n192.168.20.1/24"]
    ACC20["🔌 Access 2960\nGi0/2 · Gi0/3"]
    PCA["🖥️ PC VLAN20\n192.168.20.10/11"]

    R20 -->|"VLAN20 (routed via 3560)"| DIST20
    DIST20 -->|"Trunk (10,20)"| ACC20
    ACC20 -->|"Access ports"| PCA

    style R20 fill:#0369a1,color:#fff,stroke:#38bdf8
    style DIST20 fill:#15803d,color:#fff,stroke:#4ade80
    style ACC20 fill:#a16207,color:#fff,stroke:#fbbf24
    style PCA fill:#1d4ed8,color:#fff,stroke:#60a5fa
```

---

### IP Routing Table (ISR4331)

```mermaid
graph LR
    subgraph ISR4331 ["📡 ISR4331 — Routing Table"]
        direction TB
        WAN["S* 0.0.0.0/0\n→ Gi0/0/0 (ISP)"]
        P2P["C  192.168.30.0/30\n→ Gi0/0/1 (connected)"]
        V10["S  192.168.10.0/24\n→ via 192.168.30.2"]
        V20["S  192.168.20.0/24\n→ via 192.168.30.2"]
    end

    subgraph DIST ["🔀 DIST-SW 3560 — RT"]
        direction TB
        DP2P["C  192.168.30.0/30\n→ Gi0/1 (connected)"]
        DV10["C  192.168.10.0/24\n→ Vlan10 (SVI)"]
        DV20["C  192.168.20.0/24\n→ Vlan20 (SVI)"]
        DDEF["S* 0.0.0.0/0\n→ via 192.168.30.1"]
    end

    style WAN fill:#7f1d1d,color:#fca5a5,stroke:#ef4444
    style P2P fill:#0c4a6e,color:#bae6fd,stroke:#38bdf8
    style V10 fill:#14532d,color:#bbf7d0,stroke:#4ade80
    style V20 fill:#713f12,color:#fde68a,stroke:#fbbf24
    style DP2P fill:#0c4a6e,color:#bae6fd,stroke:#38bdf8
    style DV10 fill:#14532d,color:#bbf7d0,stroke:#4ade80
    style DV20 fill:#713f12,color:#fde68a,stroke:#fbbf24
    style DDEF fill:#7f1d1d,color:#fca5a5,stroke:#ef4444
```




## IP Address & VLAN Plan

| SEGMENT | SUBNET | DEVICE / INTERFACE | ADDRESS | ROLE |
|---------|--------|--------------------|---------|------|
| WAN | DHCP | ISR4331 Gi0/0/0 | Dynamic (ISP) | Internet uplink |
| P2P Link | 192.168.30.0/30 | ISR4331 Gi0/0/1 | 192.168.30.1 | Router LAN port |
| P2P Link | 192.168.30.0/30 | DIST-SW Gi0/1 (routed) | 192.168.30.2 | 3560 uplink |
| VLAN 10 | 192.168.10.0/24 | DIST-SW Vlan10 SVI | 192.168.10.1 | Management |
| VLAN 20 | 192.168.20.0/24 | DIST-SW Vlan20 SVI | 192.168.20.1 | Workstations GW |
| VLAN 20 | 192.168.20.0/24 | PC1 (Win11) | 192.168.20.10 | Static / DHCP |
| VLAN 20 | 192.168.20.0/24 | PC2 (Raspberry Pi) | 192.168.20.11 | Static / DHCP |

---

   
