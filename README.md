# IBC Verification Proofs: luxa-1 Mainnet Interoperability

**Network:** `luxa-1` (Cosmos SDK v0.50, CometBFT, `ibc-go` transfer module)  
**Counterparties:** Osmosis (`osmosis-1`), Cosmos Hub (`cosmoshub-4`), Celestia (`celestia`), Noble (`noble-1`)  
**Timestamp Standard:** UTC  

---

## 1. Channels Topology

| `luxa-1` Channel | Counterparty Chain | Counterparty Channel | Port | Client Type | State |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `channel-1` | `osmosis-1` | `channel-111100` | `transfer` | `07-tendermint` | `STATE_OPEN` |
| `channel-2` | `cosmoshub-4` | `channel-1941` | `transfer` | `07-tendermint` | `STATE_OPEN` |
| `channel-3` | `celestia` | `channel-709` | `transfer` | `07-tendermint` | `STATE_OPEN` |
| `channel-4` | `noble-1` | `channel-640` | `transfer` | `07-tendermint` | `STATE_OPEN` |

> **Topology Note:** Both ends of each channel have been independently queried and verified. Each counterparty chain hosts an active, synchronized `07-tendermint` light client tracking `luxa-1` headers with zero frozen clients. *(Channel `channel-0` connects to `osmo-test-5` and is strictly dedicated to testnet validation).*

---

## 2. Inbound Transactions (Tokens Received on luxa-1)

All incoming IBC transfers completed successfully with canonical write-acknowledgement: `{"result":"AQ=="}`.

| Origin Chain | Asset | Raw Amount | Packet Seq | `luxa-1` Receive Tx Hash | Block Height | Timestamp (UTC) | IBC Denom Hash on `luxa-1` |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Osmosis** | OSMO | `10000 uosmo` | 2 | `55CBF48E767AA76E55D433B6DA098734EC11C5F976B391A0130BD3EEB38D3E3E` | 38253 | 2026-11-01 03:02:56 | `ibc/0471F1C4E7AFD3F07702BEF6DC365268D64570F7C1FDC98EA6098DD6DE59817B` |
| **Cosmos Hub** | ATOM | `1000 uatom` | 3 | `16ABE6B4200F98B747AA9B4EAF07D6D7531B4FD98CC126A0D1620130A9ED12F0` | 41235 | 2026-11-01 21:17:10 | `ibc/9117A26BA81E29FA4F78F57DC2BD90CD3D26848101BA880445F119B22A1E254E` |
| **Celestia** | TIA | `1000 utia` | 1 | `661BDA81B395E3B3151C90017AE7834D1EF8EA31532FBD6DCEA100B337A93EB6` | 41418 | 2026-11-01 21:38:46 | `ibc/BF28D9C17E0306B194D50F51C3B2590BEAD15E04E03ADD34C3A26E62D85C9676` |
| **Noble** | USDC | `1000 uusdc` | 3 | `59870C50EAA9D1EAE77E235CA34FB330113AE7D19E8C7B0C223C7B3F421ED64E` | 41776 | 2026-11-01 22:20:49 | `ibc/BFF0D3805B50D93E2FA5C0B2DDF7E0B30A631076CD80BC12A48C0E95404B4A41` |

---

## 3. Departure Transactions & Relay Latency

| Origin Chain | Origin Tx Hash | Origin Height | Departure (UTC) | Finalized on `luxa-1` (UTC) | Latency |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Osmosis** | `2B9170566E7622155050989877FBED2B4F134A08F2514C7D5CFB33D1EF232FCB` | 73,728,992 | 2026-11-01 03:02:01 | 2026-11-01 03:02:56 | ~55 s |
| **Cosmos Hub** | `C438DF6E15F81FBD0BDFE7D3F8C37A23F3A85E2B206869774A25C2F31772C1C9` | 33,683,242 | 2026-11-01 21:13:58 | 2026-11-01 21:17:10 | ~3 min 12 s |
| **Celestia** | `35D440289E05323C7076377B86861B6CF6BD3B5EF184F743125D73368AAB3D19` | — | 2026-11-01 21:36:24 | 2026-11-01 21:38:46 | ~2 min 22 s |
| **Noble** | `A230A41398DBE2C829ECAFDC79BDCA7266785363D8F7978A3D64003C9B9DD5DC` | — | 2026-11-01 22:19:30 | 2026-11-01 22:20:49 | ~1 min 20 s |

