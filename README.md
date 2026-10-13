<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0F172A,50:0E7490,100:10B981&height=260&section=header&text=Smart%20Transit%20Network%20System&fontSize=44&fontColor=FFFFFF&fontAlignY=38&animation=fadeIn&desc=Cisco%20Packet%20Tracer%20%E2%80%A2%206%20sites%20%E2%80%A2%20VLSM%20%E2%80%A2%20RIP%20%2B%20Static%20Routing%20%E2%80%A2%20Network%20Services&descSize=16&descAlignY=58&descAlign=50" alt="Smart Transit Network System" width="100%"/>

<a href="https://github.com/sabikswanan/Smart-Transit-Network-System"><img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=2800&pause=1000&color=10B981&center=true&vCenter=true&width=780&lines=Router%23+show+ip+route;%3E+6+sites+%C2%B7+6+routers+%C2%B7+7+switches+%C2%B7+23+end+devices;%3E+VLSM+%C2%B7+RIP+%C2%B7+static+%26+floating+static+routes;%3E+DHCP+%C2%B7+DNS+%C2%B7+HTTP+%C2%B7+SMTP%2FPOP3+services" alt="6 sites, 6 routers, VLSM, RIP, static and floating static routes, DHCP, DNS, HTTP and email services"/></a>

<br/>

<img src="https://img.shields.io/badge/ROUTERS-6-0E7490?style=for-the-badge&labelColor=0B1220" alt="6 routers"/>
<img src="https://img.shields.io/badge/SWITCHES-7-0891B2?style=for-the-badge&labelColor=0B1220" alt="7 switches"/>
<img src="https://img.shields.io/badge/END%20DEVICES-23-06B6D4?style=for-the-badge&labelColor=0B1220" alt="23 end devices"/>
<img src="https://img.shields.io/badge/SUBNETS-12-10B981?style=for-the-badge&labelColor=0B1220" alt="12 subnets"/>

<img src="https://img.shields.io/badge/Cisco-Packet%20Tracer%208.2-049FD9?style=for-the-badge&logo=cisco&logoColor=white&labelColor=0B1220" alt="Cisco Packet Tracer 8.2"/>
<img src="https://img.shields.io/badge/Routing-RIP%20%2B%20Static-0E7490?style=for-the-badge&labelColor=0B1220" alt="RIP and static routing"/>
<img src="https://img.shields.io/badge/Addressing-VLSM-10B981?style=for-the-badge&labelColor=0B1220" alt="VLSM addressing"/>
<img src="https://img.shields.io/badge/CSE421-Computer%20Networks-334155?style=for-the-badge&labelColor=0B1220" alt="CSE421 Computer Networks"/>

<br/><br/>

