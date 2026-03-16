<p align="center">
  <img src="https://app.aqenta.com.tr/logo.svg" alt="Aqenta" width="200"/>
</p>

<h1 align="center">Aqenta Connect v1.2.9</h1>

<p align="center">
  <strong>Restoran Yazici & OKC Entegrasyon Programi</strong><br/>
  Windows masaustu uygulamasi — yerel yazici, mali cihaz ve para cekmecesi yonetimi
</p>

<p align="center">
  <a href="https://github.com/akin-umit/aqenta-connect-releases/releases/latest">
    <img src="https://img.shields.io/github/v/release/akin-umit/aqenta-connect-releases?style=for-the-badge&color=00b894&label=Son%20S%C3%BCr%C3%BCm" alt="Latest Release"/>
  </a>
  <a href="https://github.com/akin-umit/aqenta-connect-releases/releases/latest">
    <img src="https://img.shields.io/github/downloads/akin-umit/aqenta-connect-releases/total?style=for-the-badge&color=0984e3&label=%C4%B0ndirme" alt="Downloads"/>
  </a>
  <img src="https://img.shields.io/badge/.NET-8.0-512BD4?style=for-the-badge" alt=".NET"/>
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20(x64)-blue?style=for-the-badge" alt="Platform"/>
</p>

---

## Nedir?

**Aqenta Connect**, Aqenta SaaS panelinden gelen yazdir/mali islem komutlarini yerel donanima ileten Windows masaustu uygulamasidir. Sistem tepsisinde calisir, kaynak tuketimi minimumda tutar. Otomatik guncelleme, uzaktan yonetim ve rollback mekanizmasi ile kesintisiz calisir.

### Ozellikler

| Ozellik | Aciklama |
|---------|----------|
| **Termal Yazici** | ESC/POS uyumlu (USB, Seri, Ag) — Epson, Bixolon, Star, Xprinter |
| **OKC Entegrasyonu** | Odeme Kaydedici Cihaz — Z/X rapor, fis kesme |
| **Para Cekmecesi** | Yazici uzerinden otomatik tetikleme |
| **Otomatik Guncelleme** | 2dk aralikla kontrol, WinForms dialog, CMD batch script |
| **SHA256 Checksum** | Indirilen dosya butunluk dogrulamasi |
| **Rollback** | Guncelleme basarisiz → otomatik eski surume donus |
| **Uzaktan Yonetim** | Panel uzerinden guncelleme, restart, silme |
| **Guncelleme Gecmisi** | update_history.log ile surum gecislerini kaydetme |
| **Dashboard** | http://localhost:19542/health — HTML kontrol paneli |
| **Guvenlik** | AES-256-GCM sifreleme, DPAPI, localhost-only API |

---

## Kurulum

### Ilk Kurulum (Tek Sefer)

1. Asagidaki **AqentaConnect-Setup.exe** dosyasini indirin
2. Calistirin → **Program Tanitimi** → **Sozlesme Kabulu** → **Dizin Secimi** → Kur
3. Program otomatik baslar ve sistem tepsisine yerlesir
4. Aqenta panelinden **Ayarlar → Donanim** sayfasina gidin
5. "Kurulum Anahtari Olustur" butonuna basin
6. "Aqenta Connect Lisans Gonder" ile anahtari gonderin
7. Hazir! Agent yapilandirildi ve calisir durumda

### Otomatik Guncelleme

Ilk kurulumdan sonra **tekrar kurulum yapmaniza gerek yoktur**. Program kendini otomatik gunceller:

- Her **2 dakikada bir** yeni surum kontrolu yapar
- Yeni surum varsa **ilerleme penceresi** acilir ve indirilir
- **SHA256 checksum** ile dosya butunlugu dogrulanir
- **CMD batch script** ile EXE degistirilir ve program yeniden baslar
- Basarisiz olursa **otomatik rollback** — eski surum geri yuklenir
- Tum islem **guncelleme gecmisi** dosyasina kaydedilir

---

## Indirme

<p align="center">
  <a href="https://github.com/akin-umit/aqenta-connect-releases/releases/latest/download/AqentaConnect-Setup.exe">
    <img src="https://img.shields.io/badge/Indir-AqentaConnect--Setup.exe-00b894?style=for-the-badge&logo=windows" alt="Download Setup"/>
  </a>
</p>

> **AqentaConnect-Setup.exe** — Ilk kurulum icin (42 MB, Inno Setup installer).
> Kurulum sonrasi guncellemeler otomatik yapilir, tekrar indirmenize gerek yoktur.

<details>
<summary>Gelistirici Notu: AqentaConnect.exe (dogrudan EXE)</summary>

`AqentaConnect.exe` (134 MB) otomatik guncelleme sistemi tarafindan kullanilir.
Normal kullanicilarin bu dosyayi indirmesine gerek yoktur.

