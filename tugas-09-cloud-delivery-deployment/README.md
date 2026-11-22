# Tugas 9 (Pekan 9) — Cloud Delivery & Deployment Model

**Materi terkait:** Karakteristik esensial cloud computing (NIST SP 800-145), Deployment Model (Public/Private/Hybrid/Community), Service Model (IaaS/PaaS/SaaS).

> Bagian 2 mata kuliah ini murni **analisis & desain di atas kertas** — tidak ada biaya cloud sama sekali, karena tugas berhenti di tahap rekomendasi, bukan implementasi nyata (implementasi baru mulai Tugas 11).

## Studi Kasus

Pemerintah ingin membangun **Sistem Data Kependudukan** (nama, NIK, alamat, data biometrik) yang:
- Sangat **rahasia** (data pribadi warga negara, diatur UU Perlindungan Data Pribadi).
- Diakses oleh **ribuan instansi** (dukcapil daerah, kepolisian, bank untuk verifikasi KYC) dengan pola akses yang **tidak rata** (lonjakan saat musim pemilu/pendaftaran sekolah).
- Butuh **skalabilitas** agar tidak downtime saat lonjakan akses, tapi **tidak boleh** datanya berada di infrastruktur yang tidak terkendali penuh oleh negara.

## Tugas Kelompok

1. **Rekomendasikan Deployment Model** (Public / Private / Hybrid / Community Cloud) yang paling tepat untuk sistem ini. Jelaskan alasannya berdasarkan **karakteristik NIST** (on-demand self-service, broad network access, resource pooling, rapid elasticity, measured service) dikaitkan dengan kebutuhan kerahasiaan vs skalabilitas.
2. **Rekomendasikan Service Model** (IaaS / PaaS / SaaS) yang tepat untuk komponen-komponen berbeda dari sistem ini (boleh berbeda service model untuk komponen berbeda, mis. database inti vs portal publik untuk pengecekan status).
3. Jelaskan **minimal 2 risiko spesifik** dari pilihan kalian dan mitigasinya (mis. jika Hybrid Cloud dipilih: risiko sinkronisasi data antara private & public segment).
4. Bandingkan singkat dengan **satu alternatif yang kalian tolak** — kenapa alternatif itu kurang tepat (mis. kenapa Public Cloud murni ditolak, atau kenapa Private Cloud murni dianggap kurang skalabel).

## Jawaban Tugas Kelompok
### Jawaban Soal 1
Kelompok kami merekomendasikan Hybrid Cloud dengan inti privat berdaulat. Database kependudukan (NIK, alamat, dan data biometrik) ditempatkan di private cloud milik negara di dalam wilayah Indonesia. Public cloud di region Indonesia hanya digunakan untuk komponen yang tidak menyimpan data pribadi mentah, yaitu portal status, CDN dan WAF, API gateway tepi, serta antrean permintaan.

Pilihan ini muncul dari dua tuntutan yang bertolak belakang. Data biometrik tidak dapat diganti jika bocor, dan UU No. 27 Tahun 2022 tentang Perlindungan Data Pribadi serta UU Administrasi Kependudukan menuntut pengelolaan yang ketat di bawah kendali negara. Di sisi lain, ribuan instansi seperti Dukcapil daerah, kepolisian, dan bank mengakses sistem dengan lonjakan besar saat pemilu dan pendaftaran sekolah.

Kelima karakteristik NIST juga mendukung pembagian tersebut:
1. Resource pooling: data biometrik tidak boleh berbagi infrastruktur dengan tenant komersial. Pooling data sensitif dibatasi antar-instansi pemerintah di dalam private cloud.
2. Rapid elasticity: lonjakan trafik menaikkan jumlah permintaan, bukan jumlah data. Karena itu elastisitas cukup diterapkan pada lapisan stateless yang boleh burst ke public cloud, sementara data tetap di tempatnya.
3. Broad network access: akses ke inti hanya melalui API gateway, mTLS, dan jaringan privat. Hanya portal informasi yang terbuka ke internet.
4. On-demand self-service: instansi dapat mendaftar dan meminta kunci API sendiri, tetapi persetujuan hak akses tetap diberikan admin pusat.
5. Measured service: pemakaian per instansi dicatat untuk kuota, penagihan, dan audit trail, sekaligus untuk mendeteksi pengambilan data yang tidak wajar.

### Jawaban Soal 2
Service model dibedakan menurut tingkat kendali yang dibutuhkan setiap komponen:

1. Database inti dan sistem biometrik (ABIS + HSM): IaaS privat. Negara harus mengendalikan sistem operasi, enkripsi, dan kunci kriptografi.
2. API gateway dan manajemen identitas instansi: PaaS. Mudah diskalakan, sehingga tim fokus pada kebijakan akses dan kuota, bukan pengelolaan server.
3. Portal cek status publik: PaaS atau SaaS di public cloud. Datanya tidak sensitif dan trafiknya paling melonjak.
4. SIEM dan audit log: IaaS atau PaaS privat. Log berisi jejak akses terhadap NIK warga.


## Struktur Submission

```
tugas-09-cloud-delivery-deployment/
├── README.md      # Analisis lengkap (isi dengan poin 1-4 di atas)
├── JURNAL.md
└── bukti/          # (opsional) diagram pendukung
```

## Rubrik Penilaian (Tugas 9)

| Komponen | Bobot | Kriteria |
|---|---|---|
| Ketepatan rekomendasi deployment model | 30% | Argumen berbasis karakteristik NIST, bukan sekadar "private cloud lebih aman" tanpa detail |
| Ketepatan rekomendasi service model | 25% | Pemetaan komponen sistem ke IaaS/PaaS/SaaS masuk akal |
| Analisis risiko & mitigasi | 25% | Risiko spesifik ke konteks data kependudukan (bukan risiko generik) |
| Perbandingan dengan alternatif yang ditolak | 20% | Argumen penolakan jelas dan berdasar |

## Batasan Penggunaan AI (Level 2)

Kebijakan **Level 2 (AI Assisted Idea Generation & Structuring)** berlaku — lihat [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Boleh bertanya ke AI gambaran umum karakteristik NIST/deployment model untuk brainstorming; **tidak boleh** meminta AI menuliskan rekomendasi/analisis akhir yang tinggal ditempel ke `README.md`. Catat pemakaian AI di "Log Penggunaan AI" pada `JURNAL.md`.

- Jawaban yang terasa seperti "template" (bisa dipakai untuk sistem cloud apa saja tanpa menyebut spesifik kependudukan/data pribadi/regulasi) akan dinilai rendah pada komponen ketepatan rekomendasi.
- `README.md` wajib membahas minimal satu skenario tandingan yang kalian buat sendiri (mis. "bagaimana jika instansi daerah tidak punya infrastruktur private cloud sendiri?") beserta jawabannya.
