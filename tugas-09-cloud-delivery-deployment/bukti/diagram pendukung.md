### Diagram Pendukung

```mermaid
flowchart LR
    W["Warga"]
    I["Instansi<br/>(Dukcapil, kepolisian, bank)"]

    subgraph PUB["PUBLIC CLOUD (region Indonesia)<br/>tanpa data pribadi mentah"]
        CDN["CDN dan WAF"]
        POR["Portal cek status<br/>(PaaS/SaaS)"]
        GW["API gateway dan<br/>identitas instansi (PaaS)"]
        Q["Antrean permintaan"]
    end

    subgraph PRV["PRIVATE CLOUD milik negara<br/>(wilayah Indonesia)"]
        DB["Database kependudukan<br/>dan biometrik ABIS<br/>(IaaS privat)"]
        HSM["HSM<br/>(kunci enkripsi)"]
        SIEM["SIEM dan audit log<br/>(IaaS/PaaS privat)"]
    end

    W --> CDN
    I --> CDN
    CDN --> POR
    CDN --> GW
    GW --> Q
    Q -- "mTLS (risiko 2 dan 3)" --> DB
    DB -- "data status saja, satu arah (risiko 1)" --> POR
    HSM --- DB
    GW -.-> SIEM
    DB -.-> SIEM
```

Keterangan:
- Garis putus-putus: pencatatan akses untuk audit.
- Risiko 1: ketidaksinkronan data. Risiko 2: kebocoran data pribadi. Risiko 3: penyalahgunaan hak akses instansi (sesuai Jawaban Soal 3).