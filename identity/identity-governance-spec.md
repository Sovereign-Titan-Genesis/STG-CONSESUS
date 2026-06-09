# STG Identity & Governance Lifecycle Specification

Document Information
- Document Name: identity-governance-spec.md
- Repository: STG-CONSENSUS
- Folder/Path: identity/
- Category: IDENTITY-LAYER
- Version: IDENT-V2-2026
- Status: Foundational Resolution
- Classification: Sovereign Trust Generation Infrastructure

---

## 1. Purpose
Spesifikasi ini mendefinisikan aturan main formal mengenai mutasi status identitas warga dari lapisan autentikasi lokal hingga menghasilkan otorisasi sah pada transaksi hibah ekonomi, pencatatan database relasional Airtable, dan eksekusi otomatisasi perbankan.

## 2. Identity Mutation Workflow (PIN ──► PID ──► PIH)
Untuk mencegah eksploitasi identitas palsu (Sybil Attack), setiap mutasi data wajib melalui tiga tahapan validasi kriptografis:

1. **Activation Phase (PIN)**: Warga melakukan autentikasi lokal pada node disk menggunakan PIN personal. Tahap ini murni mengaktifkan kunci sesi lokal dan tidak memicu mutasi on-chain.
2. **Registry Phase (PID)**: Nomor registrasi permanen `STG-PID-XXXXXX` dicocokkan dengan basis data relasional. Satu manusia asli hanya diizinkan memegang satu nomor deskriptor permanen.
3. **Obfuscation Phase (PIH)**: Jaringan mengeksekusi `keccak256(PID + Local Secret)` untuk melahirkan Personal Identity Hash (PIH). PIH inilah yang dikirim ke Buku Induk sebagai bukti privasi mutlak di bawah Ghost Protocol.

## 3. Financial Authorization Logic (Bridge Integration)
Fungsi transfer dana pada `banking_bridge.py` atau alokasi anggaran ramah lingkungan di Airtable dinyatakan VALID dan dapat dieksekusi hanya jika memenuhi kondisi berikut:
- Transaksi wajib ditandatangani secara kriptografis oleh Dompet Multi-Sig Utama `0xD9a1E28224d6d047Eef8712dC97d11A9032b948e`.
- Payload transaksi wajib menyertakan parameter `voter_pih_proof` yang terverifikasi aktif pada indeks state root Jaringan `8081`.
- Jika parameter PIH tidak ditemukan atau status lifecycle identitas berada dalam kondisi *Frozen/Rejected*, Jenderal Sadewa secara otomatis akan mengaktifkan Circuit Breaker untuk memblokir aliran dana keluar dalam waktu 300 mikrodetik.

## 4. AI Telemetry Boundaries
- Sistem AI diperbolehkan membaca kecocokan hash PIH, mendeteksi anomali penumpukan transaksi di satu PID, serta menyajikan laporan diagnostik kepatuhan.
- Sistem AI MUTLAK dilarang merubah parameter status lifecycle identitas secara sepihak atau mem-bypass otorisasi tanda tangan digital dari dompet induk Juru Masak Tertinggi.

