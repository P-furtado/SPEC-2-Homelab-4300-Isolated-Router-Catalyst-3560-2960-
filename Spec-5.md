# Spec-10 Homelab Lab Manual (With Diagrams)

## Figures
![Figure 1: Master Topology Diagram](diagrams/master-topology.png)  
![Figure 2: VLAN 10 (Management Network)](diagrams/vlan10.png)  
![Figure 3: VLAN 20 (Workstation Network)](diagrams/vlan20.png)  

---

## Step 1: Basic Device Configuration (Core Switch 3560)

### Paste-Ready Config
```bash
CORE-SW> enable
CORE-SW# configure terminal
CORE-SW(config)# hostname CORE-SW
! Step 1.1: Set device hostname for identification
CORE-SW(config)# enable secret class
! Step 1.2: Set encrypted enable password
CORE-SW(config)# ip domain-name homelab.local
! Step 1.3: Define domain for SSH
CORE-SW(config)# username admin privilege 15 secret cisco123
! Step 1.4: Create local user for SSH login
CORE-SW(config)# crypto key generate rsa
! Step 1.5: Generate RSA keys for SSH (1024 bits)
CORE-SW(config)# ip ssh version 2
! Step 1.6: Enforce SSH v2
CORE-SW(config)# line vty 0 4
CORE-SW(config-line)# login local
! Step 1.7: Use local user accounts for SSH login
CORE-SW(config-line)# transport input ssh
! Step 1.8: Restrict access to SSH only (no Telnet)
CORE-SW(config-line)# exit
CORE-SW(config)# line console 0
CORE-SW(config-line)# logging synchronous
! Step 1.9: Prevent console disruption
CORE-SW(config-line)# exec-timeout 10 0
! Step 1.10: Auto logout after 10 minutes
CORE-SW(config-line)# password consolepass
! Step 1.11: Set console password
CORE-SW(config-line)# login
CORE-SW(config-line)# exit
CORE-SW(config)# exit
```

### Console Transcript
```bash
Switch> enable
Switch# configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Switch(config)# hostname CORE-SW
CORE-SW(config)# enable secret class
CORE-SW(config)# ip domain-name homelab.local
CORE-SW(config)# username admin privilege 15 secret cisco123
CORE-SW(config)# crypto key generate rsa
How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...
[OK] (elapsed time was 1 seconds)
CORE-SW(config)# ip ssh version 2
CORE-SW(config)# line vty 0 4
CORE-SW(config-line)# login local
CORE-SW(config-line)# transport input ssh
CORE-SW(config-line)# exit
CORE-SW(config)# line console 0
CORE-SW(config-line)# logging synchronous
CORE-SW(config-line)# exec-timeout 10 0
CORE-SW(config-line)# password consolepass
CORE-SW(config-line)# login
CORE-SW(config-line)# exit
CORE-SW(config)# exit
```

> **Explanation**  
> **What you’re doing:** Setting hostname, passwords, domain, SSH keys, and login methods.  
> **Why:** To make the switch identifiable, secure, and remotely manageable.  

---

## Step 2: Basic Device Configuration (Access Switch 2960)
(Commands and explanation similar to Step 1, tailored for ACCESS-SW.)

---

## Step 3: Basic Device Configuration (Router ISR4331)
(Configuration of hostname, SSH, and WAN/LAN interfaces.)

---

## Step 4: VLAN Creation (Core Switch)
```bash
CORE-SW(config)# vlan 10
! Step 4.1: Create VLAN 10 for management
CORE-SW(config-vlan)# name Management
CORE-SW(config)# vlan 20
! Step 4.2: Create VLAN 20 for workstations
CORE-SW(config-vlan)# name Workstations
```

> **Explanation**  
> **What you’re doing:** Creating VLANs 10 and 20 on the Core Switch.  
> **Why:** VLAN 10 isolates management traffic, VLAN 20 segments workstation traffic.  

---

## Step 5: Trunk Configuration (Core ↔ Access)
(Configuration of trunk ports on CORE-SW Gi0/2 and ACCESS-SW Gi0/1.)

---

## Step 6: Access Ports (Access Switch)
(Configuration of workstation-facing ports as VLAN 20 with PortFast enabled.)

---

## Step 7: Inter-VLAN Routing (Core Switch SVIs)
(Configuration of VLAN 10 and VLAN 20 SVIs with IPs, enabling `ip routing`.)

---

## Step 8: Optional DHCP (Core Switch)
(Configuration of DHCP pools for VLAN 20 if scaling beyond static IPs.)

---

## Step 9: WAN / NAT Setup (Router ISR4331)
(Configuration of WAN interface toward Spectrum ISP and NAT overload.)

---

## Step 10: Verification & Testing
```bash
CORE-SW# show vlan brief
ACCESS-SW# show interfaces trunk
CORE-SW# show ip route
PC> ping 192.168.20.2
PC> tracert 8.8.8.8
```
> **Explanation**  
> **What you’re doing:** Running verification commands.  
> **Why:** To ensure VLANs, trunks, routing, and connectivity are working.  

---

## Step 11: Implementation & Testing Sequence
1. (11.1) Cable ISR4331 WAN → Spectrum. Connect ISR LAN → 3560 Gi0/1. Connect 3560 Gi0/2 → 2960 Gi0/1.  
2. (11.2) Power on devices in this order: switches (3560 → 2960), then ISR, then PCs.  
3. (11.3) Configure management SVI and test SSH to 3560 and 2960.  
4. (11.4) Configure VLAN 20 access ports on 2960 and assign static IPs to PCs.  
5. (11.5) Test: ping between PCs, ping SVI, SSH to switches, test internet if NAT enabled.  

---

## Step 12: Saving & Recovery Tips
(Configuration save commands, password recovery notes.)

---

## Step 13: Quick-Start Template
(A condensed version of the most critical commands for fast rebuild.)

---
