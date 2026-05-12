# Redacted Configuration Examples

This folder contains safe, redacted configuration examples from the 5G Mobile Computing lab.

The configuration files were used to align Free5GC and UERANSIM across the virtualised 5G standalone testbed. Sensitive values, subscriber identifiers, authentication keys, usernames, and environment-specific details have been removed or replaced with placeholders.

---

## Files

| File | Purpose |
|---|---|
| `amfcfg-redacted.yaml` | Redacted AMF configuration example |
| `smfcfg-redacted.yaml` | Redacted SMF configuration example |
| `upfcfg-redacted.yaml` | Redacted UPF configuration example |
| `ueransim-gnb-redacted.yaml` | Representative redacted UERANSIM gNB configuration example |
| `ueransim-ue-redacted.yaml` | Representative redacted UERANSIM UE configuration example |

---

## Redaction Notes

The AMF, SMF, and UPF examples are redacted versions of the lab configuration files.

The UERANSIM gNB and UE files are representative redacted examples based on the Free5GC + UERANSIM configuration pattern used in the lab.

Sensitive values have been replaced with placeholders.

---

## Example Placeholders

| Placeholder | Meaning |
|---|---|
| `<FREE5GC_CORE_IP>` | IP address of the Free5GC core VM |
| `<UERANSIM_VM_IP>` | IP address of the UERANSIM VM |
| `<AMF_IP>` | AMF interface address |
| `<UPF_IP>` | UPF interface address |
| `<SUBSCRIBER_SUPI>` | Redacted subscriber identity |
| `<SUBSCRIBER_KEY>` | Redacted subscriber authentication key |
| `<OPC_VALUE>` | Redacted operator code value |
| `<DNN>` | Data Network Name |
| `<SLICE_SST>` | Slice/service type value |
| `<SLICE_SD>` | Slice differentiator value |

---

## Safety

The original unredacted configuration files may contain lab-specific subscriber values or security details. Those values are not included in this public repository.