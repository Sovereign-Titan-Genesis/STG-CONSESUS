STG Rotation Policy Specification

Document Information

Field| Value
Document Name| rotation-policy.md
Repository| STG-CONSESUS
Category| STG-SPECS / CRYPTO
Version| V1-2026
Status| Draft Specification
Classification| Infrastructure Security
Scope| Validator Session Key Lifecycle

---

1. Purpose

Dokumen ini mendefinisikan kebijakan rotasi kunci sesi validator untuk ekosistem STG.

Tujuan utama:

- membatasi blast radius kompromi,
- menjaga consensus liveness,
- menjaga deterministic validator identity lifecycle,
- mendukung recovery governance,
- dan menyediakan audit-friendly event structure.

---

2. Threat Model

Dokumen ini dirancang untuk memitigasi ancaman berikut:

- validator key compromise,
- replay attack,
- stale session reuse,
- governance abuse,
- validator impersonation,
- network partition ambiguity,
- malicious revocation injection,
- long-term signing key persistence.

---

3. Validator Identity Hierarchy

Validator menggunakan tiga lapisan identitas:

Layer| Purpose
Validator Identity Key| Identitas jangka panjang validator
Session Signing Key| Kunci aktif untuk signing consensus
Recovery Authority Key| Digunakan untuk recovery & emergency governance

---

4. Rotation Objectives

Rotasi kunci bertujuan untuk:

- membatasi masa aktif kunci,
- mengurangi risiko kompromi jangka panjang,
- meningkatkan operational resilience,
- mendukung deterministic recovery process,
- memungkinkan hot-swap validator signing.

---

5. Rotation Cadence

Key Type| Recommended Rotation
Session Signing Key| 30–90 hari
Treasury Operational Key| 7–30 hari
Recovery Authority Key| Sangat jarang

---

6. Registry Event Schema

6.1 KeyAnnounced

Dipancarkan saat validator mengumumkan session key baru.

Parameters

Parameter| Type| Notes
validatorId| address indexed| Validator identity
sessionKeyHash| bytes32 indexed| Hash session key
effectiveEpoch| uint256| Aktivasi epoch
pqcMetadata| bytes| Future PQC metadata

---

6.2 KeyActivated

Dipancarkan saat session key mulai aktif.

Parameters

Parameter| Type
validatorId| address indexed
sessionKeyHash| bytes32 indexed
startBlock| uint256

---

6.3 KeyRevoked

Dipancarkan saat session key dicabut.

Parameters

Parameter| Type
validatorId| address indexed
sessionKeyHash| bytes32 indexed
reasonCode| uint8

---

6.4 EmergencyRotation

Dipancarkan saat emergency override dijalankan.

Parameters

Parameter| Type
validatorId| address indexed
initiatorMultiSig| address indexed
revokedKeyHash| bytes32
timestamp| uint256

---

7. Revocation Reason Codes

Code| Meaning
0| UNKNOWN
1| GRACE_PERIOD_EXPIRED
2| COMPROMISE_REPORTED
3| GOVERNANCE_OVERRIDE
4| DOUBLE_SIGNING
5| VALIDATOR_EXIT

---

8. State Machine

Validator Session Key Lifecycle

PENDING
    ↓
ANNOUNCED
    ↓
GRACE_OVERLAP
    ↓
ACTIVE
    ↓
REVOKE_PENDING
    ↓
REVOKED_FINAL

---

9. State Transition Rules

State| Old Key| New Key
ANNOUNCED| valid| pending
GRACE_OVERLAP| valid| valid
ACTIVE| revoked soon| active
REVOKED_FINAL| invalid| active

---

10. Grace Period Policy

Grace overlap digunakan untuk:

- mencegah propagation race,
- mengurangi downtime,
- menjaga synchronization safety.

Rules

- overlap berbasis epoch/block height,
- bukan timestamp,
- overlap duration ditentukan governance,
- hanya satu active key diperbolehkan setelah overlap selesai.

---

11. Transition Validation Logic

Deterministic Epoch Validation

if currentEpoch < announceEpoch:
    PENDING

else if currentEpoch < activationEpoch:
    ANNOUNCED

else if currentEpoch < activationEpoch + overlapWindow:
    GRACE_OVERLAP

else:
    REVOKED_FINAL

---

12. Replay Protection

Session key wajib:

- unik,
- validator-bound,
- epoch-bound,
- chain-bound.

Reference Binding

hash(
    validatorId,
    chainId,
    epoch,
    sessionKey
)

---

13. Revocation Flow

Compromise Detected
        ↓
Quorum Approval
        ↓
REVOKE_PENDING
        ↓
Registry Update
        ↓
Network Propagation
        ↓
REVOKED_FINAL
        ↓
New Session Key Activated

---

14. Emergency Override Governance

Emergency override wajib:

- multi-signature,
- quorum approved,
- time-locked,
- audit logged,
- rate-limited.

Emergency Cooldown

Rule| Value
Max Emergency Rotations| 1 / validator / 24h
Minimum Quorum| 2/3
Forced Audit Trigger| Required

---

15. Hardware Security Requirements

Validator infrastructure minimal mendukung:

- hardware wallet integration,
- HSM compatibility,
- offline recovery,
- air-gapped root authority,
- secure enclave support.

---

16. Audit Logging Requirements

Semua transisi state wajib:

- immutable,
- timestamped,
- explorer-visible,
- machine-parseable,
- indexer-compatible.

---

17. Post-Quantum Migration Path

Migration Phases

Phase| Description
PQC-0| Classical cryptography only
PQC-1| Hybrid dual-signature
PQC-2| PQC preferred
PQC-3| Classical deprecated

---

18. Implementation Notes

Dokumen ini adalah spesifikasi arsitektur dan belum menjadi implementasi final smart contract.

Seluruh implementasi wajib:

- diaudit,
- diuji terhadap replay scenarios,
- diuji terhadap network partition,
- dan diuji terhadap validator recovery flow.

---

19. Status

Draft specification aktif untuk:

- STG validator lifecycle,
- registry synchronization,
- sovereign consensus hardening,
- dan future validator governance.
