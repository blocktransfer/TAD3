# TIP-TBA: Federated co-transfer-agent and investor-compliance controls

- Status: Draft
- Type: Standards Track
- Created: 2026-07-22

## Abstract

This proposal defines a federated control model in which each legal issuer keeps a persistent Stellar issuance account and appoints one responsible transfer agent. Other TAD3 members may be appointed as co-transfer agents, compliance agents, or recovery agents for explicitly bounded purposes. It also defines a privacy-preserving decision record for internal investor-compliance workflows.

Signer authority is necessary but not sufficient authority to act. Every action also requires a current legal appointment, an in-scope protocol permission, and any approvals required by the issuer's control policy.

## Goals

- Preserve the asset identifier when an issuer changes transfer agents.
- Limit each co-agent to named issuers, actions, jurisdictions, and appointment dates.
- Keep one responsible transfer agent accountable for each issuer.
- Separate compliance review from ledger signing and submission.
- Exchange auditable compliance results without distributing investor PII.
- Avoid a global issuance account and its shared failure domain.

## Non-goals

This proposal does not create an SRO, decide a participant's regulatory status, provide a shared settlement or netting service, or authorize an agent beyond its written appointment. It does not standardize KYC vendors or prescribe the substantive law governing an investor.

## Account and authority model

Each security MUST be identified by its asset code and an issuer-specific Stellar account. An issuer SHOULD use a separate distribution account. A TAD3 operator MUST NOT place unrelated legal issuers under one common issuance account.

An issuer-control record conforming to [`issuer-control.schema.json`](../schemas/issuer-control.schema.json) identifies:

1. the issuer and its issuance and distribution accounts;
2. the responsible transfer agent;
3. every other appointed agent and the legal instrument supporting the appointment;
4. each agent's permitted actions, jurisdictions, and effective period; and
5. the on-chain signer and threshold configuration.

The roles are:

- `responsible_ta`: accountable for the issuer's transfer-agent service and routine ledger operations;
- `co_ta`: performs only the transfer-agent functions in its appointment and scope;
- `compliance_agent`: assesses investors or transfers but does not gain ledger authority merely from that assessment role; and
- `recovery_agent`: participates only in continuity, signer replacement, or emergency restoration.

A member MAY hold more than one role, but each role and scope MUST be recorded separately. An appointment MUST be disabled when it expires, is revoked, or no longer matches the member's registration or jurisdictional eligibility.

### Stellar threshold limitation

Stellar assigns operations to low, medium, or high threshold classes; it does not express legal concepts such as “U.S. investor approval” or “court-ordered transfer.” The TAD3 policy layer MUST therefore validate semantic approvals before constructing or signing a transaction. A signer MUST reject a proposal whose decision records, appointment scope, or required approvals are missing even if the collected key weights could satisfy the Stellar threshold.

## Internal investor-compliance workflow

The workflow separates evidence, decisions, transaction construction, and ledger execution:

```text
private evidence -> compliance review -> signed decision record
                                            |
                                            v
ledger state ----> policy validation ----> transaction proposal
                                            |
                                            v
                                  required agent signatures -> submit -> audit
```

1. **Collect.** An appointed agent collects KYC, sanctions, residency, accreditation, restriction, and other evidence in its private system.
2. **Assess.** The agent evaluates only the checks required for the requested action and jurisdiction.
3. **Decide.** The agent emits a record conforming to [`compliance-decision.schema.json`](../schemas/compliance-decision.schema.json). The record contains an opaque subject reference and evidence digests, not names, addresses, birth dates, government identifiers, or unredacted documents.
4. **Validate.** The responsible TA verifies the decision signature, freshness, issuer, asset, action, agent appointment, jurisdictional scope, and approval policy.
5. **Propose.** The responsible TA binds accepted decision IDs to an exact transaction or operation digest. A decision cannot be replayed for another issuer, asset, subject, or action.
6. **Sign and submit.** Only the signers required by the control policy approve the proposal. The submitting party records the ledger transaction hash and outcome.
7. **Reconcile.** Participating agents retain an append-only audit trail and reconcile it against the ledger and the issuer's master securityholder file.

