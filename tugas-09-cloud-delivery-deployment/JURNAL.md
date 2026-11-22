# Jurnal Proses — Tugas 9

## Diskusi deployment model
- Opsi yang dipertimbangkan dan kenapa akhirnya memilih salah satu:
    1. Public Cloud murni : Dipertimbangkan karena mampu menyesuaikan kapasitas dengan cepat ketika terjadi lonjakan akses. Namun, opsi ini tidak dipilih karena data NIK dan biometrik harus tetap berada dalam infrastruktur yang sepenuhnya dikendalikan negara, terlebih lagi data biometrik tidak bisa diganti jika mengalami kebocoran.
    2. Private Cloud murni : Dipertimbangkan karena memiliki tingkat kendali dan keamanan data yang lebih tinggi serta sesuai dengan UU No. 27 Tahun 2022. Tetapi kapasitasnya perlu dipersiapkan terlebih dahulu sehingga berisiko mengalami penurunan performa ketika diakses ribuan instansi secara bersamaan.
    3. Community Cloud : Sempat menjadi pilihan karena sistem digunakan oleh berbagai instansi pemerintah. Namun, mengingat data kependudukan bersifat sangat sensitif, kendali penuh oleh negara lebih diutamakan daripada penggunaan infrastruktur bersama.

- Pertimbangan utama : Lonjakan akses yang berlebih berpengaruh terhadap jumlah permintaan daripada jumlah data yang disimpan. Oleh karena itu, penyesuaian kapasitas cukup dilakukan pada lapisan layanan yang berada di depan, sementara database utama tetap berada di private cloud. Keputusan ini juga mempertimbangkan lima karakteristik NIST, yaitu resource pooling, rapid elasticity, broad network access, on-demand self-service, dan measured service.

- Hal yang menjadi bahan diskusi : Penentuan jenis data yang diperbolehkan berada di public cloud. Hasil kesepakatannya hanya data yang tidak sensitif seperti status pengajuan yang dapat dikirim ke public cloud. Selain itu, layanan public cloud harus menggunakan region Indonesia.

## Diskusi service model per komponen
- Database inti dan sistem biometrik (ABIS + HSM) : Menggunakan IaaS privat agar negara tetap memiliki kendali terhadap sistem operasi, enkripsi, dan pengelolaan kunci keamanan.
- API gateway dan manajemen identitas instansi : Menggunakan PaaS karena lebih mudah disesuaikan dengan kebutuhan akses, sehingga pengelola dapat berfokus pada pengaturan hak akses dan kuota.
- Portal cek status publik : Menggunakan PaaS atau SaaS di public cloud karena tidak mengelola data pribadi mentah dan membutuhkan kapasitas yang fleksibel ketika trafik meningkat.
- SIEM dan audit log : Menggunakan IaaS atau PaaS privat karena menyimpan catatan aktivitas akses yang berkaitan dengan data kependudukan warga.
- Pertimbangan utama : Pemilihan service model disesuaikan dengan kebutuhan kendali dan tingkat sensitivitas setiap komponen. Komponen yang mengelola data sensitif menggunakan IaaS agar negara memiliki kendali lebih besar, sedangkan komponen yang tidak terlalu sensitif dapat menggunakan PaaS atau SaaS agar pengelolaan layanan menjadi lebih praktis.

## Log Penggunaan AI (Level 2)

> Wajib diisi sesuai kebijakan Level 2 di [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Tulis "Tidak memakai AI" pada baris pertama jika memang tidak dipakai. Hanya untuk brainstorming ide/outline — bukan untuk kode/analisis/teks akhir.

| Tanggal | Tool AI | Prompt yang diberikan | Ringkasan saran/ide AI | Bagaimana diolah jadi tulisan/kode sendiri |
|---|---|---|---|---|
| 22 Nov 2026 | Claude  | Aku sedang ada tugas sistem data kependudukan yang menyimpan informasi sensitif. Menurutmu, apakah keamanan data dan kebutuhan akses dari banyak instansi bisa menjadi dua hal yang saling bertentangan dalam penerapan cloud? | AI menjelaskan adanya perbedaan kebutuhan antara keamanan data dan fleksibilitas akses. Penggunaan Private Cloud dan Public Cloud memiliki kelebihan masing-masing yang dapat dipertimbangkan. | Saya mengembangkan diskusi tersebut dengan membandingkan deployment model dan menghubungkannya dengan karakteristik NIST hingga menentukan model yang sesuai. |
| 22 Nov 2026 | Claude  | Kalau dalam satu sistem ada penyimpanan data, penghubung antarinstansi, dan aplikasi yang digunakan masyarakat, apakah pengelolaannya memang harus dibedakan? | AI menjelaskan bahwa setiap komponen memiliki kebutuhan pengelolaan berbeda sehingga penggunaan service model tidak harus sama. | Saya mengembangkan pembahasan dengan mengelompokkan komponen sistem berdasarkan kebutuhan infrastruktur, pengembangan aplikasi, dan penggunaan layanan. Kemudian kami menentukan penerapan IaaS, PaaS, dan SaaS. |
| 22 Nov 2026 | Claude  | Meminta AI untuk merangkum materi dari PPT dan menjelaskan hubungan setiap topik dengan konsep cloud delivery dan deployment. | AI merangkum materi Coordination (jam fisik dan logis, mutual exclusion, election, gossip, pub-sub, location systems) dari PPT di Project, karena file yang dilampirkan hanya berisi satu slide kosong. AI juga memetakan tiap topik ke mekanisme cloud seperti distributed lock, leader election, message queue, dan multi-region. | Menggunakan rangkuman ini sebagai bahan belajar untuk memahami bahwa cloud adalah sistem terdistribusi, lalu menghubungkannya dengan pembahasan deployment dan service model pada tugas. |
| 22 Nov 2026 | ChatGPT | Meminta penjelasan mengenai hal-hal yang perlu dipertimbangkan ketika membandingkan Hybrid Cloud dengan deployment model lainnya pada sistem data kependudukan. | AI membantu memberikan gambaran mengenai aspek yang perlu diperhatikan, seperti keamanan data, kendali infrastruktur, dan kemampuan menangani lonjakan akses. | menggunakan gambaran tersebut sebagai bahan diskusi untuk menentukan alternatif yang akan dibandingkan. Pemilihan alternatif dan penyusunan alasan dilakukan berdasarkan pemahaman serta hasil diskusi kelompok. |
| ... | ... | ... | ... | ... |