**[Overview](#overview)** &nbsp;|&nbsp;
**[Topology](#topology)** &nbsp;|&nbsp;
**[Addressing](#addressing)** &nbsp;|&nbsp;
**[Routing](#routing)** &nbsp;|&nbsp;
**[Services](#services)** &nbsp;|&nbsp;
**[Getting Started](#getting-started)** &nbsp;|&nbsp;
**[Team](#team)**

</div>

<br/>

<a name="overview"></a>

## 🚇 Overview

A transit operator runs six sites that need to share data, email and a web portal reliably. This
project designs and simulates that network end to end in **Cisco Packet Tracer**: an efficient
**VLSM** addressing plan, a mix of **dynamic (RIP)** and **static routing** with a **floating static
backup path**, and the core **network services** every site depends on.

> [!NOTE]
> **Goal:** connect six transit sites so that every host can reach every other host and use shared
> DHCP, DNS, web and email services, with a backup route if the main path fails.

Built as a **Group 308** project for **CSE421 — Computer Networks** at **BRAC University**.

<br/>

<a name="topology"></a>

## 🗺️ Topology

```mermaid
flowchart LR
    CCH{{"<b>CCH</b><br/>LAN 17.17.0.0/24<br/>DNS · Email server"}}
    PSC{{"<b>PSC</b><br/>LAN 17.17.2.0/24<br/>Web · DHCP · Email server"}}
    DPC{{"<b>DPC</b><br/>LAN 17.17.1.0/24<br/>DHCP pool"}}
    FDU{{"<b>FDU</b><br/>LAN 17.17.3.0/25<br/>DHCP pool"}}
    DAU(["<b>DAU</b><br/>LAN 17.17.3.128/25"])
    RSP(["<b>RSP</b><br/>LAN 17.17.4.0/26"])

    CCH ---|"17.17.4.64/30<br/>Ethernet"| PSC
    CCH ---|"17.17.4.68/30<br/>Serial"| DPC
    DPC ---|"17.17.4.72/30<br/>Serial"| FDU
    FDU ---|"17.17.4.76/30<br/>Serial"| RSP
    PSC ---|"17.17.4.80/30<br/>Serial"| DAU
    DPC -.-|"17.17.4.84/30<br/>Backup serial"| PSC

    classDef rip fill:#0E7490,stroke:#22D3EE,stroke-width:2px,color:#FFFFFF
    classDef stub fill:#10B981,stroke:#047857,stroke-width:2px,color:#FFFFFF
    class CCH,PSC,DPC,FDU rip
    class DAU,RSP stub
    linkStyle 0,1,2,3,4 stroke:#06B6D4,stroke-width:2.5px
    linkStyle 5 stroke:#F59E0B,stroke-width:2px,stroke-dasharray:6 4
```

<sub>**Teal** sites run RIP · **green** sites are static-only stubs · the dashed amber DPC–PSC link is the floating static backup path. Every site LAN also has 2 PCs and a printer behind a Cisco 2960 switch.</sub>

<details>
<summary><b>Device inventory</b></summary>
<br/>

| Device | Model | Count |
|---|---|:---:|
| Router | Cisco 1841 | 6 |
| Switch | Cisco 2960-24TT | 7 |
| PC | PC-PT | 12 |
| Printer | Printer-PT | 6 |
| Server | Server-PT | 5 |
| Links | Copper Ethernet and serial | 36 |

</details>

<br/>

<a name="addressing"></a>

## 🧮 Addressing — VLSM

The whole design fits inside **17.17.0.0 – 17.17.4.87**. Subnets were allocated largest-first, so
each site gets exactly the block it needs and the six point-to-point links use tight **/30** subnets.

```mermaid
%%{init: {"themeVariables": {"pie1": "#0E7490", "pie2": "#06B6D4", "pie3": "#10B981", "pie4": "#334155", "pieStrokeColor": "#0B1220", "pieOuterStrokeColor": "#0B1220", "pieSectionTextColor": "#FFFFFF"}}}%%
pie showData
    title Address space by subnet size
    "/24 LANs ×3 (CCH, DPC, PSC)" : 768
    "/25 LANs ×2 (FDU, DAU)" : 256
    "/26 LAN ×1 (RSP)" : 64
    "/30 WAN links ×6" : 24
```

<table>
<tr>
<td valign="top">

**LAN subnets**

| Site | Subnet | Gateway | Hosts get IPs via |
|---|---|---|---|
| CCH | `17.17.0.0/24` | `.1` | Static |
| DPC | `17.17.1.0/24` | `.1` | Router DHCP pool |
| PSC | `17.17.2.0/24` | `.1` | DHCP server |
| FDU | `17.17.3.0/25` | `.1` | Router DHCP pool |
| DAU | `17.17.3.128/25` | `.129` | Static |
| RSP | `17.17.4.0/26` | `.1` | Static |

</td>
<td valign="top">

**Point-to-point links**

| Link | Subnet | Type |
|---|---|---|
| CCH ↔ PSC | `17.17.4.64/30` | Ethernet |
| CCH ↔ DPC | `17.17.4.68/30` | Serial |
| DPC ↔ FDU | `17.17.4.72/30` | Serial |
| FDU ↔ RSP | `17.17.4.76/30` | Serial |
| PSC ↔ DAU | `17.17.4.80/30` | Serial |
| DPC ↔ PSC | `17.17.4.84/30` | Serial (backup) |

</td>
</tr>
</table>

<br/>

<a name="routing"></a>

## 🔀 Routing

The network combines **RIP** in the core with **static routes** at the edges, then redistributes
the static routes into RIP so every router learns every subnet.

| Router | Role | Dynamic | Static configuration |
|---|---|:---:|---|
| **CCH** | Core hub | RIP | Routes to FDU, DAU and RSP LANs · **redistributes static into RIP** |
| **PSC** | Core · services site | RIP | Routes to DAU and RSP LANs · floating static to DPC LAN |
| **DPC** | Core | RIP | Route to RSP LAN · floating static to PSC LAN |
| **FDU** | Core | RIP | Route to RSP LAN |
| **DAU** | Stub site | — | 5 static routes via PSC |
| **RSP** | Stub site | — | Default route + static route via FDU |

### Failover with floating static routes

```mermaid
flowchart LR
    D["DPC LAN<br/>17.17.1.0/24"] -->|"Primary · RIP (AD 120)<br/>via CCH"| C{{"CCH"}}
    C --> P["PSC LAN<br/>17.17.2.0/24"]
    D -.->|"Backup · floating static (AD 130)<br/>direct serial link"| P

    classDef lan fill:#ECFEFF,stroke:#0891B2,color:#0B1220
    classDef core fill:#0E7490,stroke:#22D3EE,color:#FFFFFF
    class D,P lan
    class C core
    linkStyle 0,1 stroke:#10B981,stroke-width:2.5px
    linkStyle 2 stroke:#F59E0B,stroke-width:2px,stroke-dasharray:6 4
```

> [!TIP]
> RIP runs as **passive** on the direct DPC–PSC link, so traffic normally flows through CCH. The
> floating static routes on that link carry an **administrative distance of 130** — higher than
> RIP's 120 — so they only take over if the RIP path disappears.

<details>
<summary><b>Example: static routes on the DAU stub router</b></summary>
<br/>

```text
ip route 17.17.0.0   255.255.255.0   17.17.4.81
ip route 17.17.1.0   255.255.255.0   17.17.4.81
ip route 17.17.2.0   255.255.255.0   17.17.4.81
ip route 17.17.3.0   255.255.255.128 17.17.4.81
ip route 17.17.4.0   255.255.255.192 17.17.4.81
```

</details>

<details>
<summary><b>Example: RIP with redistribution on CCH</b></summary>
<br/>

```text
router rip
 redistribute static metric 1
 network 17.0.0.0
```

</details>

<br/>

<a name="services"></a>

## 🛰️ Services

| Service | Host | Address | Details |
|---|---|---|---|
| **DNS** | DNS-CCH | `17.17.0.2` | A records for `cch.transitline.com`, `transitline.com` and `www.transitline.local` |
| **Web (HTTP)** | PSC-WEB | `17.17.2.3` | Transit web portal, reachable as `www.transitline.local` |
| **Email (SMTP/POP3)** | Email-CCH | `17.17.0.3` | Mail domain `cch.transitline.com` |
| **Email (SMTP/POP3)** | PSC-Email | `17.17.2.4` | Mail domain `transitline.com` with mailboxes for DAU, RSP, FDU, DPC and PSC |
| **DHCP** | PSC-DHCP | `17.17.2.2` | Pool for the PSC LAN, hands out gateway and DNS server |
| **DHCP** | DPC and FDU routers | — | Router-based pools with the first 10 addresses reserved |

### What happens when a PC at DPC opens the web portal

```mermaid
sequenceDiagram
    autonumber
    participant PC as PC (DPC LAN)
    participant R as DPC router
    participant DNS as DNS server (CCH)
    participant WEB as Web server (PSC)
    PC->>R: DHCP Discover
    R-->>PC: IP 17.17.1.x · gateway 17.17.1.1 · DNS 17.17.0.2
    PC->>DNS: Who is www.transitline.local?
    DNS-->>PC: 17.17.2.3
    PC->>WEB: HTTP GET /
    WEB-->>PC: "Welcome To Web Portal"
```

<br/>

<a name="getting-started"></a>

## 🚀 Getting Started

1. Install [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer) (**8.2** or newer).
2. Clone or download this repository.
3. Open `CSE421_project.pkt`.

```bash
git clone https://github.com/sabikswanan/Smart-Transit-Network-System.git
```

<details>
<summary><b>Things to try once it is open</b></summary>
<br/>

| Try this | Where | What it checks |
|---|---|---|
| `show ip route` | Any router → CLI | Connected, RIP (`R`) and static (`S`) routes in the table |
| `ipconfig` | A DPC, FDU or PSC PC → Command Prompt | The address handed out by DHCP |
| `ping 17.17.4.2` | Any PC → Command Prompt | End-to-end reachability to a PC in the RSP LAN |
| Open `www.transitline.local` | Any PC → Web Browser | DNS resolution and the HTTP web portal |
| Send an email | A PC → Email client | The SMTP/POP3 mail servers |
| Simulation mode | Bottom-right toggle | Watch packets travel hop by hop |

</details>

<br/>

<a name="team"></a>

## 👥 Team — Group 308

| Student ID | Responsibility |
|---|---|
| 24101007 | VLSM tree and special conditions |
| 24101234 | Dynamic routing |
| 24101656 | Static routing requirements and topology drawing |
| 23241049 | Services and router configuration |

<br/>

<div align="center">

**Sabik Swanan**<br/>
B.Sc. in Computer Science · BRAC University

<br/>

<a href="https://github.com/sabikswanan"><img src="https://img.shields.io/badge/GitHub-sabikswanan-0E7490?style=for-the-badge&logo=github&logoColor=white&labelColor=0B1220" alt="GitHub"/></a>
<a href="https://www.linkedin.com/in/sabikswanan"><img src="https://img.shields.io/badge/LinkedIn-sabikswanan-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white&labelColor=0B1220" alt="LinkedIn"/></a>
<a href="https://mail.google.com/mail/?view=cm&fs=1&to=sabikswanan@gmail.com"><img src="https://img.shields.io/badge/Email-sabikswanan%40gmail.com-10B981?style=for-the-badge&logo=gmail&logoColor=white&labelColor=0B1220" alt="Email"/></a>

<br/><br/>

<sub>⭐ Star this repo if you found it useful</sub>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:10B981,50:0E7490,100:0F172A&height=130&section=footer" alt="" width="100%"/>

</div>
