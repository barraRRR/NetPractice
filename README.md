*This project has been created as part of the 42 curriculum by jbarreir.*

<div align="center">

<img width="60%" alt="NetPractice" src="https://github.com/user-attachments/assets/62d256bc-4058-4ad6-8b76-51bfeb337ba9" />
   
### 🌐 *You won't exit the Matrix if you don't know the route* 🌐

![Topic](https://img.shields.io/badge/topic-networking-blue.svg)
![Protocol](https://img.shields.io/badge/protocol-TCP%2FIP-orange.svg)
![Levels](https://img.shields.io/badge/levels-10%2F10-brightgreen.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![42 School](https://img.shields.io/badge/school-42-black?logo=42&logoColor=white)

</div>

## Description

**NetPractice** is your first dive into **computer networking**. The goal of the project is to configure small, simulated networks so that every device can reach the destinations required by each level. No code is written: just octetcs and their decimal translation! Reason about **IP addresses**, **subnet masks**, **default gateways** and **routing tables**, and complete all the fields of the network diagram.

### Contents

- [Instructions](#instructions)
- [Submission details](#submission-details-42-school)
- [Method used to solve the levels](#method-used-to-solve-the-levels)
- [CIDR reference table](#cidr-reference-table)
- [Private and special address ranges](#private-and-special-address-ranges)
- [Project structure](#project-structure)
- [Resources](#resources)

---
<div align="center">

<img width="100%" alt="Gemini_Generated_Image_3wx8mu3wx8mu3wx8" src="https://github.com/user-attachments/assets/a87a7442-25c4-487e-bd4a-fd3758cd2b4c" />
</div>


## Instructions

### Running the training interface

1. Download the file attached to the project page and extract it in any folder.
2. From that folder, run the launcher script:

```bash
./run.sh
```

This starts a local web server and opens your preferred browser on the training page.

If `run.sh` does not work (browsers restrict how local pages can be loaded, so a local web server is required), start the server manually:

```bash
python3 -m http.server 49242
```

Then open `http://localhost:49242` in the browser. The port number can be changed.

### Using the interface

- Enter your **intranet login** in the *Training* tab. This is very important: the exported configuration is tied to it, and it is how the evaluation tool recognizes your own configuration. The *Evaluation* tab generates a random configuration, which is also what is used during defenses.
- Each level shows a non-functioning network diagram and one or more goals at the top of the window.
- Edit the unshaded fields (IP addresses, masks, gateways and routes) until the goals are met.
- Click **Check again** to verify the configuration. The logs at the bottom of the page explain where a packet is dropped.
- When the level is solved, a **Next level** button appears.

### Exporting configurations

Before moving to the next level, click **Get my config** to download the configuration of the current level. Do this for each of the 10 levels, because the exported files are what is evaluated in the 42 curriculum.

---

## Submission details (42 school)

- The repository must contain **10 exported configuration files (one per level)** placed at the **root of the repository**, next to this `README.md`.
- Every file must be exported with the login filled in on the interface.
- During the defense, **three random levels** must be completed successfully within a limited amount of time.

---

## Method used to solve the levels

1. **Mask and subnet**: a mask is a run of `1` bits followed by `0` bits. The number of `1` bits is the CIDR prefix (`255.255.255.0` = `/24`). It splits the IP into a network part and a host part.
2. **Usable range**: a `/n` subnet has `2^(32-n)` addresses. The first is the network address, the last is the broadcast address, and the `2^(32-n) - 2` in between go to hosts and router interfaces.
3. **Each link is its own subnet**: both ends must be in the same subnet, with different, valid IPs.
4. **Gateways and routes**:
   - A host's gateway must be a router interface inside the host's own subnet.
   - The default route `0.0.0.0/0` matches any destination, and its next hop must be directly reachable.
   - Routes are needed in both directions, so replies can return.
5. **Common mistakes**:
   - Using the network or broadcast address as a host IP.
   - Duplicating an IP within a subnet.
   - Masks that are too small or too wide, causing overlaps.
   - A gateway outside the device's subnet.
   - Overlapping subnets on the same router.
   - A missing return route.

---

## CIDR reference table

Useful values for the levels. The *block size* is the step between consecutive subnets in the octet that changes.

| CIDR | Subnet mask | Block size | Total addresses | Usable hosts |
|:---:|:---|:---:|---:|---:|
| `/8`  | `255.0.0.0`       | 1 (1st octet)   | 16,777,216 | 16,777,214 |
| `/9`  | `255.128.0.0`     | 128 (2nd octet) | 8,388,608  | 8,388,606 |
| `/10` | `255.192.0.0`     | 64 (2nd octet)  | 4,194,304  | 4,194,302 |
| `/11` | `255.224.0.0`     | 32 (2nd octet)  | 2,097,152  | 2,097,150 |
| `/12` | `255.240.0.0`     | 16 (2nd octet)  | 1,048,576  | 1,048,574 |
| `/13` | `255.248.0.0`     | 8 (2nd octet)   | 524,288    | 524,286 |
| `/14` | `255.252.0.0`     | 4 (2nd octet)   | 262,144    | 262,142 |
| `/15` | `255.254.0.0`     | 2 (2nd octet)   | 131,072    | 131,070 |
| `/16` | `255.255.0.0`     | 1 (2nd octet)   | 65,536     | 65,534 |
| `/17` | `255.255.128.0`   | 128 (3rd octet) | 32,768     | 32,766 |
| `/18` | `255.255.192.0`   | 64 (3rd octet)  | 16,384     | 16,382 |
| `/19` | `255.255.224.0`   | 32 (3rd octet)  | 8,192      | 8,190 |
| `/20` | `255.255.240.0`   | 16 (3rd octet)  | 4,096      | 4,094 |
| `/21` | `255.255.248.0`   | 8 (3rd octet)   | 2,048      | 2,046 |
| `/22` | `255.255.252.0`   | 4 (3rd octet)   | 1,024      | 1,022 |
| `/23` | `255.255.254.0`   | 2 (3rd octet)   | 512        | 510 |
| `/24` | `255.255.255.0`   | 1 (3rd octet)   | 256        | 254 |
| `/25` | `255.255.255.128` | 128 (4th octet) | 128        | 126 |
| `/26` | `255.255.255.192` | 64 (4th octet)  | 64         | 62 |
| `/27` | `255.255.255.224` | 32 (4th octet)  | 32         | 30 |
| `/28` | `255.255.255.240` | 16 (4th octet)  | 16         | 14 |
| `/29` | `255.255.255.248` | 8 (4th octet)   | 8          | 6 |
| `/30` | `255.255.255.252` | 4 (4th octet)   | 4          | 2 |
| `/31` | `255.255.255.254` | 2 (4th octet)   | 2          | 2 (point-to-point links, RFC 3021) |
| `/32` | `255.255.255.255` | 1 (4th octet)   | 1          | 1 (single host) |

### Worked example

Take `192.168.1.77/26`:

- Block size in the last octet: `64`, so the subnets are `.0`, `.64`, `.128` and `.192`.
- `77` falls inside `.64`, so the **network address** is `192.168.1.64`.
- The **broadcast address** is `192.168.1.127` (the next block minus one).
- Usable hosts: `192.168.1.65` to `192.168.1.126` (62 hosts).

### Powers of two and mask octets

| Mask bits set in the octet | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Octet value | 128 | 192 | 224 | 240 | 248 | 252 | 254 | 255 |

---

## Private and special address ranges

| Range | CIDR | Purpose |
|:---|:---:|:---|
| `10.0.0.0` – `10.255.255.255` | `10.0.0.0/8` | Private network (RFC 1918) |
| `172.16.0.0` – `172.31.255.255` | `172.16.0.0/12` | Private network (RFC 1918) |
| `192.168.0.0` – `192.168.255.255` | `192.168.0.0/16` | Private network (RFC 1918) |
| `127.0.0.0` – `127.255.255.255` | `127.0.0.0/8` | Loopback |
| `169.254.0.0` – `169.254.255.255` | `169.254.0.0/16` | Link-local (automatic addressing) |
| `0.0.0.0` | `0.0.0.0/0` | Default route / "any address" |

---

## Project structure

```text
.
├── README.md
└── <10 exported configuration files, one per level>
```

The training interface itself (`run.sh` and its web files) is provided by the project and is not part of the submission.

---

## Resources

### Networking concepts studied

#### TCP/IP addressing

An IPv4 address is a 32-bit number written as four octets (e.g. `192.168.1.10`). It identifies a network interface and has two parts: a **network part**, shared by all devices in the same network, and a **host part**, which identifies one device inside it. Addresses can be **public** (routable on the Internet) or **private** (reserved ranges such as `10.0.0.0/8`, `172.16.0.0/12` and `192.168.0.0/16`, only valid inside local networks). Two special values in every subnet cannot be assigned to a device: the **network address** (host bits all `0`) and the **broadcast address** (host bits all `1`).

#### Subnet masks and CIDR notation

A **subnet mask** tells where the network part of an address ends: every `1` bit marks the network part and every `0` bit marks the host part. **CIDR notation** writes the mask as a prefix length, so `255.255.255.0` becomes `/24`. Applying the mask to an address gives the network it belongs to, and the prefix determines how many hosts fit in it (`2^(32-n) - 2`). Choosing a longer or shorter prefix is how a network is split into subnets or merged into a larger one.

#### Default gateways

Two devices can talk directly only if they are in the same subnet. To reach any other subnet, a host sends its packets to the **default gateway**: the router interface that connects its network to the rest. For this to work, the gateway must belong to the host's own subnet.

#### Routers and switches

A **router** connects different networks. It has one interface per network and uses a **routing table** to decide where to forward each packet, falling back to the **default route** (`0.0.0.0/0`) when no more specific entry matches. A **switch** connects devices inside the same network by forwarding frames using MAC addresses, without changing the subnet.

#### OSI layers

The **OSI model** describes network communication in seven layers: physical, data link, network, transport, session, presentation and application. The **TCP/IP model** is a simplified version of it with four layers (link, internet, transport and application). Each layer provides services to the one above and relies on the one below. Switches work mainly at the data link layer, while IP addressing and routing belong to the network layer, which is where most of this project takes place.

### References

#### Videos

- [**Programación y más — *Aprende Todo sobre Direcciones IP 👽***](https://www.youtube.com/watch?v=UVRfI16jf8k): Spanish-language video introducing IP addresses, the starting point for understanding how devices are identified in a network.
- [**NetworkChunk — what is TCP/IP and OSI? // FREE CCNA**](https://www.youtube.com/watch?v=5WfiTHiU4x8): free CCNA course that introduces the TCP/IP and OSI models and how network communication is organized into layers.
- [**NetworkChunk — You SUCK at Subnetting // FREE COURSE // 9 episodes**](https://www.youtube.com/watch?v=5WfiTHiU4x8&list=PLIhvC56v63IKrRHh3gvZZBAGvsvOhwrRF): Course focused on subnetting. The mentor is remarkably engaging.

#### Articles and specifications

- [**IPv4 (Wikipedia)**](https://en.wikipedia.org/wiki/IPv4): overview of the fourth version of the Internet Protocol, covering the 32-bit address format, address classes, private and special-purpose ranges, and how packets are delivered.
- [**Classless Inter-Domain Routing (Wikipedia)**](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing): explains CIDR notation (`/n` prefixes), how it replaced the old class-based system, and how it allows flexible subnet sizes and route aggregation.
- [**Subnetwork (Wikipedia)**](https://en.wikipedia.org/wiki/Subnetwork): describes how a network is divided into subnets, how subnet masks separate the network and host parts of an address, and how network and broadcast addresses are determined.
- [**OSI model (Wikipedia)**](https://en.wikipedia.org/wiki/OSI_model): describes the seven-layer reference model for network communication and what each layer is responsible for.
- [**RFC 791 — Internet Protocol**](https://www.rfc-editor.org/rfc/rfc791): the original 1981 specification of IPv4, defining the addressing scheme, the original address classes and how datagrams are forwarded between networks.
- [**RFC 1918 — Address Allocation for Private Internets**](https://www.rfc-editor.org/rfc/rfc1918): reserves the private ranges `10.0.0.0/8`, `172.16.0.0/12` and `192.168.0.0/16` for internal networks, which are not routable on the public Internet.
- [**RFC 4632 — Classless Inter-domain Routing (CIDR)**](https://www.rfc-editor.org/rfc/rfc4632): explains why the class A/B/C system was replaced by classless prefixes (to slow routing-table growth and address exhaustion), defines the prefix notation, and includes a table of all prefix sizes with the number of addresses in each block.

### AI usage

AI was used as a learning and writing assistant. It helped with:

- Drafting and improving this README, including the structure of the sections and the CIDR reference table.

All level solutions were worked out and tested by the author in the training interface, and the content of this README was reviewed and understood by the author.
