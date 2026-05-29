# STG Consensus Core Specification

## Document Information
- System: STG-CONSESUS
- Module: Consensus Core
- Version: CONSENSUS-V1-2026
- Status: CORE PROTOCOL LAYER

---

# 1. Purpose

Consensus Core mendefinisikan:

- siapa yang berhak membuat blok
- bagaimana blok divalidasi
- bagaimana finality dicapai
- bagaimana fork diselesaikan
- bagaimana jaringan tetap hidup (liveness)

---

# 2. Consensus Model

STG menggunakan hybrid model:

- Proof of Stake (PoS)
- Validator Voting Consensus
- Finality Gadget (deterministic finality layer)

---

# 3. Validator Selection

Validator dipilih berdasarkan:

- stake weight
- uptime score
- slashing history
- network latency score

Formula konseptual:
---

# 4. Block Production Rule

Hanya 1 validator aktif per slot waktu:

- slot = fixed block interval
- leader dipilih deterministically
- fallback leader jika gagal produce block

---

# 5. Block Validation Pipeline

Setiap block harus melewati:

1. Signature validation
2. Transaction validity check
3. State transition validation
4. Gas/execution validation
5. Finality pre-check

---

# 6. Finality Model

Finality terjadi jika:

- ≥ 2/3 validator menyetujui block
- tidak ada conflicting fork dalam window
- checkpoint tercapai

Finality bersifat:

> irreversible after confirmation threshold

---

# 7. Fork Choice Rule

Jika terjadi fork:

Rule:

1. pilih chain dengan highest validator weight
2. jika sama → pilih chain dengan longest finality depth
3. jika masih sama → pilih chain dengan earliest timestamp

---

# 8. Slashing Conditions

Validator akan di-slash jika:

- double signing
- invalid block proposal
- downtime terlalu lama
- memanipulasi state transition

---

# 9. Liveness Guarantee

Jika ≥ 2/3 validator aktif:

- chain tetap berjalan
- block tetap diproduksi
- network tidak berhenti

---

# 10. Safety Guarantee

Jika tidak ada ≥ 2/3 agreement:

- block tidak difinalisasi
- state tidak berubah
- chain masuk mode pending

---

# 11. Epoch System

Consensus berjalan per epoch:

- epoch = batch waktu validator set
- validator set update di akhir epoch
- reward dihitung per epoch

---

# 12. Randomness Source

Randomness digunakan untuk:

- leader election
- validator rotation

Sumber:

- block hash entropy
- VRF (Verifiable Random Function)

---

# 13. Network Assumptions

Consensus mengasumsikan:

- partial synchrony
- honest majority ≥ 2/3 stake
- network delays are temporary

---

# 14. Attack Resistance

Tahan terhadap:

- Sybil attack
- long-range attack
- nothing-at-stake problem
- bribery attack (partial mitigation)

---

# 15. Finality Checkpoint System

Checkpoint:

- setiap N block
- disetujui validator supermajority
- digunakan untuk fast sync

---

# 16. AI Role Boundary

STG-AI hanya boleh:

- menganalisis consensus health
- mendeteksi anomaly
- memberikan alert

STG-AI tidak boleh:

- memilih validator
- mengubah fork choice
- mengubah finality rule

---

# 17. Cross-Chain Implication

Finality STG dapat:

- dipakai sebagai proof untuk bridge
- diverifikasi chain lain
- digunakan sebagai settlement layer

---

# 18. Failure Modes

Jika consensus gagal:

- chain pauses (safe mode)
- validator resync required
- checkpoint rollback

---

# 19. Philosophy

Consensus STG adalah:

> "mekanisme matematis untuk memastikan semua node melihat realitas yang sama tanpa saling percaya."

---

# END OF SPEC
