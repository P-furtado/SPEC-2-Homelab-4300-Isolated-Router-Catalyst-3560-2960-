# SPEC-2 Homelab (Cisco ISR4331 + Catalyst 3560/2960)

## 🧠 Overview
The **SPEC-2 Homelab** is a fully configured small-scale enterprise-style network built for hands-on Cisco practice.  
It demonstrates routing, VLAN segmentation, inter-VLAN routing, SSH configuration, and verification workflows across multiple devices.  
This lab is text-only and designed to be followed step-by-step using the provided **PDF Lab Manual**.

---

## ⚙️ Equipment Used
- **Cisco ISR4331 Router** – WAN/LAN gateway  
- **Cisco Catalyst 3560** – Core Layer 3 switch  
- **Cisco Catalyst 2960** – Access Layer 2 switch  
- **2x PCs (Windows/Linux)** – End devices for testing  
- **Spectrum ISP modem** – Internet connection  
- **Cabling & Tools**
  - Cat5e/Cat6 patch cables (straight-through)
  - Cisco console cable (RJ45→DB9 rollover or USB)
  - USB→Serial adapter (if needed)
  - PuTTY for SSH/Console access

---

## 🧩 Network Topology (Text Overview)
- **VLAN 10 – Management:** 192.168.10.0/24 → Gateway: 192.168.10.1  
- **VLAN 20 – User Data:** 192.168.20.0/24 → Gateway: 192.168.20.1  
- **Router–Core Link:** 200.1.1.0/24  
- **WAN:** DHCP from ISP (Spectrum)

---

## 🔧 Features Implemented
- VLAN segmentation and trunking between Core and Access
- Inter-VLAN routing on Core switch
- SSH configuration on all Cisco devices
- Full CLI configuration with comments
- Verification commands and expected outputs
- PuTTY setup for both console and SSH access
- Troubleshooting guide for VLAN, ARP, and trunk mismatches
- Configuration save and restore steps (`do wr`, `copy run start`)

---

## 🧾 File Included
- **`SPEC-2_Homelab_CISCO_Text_Only.pdf`** – Full CLI-based lab manual (no diagrams)

---

## 🖥️ Usage Instructions
1. Open the PDF in any reader (Adobe Acrobat, Edge, etc.).
2. Follow the physical cabling section step-by-step.
3. Use PuTTY for console setup first, then SSH after configuration.
4. Run each CLI section in sequence (Router → Core → Access → PCs).
5. Use the verification section to validate connectivity and VLAN operation.

---

## 🧰 Skills Practiced
- Cisco IOS CLI configuration  
- VLAN trunking & SVI setup  
- SSH configuration and key generation  
- Basic IP routing & static routes  
- Troubleshooting (ARP, VLANs, trunk mismatch)  
- Network verification & diagnostics  

---

## 📂 Repository Structure
```
/SPEC-2-Homelab/
│
├── SPEC-2_Homelab_CISCO_Text_Only.pdf     # Full Lab Manual
└── README.md                              # Project Overview (this file)
```

---

## 🧩 Author
**Patrick Furtado**  
Hands-on networking enthusiast building Cisco-based home lab environments.