---

## 4. Token Unwinding Proof (Native LUXA Return)

* **Transaction Hash:** `C56A78A6AE2E573D2F7B581182A9C08F815AEBFD5CE45517051BF0B53490D783`
* **Network & Height:** `luxa-1`, Block `28619`
* **Timestamp:** `2026-10-26 22:01:23 UTC`
* **Unwound Asset:** `1 LUXA` (`1000000` base units)
* **Path Traversed:** Sent back from Noble via denom path `transfer/channel-640/uluxa`.
* **Verification Result:** The packet prefix matched the originating escrow channel (`channel-4`), successfully unlocking the escrowed base asset and crediting native `uluxa` with an acknowledged state.

---

## 5. Outbound Transactions (LUXA Sent from luxa-1)

| Destination Chain | Packet Sequence | `luxa-1` Outbound Tx Hash | `luxa-1` Height | Status |
| :--- | :--- | :--- | :--- | :--- |
| **Osmosis** | — | `592ADCA7E96C9844F5AA49B7CA47C20D6ED45A21F7483068C1BE3651AF77DBA9` | 21506 | Acknowledged |
| **Cosmos Hub** | 1 | `54526D54B111E96E0E794E3D56E9AA7A2338EB32CE0B8CAE7639A255F2B28258` | 21930 | Acknowledged |
| **Cosmos Hub** | 2 | `380A8ADB596272990949A337E096E0E11C314A567EC7001EEEC0A7A14CAFC619` | 21967 | Acknowledged |
| **Cosmos Hub** | 3 | `959CE8FF88257DFC8C7DED29D38331052C7002DE34090F19216E5A9D3FAD6F46` | 21972 | Acknowledged |
| **Celestia** | — | `F43B1F0FC1FE1FAA5A167D17F064926A02C9526FBBBDED67F925F068CB6E086E` | 24101 | Acknowledged |
| **Noble** | 1 | `038849F2D6D26167F46BDACCEE67F77D652D64AC2312E30BADBEE39F421EA8BC` | 28519 | Acknowledged |
| **Noble** | 2 | `48791E547C0F9FEC726D8F6BC2144AF386B312B9783B63A7F8674A44519E2553` | 28632 | Acknowledged |

> **Queue Health:** Zero packet commitments remain open on channels 1 through 4. All outbound sequence packets have been picked up, relayed, and acknowledged by target chains.

---

## 6. Deterministic On-Chain Verification Commands

Run the following commands using any configured `luxad` binary pointing to an active RPC endpoint:

```bash
# 1. Query active IBC channels and counterparty mapping
luxad q ibc channel channels -o json | jq -r '.channels[] | "\(.channel_id) <-> \(.counterparty.channel_id) [\(.state)]"'

# 2. Inspect IBC denom traces registered on luxa-1
luxad q ibc-transfer denom-traces -o json | jq -c '.denom_traces[]'

# 3. Query specific transaction receipts and write-acknowledgements
luxad q tx <TX_HASH> -o json | jq '.events[] | select(.type=="recv_packet" or .type=="write_acknowledgement")'

# 4. Verify IBC denom hash calculation (SHA-256 uppercase hex of path/base_denom)
# Example for Osmosis uosmo on channel-1:
printf %s "transfer/channel-1/uosmo" | sha256sum | cut -d' ' -f1 | tr a-f A-F
# Expected Result: 0471F1C4E7AFD3F07702BEF6DC365268D64570F7C1FDC98EA6098DD6DE59817B
