# IBC Verification: luxa-1 <-> Osmosis, Cosmos Hub, Celestia, Noble

**Chain**: `luxa-1` (Cosmos SDK v0.50, ibc-go transfer)  
**All times**: UTC  
*Everything below can be re-checked with the verification commands at the end of this document.*

---

## 1. Channels Topology

| `luxa-1` Side | Counterparty Chain | Counterparty Channel | State |
| :--- | :--- | :--- | :--- |
| `channel-1` | `osmosis-1` | `channel-111100` | `STATE_OPEN` |
| `channel-2` | `cosmoshub-4` | `channel-1941` | `STATE_OPEN` |
| `channel-3` | `celestia` | `channel-709` | `STATE_OPEN` |
| `channel-4` | `noble-1` | `channel-640` | `STATE_OPEN` |

> [!NOTE]
> Both ends were verified: the counterparty chain reports the matching channel, connection, and an active `07-tendermint` client tracking `luxa-1` (Active, not frozen).  
> *(An additional `channel-0` connects to `osmo-test-5` and is reserved for testing only).*

---

## 2. Inbound: Tokens Received on `luxa-1`
*Successful acknowledgement `{"result":"AQ=="}`*

| From | Token | Amount (raw) | Packet Seq | Tx on `luxa-1` | `luxa-1` Block | Time (UTC) | IBC Denom on `luxa-1` |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Osmosis** | OSMO | `10000 uosmo` | 2 | `55CBF48E767AA76E55D433B6DA098734EC11C5F976B391A0130BD3EEB38D3E3E` | 38253 | 2026-11-01 03:02:56 | `ibc/0471F1C4E7AFD3F07702BEF6DC365268D64570F7C1FDC98EA6098DD6DE59817B` |
| **Cosmos Hub** | ATOM | `1000 uatom` | 3 | `16ABE6B4200F98B747AA9B4EAF07D6D7531B4FD98CC126A0D1620130A9ED12F0` | 41235 | 2026-11-01 21:17:10 | `ibc/9117A26BA81E29FA4F78F57DC2BD90CD3D26848101BA880445F119B22A1E254E` |
| **Celestia** | TIA | `1000 utia` | 1 | `661BDA81B395E3B3151C90017AE7834D1EF8EA31532FBD6DCEA100B337A93EB6` | 41418 | 2026-11-01 21:38:46 | `ibc/BF28D9C17E0306B194D50F51C3B2590BEAD15E04E03ADD34C3A26E62D85C9676` |
| **Noble** | USDC | `1000 uusdc` | 3 | `59870C50EAA9D1EAE77E235CA34FB330113AE7D19E8C7B0C223C7B3F421ED64E` | 41776 | 2026-11-01 22:20:49 | `ibc/BFF0D3805B50D93E2FA5C0B2DDF7E0B30A631076CD80BC12A48C0E95404B4A41` |

---

## 3. Departure Transactions on Source Chains & Delivery Times

| From | Tx on Source Chain | Sent (UTC) | Received on `luxa-1` (UTC) | Delivery Time |
| :--- | :--- | :--- | :--- | :--- |
| **Osmosis** | `2B9170566E7622155050989877FBED2B4F134A08F2514C7D5CFB33D1EF232FCB`<br>*(osmosis-1, height 73,728,992)* | 2026-11-01 03:02:01 | 2026-11-01 03:02:56 | ~55 s |
| **Cosmos Hub** | `C438DF6E15F81FBD0BDFE7D3F8C37A23F3A85E2B206869774A25C2F31772C1C9`<br>*(cosmoshub-4, height 33,683,242)* | 2026-11-01 21:13:58 | 2026-11-01 21:17:10 | ~3 min 12 s |
| **Celestia** | `35D440289E05323C7076377B86861B6CF6BD3B5EF184F743125D73368AAB3D19` | 2026-11-01 21:36:24 | 2026-11-01 21:38:46 | ~2 min 22 s |
| **Noble** | `A230A41398DBE2C829ECAFDC79BDCA7266785363D8F7978A3D64003C9B9DD5DC` | 2026-11-01 22:19:30 | 2026-11-01 22:20:49 | ~1 min 20 s |

---

## 4. Native LUXA Returning to `luxa-1` (Unwinding Proof)

* On **2026-10-26 22:01:23 UTC** (`luxa-1` block **28619**, tx `C56A78A6AE2E573D2F7B581182A9C08F815AEBFD5CE45517051BF0B53490D783`), **1 LUXA** (`1000000`, denom `transfer/channel-640/uluxa`) returned from Noble.
* The prefix matches the originating channel, so the IBC packet was successfully unwound into native `uluxa` with positive acknowledgement.

---

## 5. Outbound: LUXA Sent from `luxa-1`

| Destination Chain | Tx on `luxa-1` | `luxa-1` Block |
| :--- | :--- | :--- |
| **Osmosis** | `592ADCA7E96C9844F5AA49B7CA47C20D6ED45A21F7483068C1BE3651AF77DBA9` | 21506 |
| **Cosmos Hub** (seq 1) | `54526D54B111E96E0E794E3D56E9AA7A2338EB32CE0B8CAE7639A255F2B28258` | 21930 |
| **Cosmos Hub** (seq 2) | `380A8ADB596272990949A337E096E0E11C314A567EC7001EEEC0A7A14CAFC619` | 21967 |
| **Cosmos Hub** (seq 3) | `959CE8FF88257DFC8C7DED29D38331052C7002DE34090F19216E5A9D3FAD6F46` | 21972 |
| **Celestia** | `F43B1F0FC1FE1FAA5A167D17F064926A02C9526FBBBDED67F925F068CB6E086E` | 24101 |
| **Noble** (seq 1) | `038849F2D6D26167F46BDACCEE67F77D652D64AC2312E30BADBEE39F421EA8BC` | 28519 |
| **Noble** (seq 2) | `48791E547C0F9FEC726D8F6BC2144AF386B312B9783B63A7F8674A44519E2553` | 28632 |

> [!NOTE]
> After the tests above, no packet commitment is left open on channels 1 to 4 (all outbound packets were acknowledged).

---

## 6. How to Re-Check On-Chain

```bash
# Query channels and clients
luxad q ibc channel channels -o json | jq -r '.channels[]|"\(.channel_id) <-> \(.counterparty.channel_id) \(.state)"'
luxad q ibc-transfer denom-traces -o json | jq -c '.denom_traces[]'

# Query a transaction and its acknowledgement
luxad q tx <TX_HASH> -o json | jq '.events[] | select(.type=="recv_packet" or .type=="write_acknowledgement")'

# Calculate expected IBC denom = sha256 of "transfer/<luxa-1 channel>/<base denom>" (uppercase hex)
printf %s "transfer/channel-1/uosmo" | sha256sum | cut -d' ' -f1 | tr a-f A-F
```

---

> [!IMPORTANT]
> *Test amounts are deliberately tiny. This document serves as verifiable cryptographic proof of IBC cross-chain connectivity, packet sequencing, and token unwinding across production mainnet relays.*
