# Performance Summary

This file summarises the performance testing and evaluation completed during the 5G Mobile Computing lab.

The testing focused on validating a virtualised 5G standalone environment using Free5GC, UERANSIM, iperf3, Wireshark, tcpdump, and Linux NetEm.

---

## Testing Focus

The practical testing focused on:

- Free5GC core startup
- gNB connection to the AMF
- UE registration success
- PDU session establishment
- Tunnel interface creation
- End-to-end user-plane connectivity
- TCP, UDP, and SCTP throughput testing
- YouTube streaming through a simulated UE tunnel
- Packet capture verification
- Control-plane and user-plane separation
- Network impairment testing using NetEm

---

## Tools Used

| Tool | Purpose |
|---|---|
| iperf3 | TCP, UDP, and SCTP throughput testing |
| ping | Basic connectivity testing |
| Wireshark | Packet inspection and protocol analysis |
| tcpdump | Command-line packet capture and tunnel traffic verification |
| tc/netem | Applying packet loss, delay, reordering, and duplication |
| Linux networking commands | Interface, route, and tunnel verification |

---

## Baseline Validation

Before impairment testing, the 5G testbed was validated under normal conditions.

The validation confirmed:

- Free5GC network functions started successfully
- SMF and UPF established PFCP association
- gNB connected to the AMF
- UE1 registered successfully
- UE2 registered successfully
- PDU sessions were established
- UE tunnel interfaces were created
- User-plane connectivity was available
- Traffic could be tested using iperf3
- YouTube playback could run through the UE2 tunnel

---

## Baseline Throughput Results

| Protocol | Sender Throughput | Receiver Throughput | Observation |
|---|---:|---:|---|
| TCP | 31.0 Mbit/s | 30.4 Mbit/s | Stable throughput with 116 retransmissions |
| UDP | 50.0 Mbit/s | 50.0 Mbit/s | Highest raw throughput, 0.379 ms jitter, very low packet loss |
| SCTP | 18.3 Mbit/s | 18.2 Mbit/s | Worked correctly but lower than TCP and UDP |

UDP achieved the highest raw throughput in the baseline test. TCP gave a balanced result for ordinary application traffic, while SCTP worked but produced lower throughput in this virtualised setup.

---

## Application-Level Validation

UE2 was used for YouTube playback testing through its UE tunnel interface.

This test was useful because it showed how the network behaved at application level, not only in raw throughput tests. Under baseline conditions, YouTube playback was successful and stable.

The video test helped observe:

- playback quality
- connection speed
- dropped frames
- buffer behaviour
- adaptive bitrate changes

This showed that available bandwidth alone does not fully explain user experience. Application-level behaviour can change even when basic connectivity is still working.

---

## NetEm Impairment Results

Linux NetEm was used to introduce network impairments on the user-plane path.

| Scenario | Throughput Result | Video Observation |
|---|---|---|
| Baseline TCP | 31.0 / 30.4 Mbit/s | Smooth playback |
| Baseline UDP | 50.0 Mbit/s, 0.379 ms jitter, very low loss | Stable playback |
| Baseline SCTP | 18.3 / 18.2 Mbit/s | Worked but lower throughput |
| 30% packet loss | 6.08 / 5.74 Mbit/s | Quality dropped to around 360p |
| 150 ms delay | 19.8 / 18.0 Mbit/s | Playback continued but responsiveness reduced |
| Packet reordering | 10.7 / 8.13 Mbit/s, 355 retransmissions | Unstable playback and lower quality |
| 15% duplication | 22.3 / 20.4 Mbit/s | Moderate degradation |

---

## Scenario Analysis

### 30% Packet Loss

Packet loss had the strongest negative effect on the network.

TCP throughput dropped from the baseline result to around 6 Mbit/s. YouTube playback continued, but the quality dropped heavily, reaching around 360p in the worst condition.

This showed that packet loss has a major effect on both throughput and user experience.

### 150 ms Delay

The delay scenario reduced TCP throughput but did not completely break the service.

YouTube playback continued, but responsiveness was weaker. This showed that delay affects user experience even when the connection remains usable.

### Packet Reordering

Packet reordering caused one of the biggest TCP performance drops.

The result showed reduced throughput and a high number of retransmissions. This made the connection less stable and affected video quality.

### 15% Packet Duplication

Packet duplication caused moderate degradation.

Throughput dropped compared with the baseline, but the effect was less severe than packet loss or reordering.

---

## Key Analysis

The results showed that not all network impairments affect performance in the same way.

The most damaging scenarios were:

1. 30% packet loss
2. Packet reordering

These caused the largest drops in throughput and the most visible reduction in video quality.

Delay and duplication also affected the network, but their impact was smaller. Delay mainly reduced responsiveness, while duplication reduced efficiency.

The YouTube test showed that application performance is not only about raw bandwidth. Under degraded conditions, adaptive streaming reduced video quality to keep playback running. This showed the link between network impairment and user experience.

---

## Evaluation Points

The results were evaluated using:

- Whether the UE could register successfully
- Whether the PDU session was established
- Whether the tunnel interface appeared correctly
- Whether user-plane traffic passed through the tunnel
- Whether Wireshark/tcpdump showed expected traffic behaviour
- Whether throughput results changed under different network conditions
- Whether YouTube playback quality changed under impairment

---

## Limitations

The lab was completed in a virtualised environment, so performance may have been affected by:

- host laptop resources
- VirtualBox overhead
- CPU scheduling
- virtual interface timing
- tunnel overhead
- simulated radio behaviour rather than real RF conditions

UERANSIM simulates the radio access side in software, so the results should be treated as protocol-level and system-level behaviour rather than full real-world mobile radio performance.

---

## Key Learning

The testing showed how a 5G network separates control-plane signalling from user-plane traffic.

A major lesson was that successful UE registration does not automatically prove that user-plane connectivity is working. Tunnel interfaces, routing, packet capture, and throughput testing were all needed to prove the full system worked.

The experiment also showed how packet loss, delay, reordering, and duplication affect throughput and video quality differently.