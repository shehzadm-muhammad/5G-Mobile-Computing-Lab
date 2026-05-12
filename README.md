# 5G Mobile Computing Lab

This repository contains a portfolio summary of my Level 6 Mobile Computing practical project.

The project focused on the **configuration, diagnosis, and performance evaluation of a virtualised 5G standalone network** using **Free5GC**, **UERANSIM**, **VirtualBox**, **Ubuntu Linux**, **Wireshark**, **tcpdump**, **tc/netem**, and **iperf3**.

The aim was to understand how a 5G core network, simulated gNB, and simulated UEs communicate across both the **control plane** and **user plane**, and to test how network impairments affect throughput and application-level performance.

> This project was completed in a controlled university lab environment for educational purposes.

---

## Project Overview

The lab used a virtualised 5G standalone testbed to configure, run, validate, and evaluate a simulated 5G network.

The main goals were to:

- Set up a 5G core network using Free5GC
- Configure UERANSIM as the simulated gNB and UE environment
- Connect the simulated gNB to the Free5GC AMF
- Register two simulated UEs
- Establish PDU sessions
- Verify UE tunnel interface creation
- Validate user-plane connectivity
- Test baseline throughput using TCP, UDP, and SCTP
- Validate application-level traffic using YouTube streaming
- Apply network impairments using Linux NetEm
- Evaluate the effect of packet loss, delay, reordering, and duplication

---

## Lab Environment

| Component | Technology |
|---|---|
| Virtualisation | Oracle VirtualBox |
| Operating System | Ubuntu Linux |
| 5G Core | Free5GC |
| RAN/UE Simulator | UERANSIM |
| Packet Analysis | Wireshark, tcpdump |
| Performance Testing | iperf3 |
| Network Impairment | tc/netem |
| Network Testing | ping, routing checks, tunnel interface checks |
| Network Type | NAT + Host-only virtual networking |

---

## Network Architecture

The lab followed a two-VM topology.

```text
+-----------------------------+        Host-Only Network        +-----------------------------+
| Free5GC Core VM              | <----------------------------> | UERANSIM VM                  |
| Host-only IP: 192.168.56.103 |                              | Host-only IP: 192.168.56.104 |
|                              |                              |                             |
| AMF / SMF / UPF / NRF        |                              | Simulated gNB + UE1 + UE2    |
| AUSF / UDM / UDR / PCF       |                              | UE tunnel interfaces         |
| WebConsole                   |                              | uesimtun0 / uesimtun1        |
+-----------------------------+                              +-----------------------------+