## Decision semantics

A decision is one of:

- `approved`: the requested action may proceed until the stated expiry, subject to transaction policy;
- `rejected`: the requested action must not proceed;
- `needs_review`: no ledger action may proceed until a superseding decision is issued; or
- `suspended`: prior approval is no longer usable and any separate restriction workflow should begin.

Decisions are immutable. Corrections, renewals, and reversals MUST use a new decision with `supersedes_decision_id`. Systems MUST fail closed for absent, expired, malformed, unverifiable, out-of-scope, or conflicting decisions.

The shared `checks` list communicates categorical results such as `identity`, `sanctions`, `residency`, `accreditation`, `transfer_restriction`, or `ownership_limit`. It MUST NOT contain source documents or free-form PII. Evidence stays with the collecting agent under its retention and production obligations; `evidence_refs` contains opaque references and SHA-256 digests sufficient to detect later substitution.

## Action and approval policy

The issuer-control record lists scopes for each appointment. The following defaults are RECOMMENDED:

| Action | Compliance result | Additional approval |
| --- | --- | --- |
| Assess investor | In-scope compliance agent | None; no ledger authority results |
| Authorize or restore trustline | Current approval bound to subject, issuer, asset, and action | Responsible TA |
| Restrict trustline | Suspension, legal hold, or documented exception | Responsible TA; issuer notice according to procedure |
| Clawback or forced transfer | Documented legal basis | Responsible TA plus issuer approval |
| Add/remove signer or change thresholds | Current succession or governance instrument | High-threshold parties under the control policy |
| Activate recovery agent | Recorded continuity incident | Issuer plus the required independent parties |

The table is a protocol default, not a statement that an action is legally available. Issuer policy MAY require more approvals and MUST NOT require fewer than applicable law or the governing documents.

## Privacy and security requirements

- Public ledger data MUST use holder account addresses, not direct identifiers.
- Shared decision records MUST use issuer-scoped, non-guessable subject references. The same investor SHOULD receive different references for different issuers.
- Raw evidence and the mapping from subject reference to investor identity MUST remain encrypted off-chain with access logging and retention controls.
- Decision and control records MUST be digitally signed outside the JSON object or by a canonicalized-envelope mechanism selected by a later TIP.
- Keys used for compliance attestations SHOULD be distinct from Stellar transaction-signing keys.
- Systems MUST log appointment changes, decisions, proposal digests, signatures, submission results, overrides, and reconciliations.
- Emergency access MUST be time-limited, independently approved, and reviewed after use.

## Succession and revocation

The issuer account remains constant during a transfer-agent change. A succession change SHOULD:

1. suspend new proposals under the outgoing appointment;
2. reconcile the master file, pending decisions, and ledger state;
3. export records in the agreed format;
4. add or activate the successor under the existing high-threshold policy;
5. remove the outgoing signer and revoke its appointment; and
6. publish a new issuer-control record with a monotonically increased `revision`.

An agent's internal permission MUST be revoked before or atomically with removal of its ledger key. A key compromise triggers the inverse order only where delay would increase harm.

## Schemas and examples

The normative JSON Schemas are in [`schemas/`](../schemas). Example records in [`examples/`](../examples) illustrate a U.S. responsible TA and a German co-TA whose assessment authority is limited to Germany and the European Union.

## References

- [Stellar signatures and multisig](https://developers.stellar.org/docs/learn/fundamentals/transactions/signatures-multisig)
- [Stellar asset access controls](https://developers.stellar.org/docs/tokens/control-asset-access)
- [Stellar operation threshold reference](https://developers.stellar.org/docs/learn/fundamentals/transactions/list-of-operations)
