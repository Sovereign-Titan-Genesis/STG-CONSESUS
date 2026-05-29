# STG Block Production Engine Specification

## Document Information
- System: STG-CONSESUS
- Module: Block Production Engine
- Version: BLOCK-PRODUCTION-V1-2026
- Status: CORE BLOCK LIFECYCLE LAYER

---

# 1. Purpose

Block Production Engine bertugas untuk:

- membentuk blok dari transaksi
- memilih validator producer
- mengeksekusi transaksi melalui Execution Engine
- menghubungkan Consensus Core dengan State Transition
- menyebarkan blok ke seluruh jaringan (P2P)

---

# 2. Block Lifecycle Overview
---

# 3. Block Structure

Setiap block STG terdiri dari:

- block header
- transaction list
- state root
- receipt root
- validator signature

---

# 4. Block Header Specification

Block Header:

- previousBlockHash
- blockNumber
- timestamp (epoch-based)
- stateRoot
- transactionRoot
- receiptRoot
- validatorId
- consensusProof

---

# 5. Block Production Flow

## Step-by-step:

1. Validator dipilih oleh Consensus Core
2. Validator mengambil transaksi dari mempool
3. Transactions diurutkan berdasarkan:
   - gas price
   - nonce ordering
4. Execution Engine menjalankan transaksi
5. State root dihitung
6. Block dibentuk
7. Validator menandatangani block
8. Block disebarkan ke jaringan

---

# 6. Transaction Selection Rule

Mempool ordering:

Priority:

1. Higher gas fee
2. Valid nonce sequence
3. No state conflict
4. Earlier arrival (tie breaker)

---

# 7. Execution Binding

Block Production Engine wajib:

- memanggil Execution Engine
- memastikan deterministic result
- menyimpan state root final

Jika hasil berbeda antar node:
> block dianggap invalid

---

# 8. Consensus Binding

Block hanya valid jika:

- signed oleh validator leader
- ≥ 2/3 validator set dapat memverifikasi hash
- tidak ada fork conflict

---

# 9. P2P Broadcast Layer

Setelah block valid:

- block dikirim ke seluruh peer
- gossip protocol digunakan
- partial propagation diperbolehkan
- full sync dilakukan secara eventual

---

# 10. Fork Handling

Jika dua block muncul di slot yang sama:

Rule:

1. pilih block dengan validator score lebih tinggi
2. jika sama → pilih chain dengan cumulative difficulty lebih tinggi
3. jika masih sama → pilih chain dengan finality depth lebih dalam

---

# 11. Block Validation Rules

Node harus memverifikasi:

- signature valid
- transaction valid
- state root cocok
- execution deterministic
- no double spend

---

# 12. Finalization Pipeline

Block menjadi final jika:

- melewati consensus threshold
- tidak ada fork dalam window finality
- checkpoint disetujui validator supermajority

---

# 13. Mempool Management

Mempool:

- menyimpan transaksi pending
- membersihkan transaksi expired
- menghapus transaksi invalid nonce
- memprioritaskan fee market

---

# 14. Fee Distribution Model

Fee dari block:

- validator reward
- treasury allocation
- network sustainability fund

---

# 15. Security Constraints

Block production tidak boleh:

- mengubah consensus rules
- bypass execution engine
- memanipulasi state root
- menulis block tanpa signature

---

# 16. Anti-Attack Protections

Dilindungi dari:

- MEV manipulation (partial mitigation)
- spam transactions
- block stuffing
- validator collusion (via slashing system)

---

# 17. Reorg Handling

Jika reorg terjadi:

- rollback ke last final block
- re-execute transactions
- update state root

---

# 18. AI Monitoring Boundary

STG-AI hanya boleh:

- memonitor block health
- mendeteksi anomaly produksi block
- alert latency/failure

STG-AI tidak boleh:

- membuat block
- memilih validator
- mengubah mempool ordering

---

# 19. Cross-Layer Integration

Block Production Engine terhubung ke:

- Consensus Core
- Execution Engine
- P2P Network
- Storage State Layer
- Bridge Protocol Layer

---

# 20. Design Philosophy

> "Block bukan sekadar data — tapi unit realitas yang disepakati, dieksekusi, dan disebarkan."

---

# END OF SPEC
