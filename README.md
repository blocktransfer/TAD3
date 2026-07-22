# TAD3 protocol

TAD3 is a working area for standards for direct, on-chain securities records administered by transfer agents. The protocol is being developed here while BlockTransfer is the initial operator, with the intention of making responsibilities and interfaces portable among participating agents.

## Protocol proposals

- [TIP-TBA: Federated co-transfer-agent and investor-compliance controls](tips/TIP-TBA-co-ta-investor-compliance.md)

## Machine-readable artifacts

- [`issuer-control.schema.json`](schemas/issuer-control.schema.json) describes an issuer-specific account, appointed agents, signer policy, and bounded authority.
- [`compliance-decision.schema.json`](schemas/compliance-decision.schema.json) describes the minimum shared record for an investor-compliance decision without putting investor PII on-chain.
- [`examples/`](examples) contains conforming sample records.

These materials are technical and operational proposals. They do not determine whether a participant is acting as a transfer agent or satisfy any participant's legal, regulatory, or contractual obligations.