[AqentaConnect.exe Indir](https://github.com/akin-umit/aqenta-connect-releases/releases/latest/download/AqentaConnect.exe)

</details>

---

## Teknik Detaylar

| Ozellik | Deger |
|---------|-------|
| **Platform** | Windows 10/11 (x64) |
| **Runtime** | .NET 8 (self-contained — ek kurulum gerekmez) |
| **Boyut** | Setup: ~42 MB, EXE: ~134 MB |
| **Yerel API** | `http://localhost:19542` |
| **Veri Klasoru** | `%LOCALAPPDATA%\AqentaConnect` |
| **Sifreleme** | AES-256-GCM + Windows DPAPI |
| **Baslangic** | Windows ile otomatik baslar (HKCU\Run) |
| **Guncelleme** | Supabase `agent_versions` + CMD batch + rollback |
| **Checksum** | SHA256 dogrulama (agent_versions.checksum_sha256) |

### API Endpointleri

```
GET  /health          → Agent durumu (surum, yazici, OKC, config)
GET  /diagnostics     → Detayli tani bilgisi (supabase, config, surum)
GET  /update-status   → Guncelleme durumu (status, percent, target)
GET  /trigger-update  → Manuel guncelleme tetikle
POST /setup           → Lisans anahtari gonder
POST /print           → ESC/POS fis yazdir
POST /open-drawer     → Para cekmecesi ac
POST /fiscal/sale     → OKC satis fisi
GET  /fiscal/z-report → Z raporu
GET  /fiscal/x-report → X raporu
```

### Mimari

```
AqentaConnect.exe (.NET 8, self-contained, single-file)
├── Services/
│   ├── AutoUpdater.cs           — Otomatik guncelleme (Supabase → GitHub Releases)
│   ├── PrintJobPollingService.cs — print_jobs tablosundan is alma (receipt/fiscal/restart/update)
│   ├── LocalHttpServer.cs       — HTTP API (port 19542, API key + rate limit)
│   ├── SupabaseService.cs       — Supabase REST API istemcisi
│   ├── ThermalPrinterService.cs — ESC/POS yazici surucusu
│   └── OkcDriverService.cs      — OKC (fiscal) surucusu
├── ConfigProtector.cs            — Sifreli config.dat yonetimi
└── Program.cs                    — WinForms + Background Services
```

### Supabase Tablolari

| Tablo | Aciklama |
|-------|----------|
| `agent_versions` | Aktif surum, download URL, checksum, mandatory flag |
| `agent_instances` | Kayitli Agent'lar, surum, heartbeat, guncelleme durumu |
| `print_jobs` | Yazdirma + guncelleme + restart komutlari (polling) |

---

## Surum Gecmisi

### v1.2.9 (16 Mart 2026) — FINAL
- **Uctan uca smoke test** gecti (v1.2.8 → v1.2.9 otomatik guncelleme)
- Uzaktan restart komutu (Owner panelden)
- Guncelleme gecmisi loglama (update_history.log)
- Installer guncellendi: tanitim yazisi + sozlesme kabulu + dizin secimi

### v1.2.8
- SHA256 checksum dogrulama (indirilen EXE butunluk kontrolu)
- Rollback mekanizmasi (CMD batch: basarisiz → .bak'tan geri donus)
- Tekil agent guncelleme (Owner panelde ⬆ butonu)

### v1.2.7
- API key bypass (/diagnostics, /update-status, /trigger-update)
- Smoke test PASSED

### v1.2.5 — v1.2.6
- /diagnostics endpoint
- WinForms progress dialog (indirme ilerlemesi gorsel)
- PowerShell → CMD batch script (daha guvenilir)
- HttpClient timeout fix (ayri download client, 15dk timeout)

### v1.2.4
- Dashboard guncelleme durumu (progress bar + buton + /update-status)
- Dinamik surum logu

### v1.2.0
- PowerShell ile yerinde guncelleme
- 2 dakika guncelleme kontrol araligi
- Heartbeat UPSERT fix
- Windows Defender exclusion (kurulumda otomatik)

### v1.1.0
- Configured durum kontrolu (frontend entegrasyonu)

### v1.0.3
- Turkce karakter ASCII transliteration
- Otomatik guncelleme (30sn geri sayim)

### v1.0.2
- KDS fis fiyat duzeltme

### v1.0.1
- Profesyonel fis sablonu
- Yazici bilgisi panelde

### v1.0.0
- Ilk surum: Splash screen, GUI wizard, AES-256-GCM sifreleme
- Otomatik yazici kesif, coklu yazici destegi

---

## Guvenlik

- Konfigürasyon dosyasi **AES-256-GCM** ile sifrelenmistir
- Anahtar **Windows DPAPI** ile korunur (sadece o PC'de cozulebilir)
- Yerel API sadece `localhost` dinler — dis agdan erisilemez
- CORS ile sadece izin verilen originler kabul edilir
- Indirilen guncellemeler **SHA256 checksum** ile dogrulanir
- Guncelleme basarisiz olursa **otomatik rollback** yapilir

---

## Destek

- **Web:** [aqenta.com.tr/destek](https://aqenta.com.tr/destek)
- **E-posta:** destek@aqenta.com.tr

---

<p align="center">
  <sub>Copyright &copy; 2024-2026 Aqenta Teknoloji A.S. Tum haklari saklidir.</sub>
</p>
