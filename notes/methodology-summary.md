
---

# 2. Replace `notes/methodology-summary.md`

```markdown
# Methodology Summary

This file summarises the practical methodology followed in the 5G Mobile Computing lab.

The project used a virtualised 5G standalone testbed made from two Ubuntu virtual machines. One VM ran the Free5GC core network and the second VM ran UERANSIM to simulate the gNB and two UEs.

---

## 1. Virtual Lab Setup

The lab environment was created using Oracle VirtualBox.

Two Ubuntu virtual machines were used:

| VM | Purpose |
|---|---|
| Free5GC VM | Hosted the 5G core network |
| UERANSIM VM | Simulated the gNB and UEs |

Each VM used two network adapters:

| Adapter | Purpose |
|---|---|
| NAT | Internet access, package installation, and browser testing |
| Host-only Adapter | Internal communication between Free5GC and UERANSIM |

The final working Host-only addresses were:

| Component | Host-only IP |
|---|---|
| Free5GC VM | 192.168.56.103 |
| UERANSIM VM | 192.168.56.104 |

This structure kept the lab manageable because installation traffic and internal 5G testbed traffic were separated.

---

## 2. Free5GC Configuration

Free5GC was configured to provide the 5G core network.

The main network functions included:

- AMF
- SMF
- UPF
- NRF
- AUSF
- UDM
- UDR
- PCF
- NSSF
- WebConsole

The main configuration work focused on the AMF, SMF, and UPF files.

The externally reachable values needed by UERANSIM were configured using the Free5GC Host-only IP address. This included NGAP, PFCP, and GTP-related communication.

During the practical work, one issue was that moving too many internal service bindings to the Host-only IP caused binding and port conflicts. The final solution was to keep externally required NGAP, PFCP, and GTP-related values on the Host-only interface, while leaving internal SBI-related values on loopback where appropriate.

---

## 3. UERANSIM Configuration

UERANSIM was configured to simulate:

- gNB
- UE1
- UE2

The gNB configuration was aligned with the Free5GC AMF address.

In the final working setup:

| Item | Value |
|---|---|
| gNB local link IP | 192.168.56.104 |
| gNB NGAP IP | 192.168.56.104 |
| gNB GTP IP | 192.168.56.104 |
| AMF target IP | 192.168.56.103 |

UE configuration was aligned with the subscriber profiles created in the Free5GC WebConsole. UE1 was mainly used for throughput testing, while UE2 was used for application-level video streaming validation.

---

## 4. Control-Plane Validation

Control-plane validation focused on proving that the gNB and UEs could communicate successfully with the 5G core.

The main checks were:

- Free5GC core startup logs
- AMF availability
- PFCP association between SMF and UPF
- gNB connection to AMF
- UE registration
- PDU session establishment

Successful UE registration alone was not treated as complete proof that the full system worked. It only proved that the signalling side was working.

---

## 5. User-Plane Validation

User-plane validation focused on proving that traffic could pass through the UE tunnel path.

The main checks were:

- UE tunnel interface creation
- uesimtun0 / uesimtun1 interface checks
- route validation
- ping testing
- iperf3 throughput testing
- tcpdump packet verification
- YouTube playback through the UE tunnel path

This was important because a 5G system can show successful registration while still having user-plane routing or tunnel problems.

---

## 6. Packet Capture and Traffic Analysis

Wireshark and tcpdump were used to inspect the network behaviour.

The main protocol areas checked were:

| Protocol / Interface | Reason |
|---|---|
| NGAP / SCTP | gNB to AMF signalling |
| PFCP | SMF to UPF session control |
| GTP-U | User-plane tunnel traffic |
| UE tunnel traffic | Verification of simulated UE connectivity |

Packet capture evidence helped confirm that traffic was using the expected interfaces rather than only the VM management network.

---

## 7. Performance Testing

iperf3 was used to test throughput across the simulated 5G user-plane path.

The baseline tests included:

- TCP throughput
- UDP throughput
- SCTP throughput

Before testing, the active UE tunnel IP was checked because tunnel addresses could change after restarting the simulated UEs.

---

## 8. Application-Level Validation

UE2 was used for YouTube streaming validation.

The purpose was to test visible application behaviour, not just raw throughput. YouTube playback helped show how network conditions affected real user experience, such as:

- automatic quality reduction
- dropped frames
- lower connection speed
- weaker buffer behaviour
- possible playback instability

This was useful because iperf3 gave numerical performance results, while YouTube showed practical user-facing behaviour.

---

## 9. NetEm Impairment Testing

Linux NetEm was used to apply network impairments on the user-plane path.

The scenarios tested were:

| Scenario | Purpose |
|---|---|
| 30% packet loss | Test the effect of heavy packet loss |
| 150 ms delay | Test the effect of added latency |
| Packet reordering | Test TCP behaviour when packets arrive out of order |
| 15% duplication | Test the effect of duplicated packets |

For each scenario, the active qdisc rule was applied, checked, and then performance was tested using iperf3 and YouTube playback.

---

## 10. Evaluation

The results were evaluated by comparing:

- expected behaviour
- terminal logs
- tunnel interface state
- routing behaviour
- iperf3 throughput results
- packet capture evidence
- YouTube playback behaviour

The evaluation showed that packet loss and packet reordering caused the largest performance drop. Delay and duplication also affected the network, but the effect was smaller.

---

## Lessons Learned

A key lesson from the lab was that control-plane and user-plane validation must be checked separately. Successful UE registration does not automatically prove that user-plane traffic is working correctly.

Another lesson was that the live UE tunnel IP addresses changed after some restarts. Because of this, testing commands had to be checked against the active tunnel interface instead of relying on old IP addresses.

The experiment was limited because UERANSIM simulates the radio side in software. This means the results should be understood as protocol-level and system-level behaviour rather than real RF performance.

The virtualised setup may also be affected by host laptop resources, CPU scheduling, tunnel overhead, and interface timing. Even with these limitations, the lab provided a useful practical understanding of 5G architecture, configuration, diagnosis, and performance evaluation.