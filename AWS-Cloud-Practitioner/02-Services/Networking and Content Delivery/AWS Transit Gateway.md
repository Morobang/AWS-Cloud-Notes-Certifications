# AWS Transit Gateway

## What you'll learn
- What Transit Gateway is and the problem it solves
- Hub-and-spoke network model
- Transit Gateway vs VPC Peering
- Key exam facts

---

## What is AWS Transit Gateway?

AWS Transit Gateway is a **network hub that connects multiple VPCs, on-premises networks (via VPN or Direct Connect), and AWS accounts through a single gateway**. It simplifies network topology by replacing complex VPC peering meshes with a centralized hub.

---

## The problem without Transit Gateway

In a VPC peering model, each VPC connects directly to every other VPC it needs to communicate with. With 5 VPCs, you need up to 10 peering connections. With 25 VPCs — 300 connections. This is a **full mesh** topology — complex to manage and doesn't scale.

**Transit Gateway** replaces this with a **hub-and-spoke model**: every VPC connects to the Transit Gateway hub; the hub routes traffic between them. Adding a new VPC requires only one connection to the hub.

---

## What Transit Gateway connects

- AWS VPCs (in the same account or cross-account)
- On-premises networks (via Site-to-Site VPN or Direct Connect Gateway)
- Other Transit Gateways (via peering — for inter-Region connectivity)

---

## Key features

**Centralized routing** — Configure route tables in Transit Gateway to control which VPCs can communicate with which. Segment your network (e.g., production VPCs can't communicate with development VPCs).

**Scalability** — Supports thousands of VPCs and connections.

**Cross-account and cross-Region** — Share a Transit Gateway across multiple AWS accounts using Resource Access Manager (RAM). Peer Transit Gateways across Regions.

**Multicast support** — Send data from one source to multiple destinations (specialized use cases like media streaming).

---

## Transit Gateway vs VPC Peering

| | Transit Gateway | VPC Peering |
|-|----------------|-------------|
| Topology | Hub-and-spoke | Point-to-point mesh |
| Scale | Thousands of VPCs | Complex above ~5 VPCs |
| Transitive routing | Yes | No (not transitive) |
| Cross-account | Yes (via RAM) | Yes |
| Cross-Region | Yes (via peering) | Yes |
| Cost | Per attachment + data transfer | No hourly cost, only data transfer |

**Transitive routing**: If VPC A peers with VPC B, and VPC B peers with VPC C, VPC A cannot reach VPC C through VPC B (no transitivity). With Transit Gateway, all attached VPCs can route to each other.

---

## When to use it

**Many VPCs to connect** — Replace a sprawling peering mesh with a clean hub-and-spoke model.

**Centralized on-premises connectivity** — Connect all VPCs to on-premises through one Transit Gateway instead of multiple VPNs.

**Network segmentation** — Use Transit Gateway route tables to control which VPCs can communicate with each other.

---

## Exam focus

- Transit Gateway = **centralized hub** connecting multiple VPCs, VPNs, and Direct Connect
- Replaces complex VPC peering mesh with **hub-and-spoke** topology
- **Transitive routing**: VPCs don't need direct peering — all traffic routes through the hub
- Use when exam describes: "connect many VPCs," "simplify network topology," "hub for VPCs and on-premises"

---

## Practice questions

**Q1.** A company has 20 VPCs across multiple AWS accounts that all need to communicate with each other and with an on-premises data center. Their current VPC peering configuration has become unmanageable. Which AWS service simplifies this into a hub-and-spoke architecture?

A) VPC Peering  
B) AWS Direct Connect  
C) AWS Transit Gateway  
D) AWS PrivateLink

**Answer: C** — Transit Gateway connects all VPCs and on-premises networks through a single hub, replacing the complex peering mesh. VPC Peering requires individual connections between each pair of VPCs — doesn't scale. Direct Connect is the private link to on-premises but doesn't connect VPCs to each other. PrivateLink provides private service endpoints, not general VPC-to-VPC connectivity.
