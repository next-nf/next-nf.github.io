---
layout: default
title: "next-nf — open-source 3GPP mobile core in Erlang/OTP"
description: "Open-source 3GPP mobile-core network functions — SMF/PGW, HSS/UDR/UDM, PCF/PCRF, CHF/OCS/OFCS — built in Erlang/OTP, interoperable with Open5GS and srsRAN."
---

# next-nf

**Open-source 3GPP mobile-core network functions, built in Erlang/OTP.**
A growing set of 4G/EPC and 5GC network functions — resurrecting a forked
PGW/GGSN and growing it into a full SMF, surrounded by the policy, charging,
and subscriber-data functions a real mobile core needs.

Erlang was born at Ericsson to run telephone switches, so running modern 3GPP
network functions on it brings the workload home.

## Components

| Component | What it is | Key interfaces | Source |
|---|---|---|---|
| **smf** | GGSN / PDN-GW, evolving toward a full **SMF** | GTP-C/U, PFCP, Gx, Gy, RADIUS/Diameter Gi/SGi | [github.com/next-nf/smf](https://github.com/next-nf/smf) |
| **udr** | Converged **HSS + UDR/UDM** subscriber data | S6a Diameter, Nudr SBI, MILENAGE | [github.com/next-nf/udr](https://github.com/next-nf/udr) |
| **pcf** | **PCF + PCRF** policy control | Gx Diameter, Npcf SBI | [github.com/next-nf/pcf](https://github.com/next-nf/pcf) |
| **chf** | **CHF + OCS + OFCS** online/offline charging | Gy/Ro, Rf Diameter, Nchf SBI | [github.com/next-nf/chf](https://github.com/next-nf/chf) |
| **support-containers** | Reusable images (e.g. `srsran-4g`) for interop & demos | podman/buildah → GHCR | [github.com/next-nf/support-containers](https://github.com/next-nf/support-containers) |

## Toolbox

`Erlang/OTP` · `rebar3` · `DIAMETER` · `GTP-C/U` · `PFCP` · `3GPP SBI (HTTP/2)` ·
`MILENAGE` · `Mnesia` · `OpenTelemetry` · `podman` · `srsRAN` · `Open5GS`

## Contact

Considering commercial support — maintenance, integration, feature work — for
these components. If that's useful to you, get in touch: **next-nf@proton.me**

<footer>Putting modern 3GPP network functions back on the runtime that was designed for telecom in the first place.</footer>
