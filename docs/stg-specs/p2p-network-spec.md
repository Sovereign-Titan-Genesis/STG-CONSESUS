# STG P2P Network Specification (Core Network Layer)

## Document Information
- System: STG-CONSESUS
- Module: P2P Network Core
- Version: P2P-NETWORK-V1-2026
- Status: Core Infrastructure Layer

---

# 1. Purpose

Modul ini mendefinisikan seluruh sistem komunikasi node STG blockchain termasuk:

- Peer-to-peer discovery
- Gossip protocol
- Block propagation
- RPC gateway routing
- Validator communication
- Cross-chain relay interface

---

# 2. Network Philosophy

STG Network harus:

- Fully decentralized
- Fault tolerant
- Partition resistant
- Deterministic sync
- Low latency propagation
- Validator-consistent communication

---

# 3. Node Types

## 3.1 Full Node
- Menyimpan seluruh state
- Memvalidasi blok
- Melayani RPC request

## 3.2 Validator Node
- Menghasilkan blok
- Menandatangani consensus
- Menjalankan staking logic

## 3.3 Archive Node
- Menyimpan seluruh history blockchain
- Digunakan untuk audit & forensic

## 3.4 Light Node
- Hanya memverifikasi header
- Digunakan wallet & mobile apps

---

# 4. Peer Discovery System

Node menemukan peer melalui:

- Bootstrap nodes
- DHT (Distributed Hash Table)
- Manual seed list
- Gossip propagation discovery

Flow:

Client Node → Bootstrap → Peer Table → Network Mesh

---

# 5. Gossip Protocol

Gossip digunakan untuk:

- Block propagation
- Transaction broadcast
- Validator updates

Rules:

- Random peer selection
- TTL-limited propagation
- Duplicate suppression
- Signature verification mandatory

---

# 6. Block Propagation Flow
cat > docs/stg-specs/p2p-network-spec.md << 'EOF'
# STG P2P Network Specification (Core Network Layer)

## Document Information
- System: STG-CONSESUS
- Module: P2P Network Core
- Version: P2P-NETWORK-V1-2026
- Status: Core Infrastructure Layer

---

# 1. Purpose

Modul ini mendefinisikan seluruh sistem komunikasi node STG blockchain termasuk:

- Peer-to-peer discovery
- Gossip protocol
- Block propagation
- RPC gateway routing
- Validator communication
- Cross-chain relay interface

---

# 2. Network Philosophy

STG Network harus:

- Fully decentralized
- Fault tolerant
- Partition resistant
- Deterministic sync
- Low latency propagation
- Validator-consistent communication

---

# 3. Node Types

## 3.1 Full Node
- Menyimpan seluruh state
- Memvalidasi blok
- Melayani RPC request

## 3.2 Validator Node
- Menghasilkan blok
- Menandatangani consensus
- Menjalankan staking logic

## 3.3 Archive Node
- Menyimpan seluruh history blockchain
- Digunakan untuk audit & forensic

## 3.4 Light Node
- Hanya memverifikasi header
- Digunakan wallet & mobile apps

---

# 4. Peer Discovery System

Node menemukan peer melalui:

- Bootstrap nodes
- DHT (Distributed Hash Table)
- Manual seed list
- Gossip propagation discovery

Flow:

Client Node → Bootstrap → Peer Table → Network Mesh

---

# 5. Gossip Protocol

Gossip digunakan untuk:

- Block propagation
- Transaction broadcast
- Validator updates

Rules:

- Random peer selection
- TTL-limited propagation
- Duplicate suppression
- Signature verification mandatory

---

# 6. Block Propagation Flow
---

# 7. RPC Integration Layer

Network menyediakan RPC gateway:

- JSON-RPC (primary)
- WebSocket (real-time)
- Internal IPC (node-local)

RPC Gateway terhubung langsung ke P2P layer.

---

# 8. Node Synchronization

Sync mode:

## 8.1 Full Sync
- Download semua block

## 8.2 Fast Sync
- Download state snapshot

## 8.3 Light Sync
- Verify headers only

---

# 9. Fork Resolution

Jika terjadi fork:

1. Bandingkan chain weight
2. Bandingkan validator stake
3. Pilih chain dengan finality tertinggi
4. Reject orphan chain

---

# 10. Network Security Model

Proteksi:

- Sybil attack resistance
- Peer authentication
- Signature validation
- Rate limiting gossip spam
- Encrypted peer channels

---

# 11. Anti Split-Brain Mechanism

Jika network terpisah:

- validator checkpoint comparison
- finality threshold enforcement
- automatic reconciliation after reconnect

---

# 12. Cross-Chain Bridge Hook

P2P layer mendukung:

- External chain message relay
- Proof verification forwarding
- Bridge validator relay system

---

# 13. Latency Optimization

- Peer scoring system
- Fast path routing
- Regional clustering
- Adaptive gossip radius

---

# 14. Event Streaming

Network events:

- NewBlock
- NewTransaction
- ValidatorUpdate
- ForkDetected

---

# 15. AI Monitoring Boundary

STG-AI hanya boleh:

- membaca network metrics
- mendeteksi anomaly
- memberikan alert

STG-AI tidak boleh:

- mengubah routing
- mengubah consensus
- mengontrol peer selection

---

# 16. Failure Handling

Jika node gagal:

- auto peer replacement
- gossip reroute
- retry sync
- fallback bootstrap node

---

# 17. Network Topology

Model:

- Mesh network (primary)
- Cluster-based optimization
- Region-aware propagation

---

# 18. Upgrade Path

- QUIC transport layer
- libp2p integration
- quantum-safe encrypted channels
- decentralized relay routing

---

# 19. Security Guarantees

Network menjamin:

- message authenticity
- propagation integrity
- consensus alignment
- replay protection

---

# 20. Philosophy

P2P Network STG adalah:

> "Sistem saraf digital blockchain yang memastikan setiap node berpikir dan melihat dunia yang sama."

---

# END OF SPEC
