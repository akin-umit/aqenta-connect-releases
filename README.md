<p align="center">
  <img src="https://app.aqenta.com.tr/logo.svg" alt="Aqenta" width="200"/>
</p>

<h1 align="center">Aqenta Connect v1.3.11</h1>

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
| **Otomatik Guncelleme** | 10sn aralikla kontrol, anlik indirme, CMD batch script |
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

- Her **10 saniyede bir** yeni surum kontrolu yapar
- Yeni surum varsa **anlik** ilerleme penceresi acilir ve indirilir
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

### v1.3.11 (17 Mart 2026) — Cloud Print Pipeline

Yazdirma mimarisi tamamen bulut tabanli pipeline'a tasindi. Agent artik dogrudan HTTP cagrisi almak yerine Supabase `print_jobs` tablosunu polling ile takip eder. KDS ve kasa paneli farkli cihazlardan (tablet, PC) acildigi icin `localhost` HTTP yaklasimi terk edildi — tum yazdirma islemleri veritabani uzerinden yurutulmektedir.

**Yeni Ozellikler:**
- Cloud-first yazdirma: Tum fis isleri `print_jobs` tablosuna yazilir, agent polling ile alip yazdirir
- Uzaktan diagnostik: Agent her polling cycle'da heartbeat gonderir; `agent_instances.update_message` alanindan is durumu, hata mesajlari ve polling metrikleri anlik izlenebilir
- Otomatik hata yonetimi: Basarisiz isler `error` olarak isaretlenir; gecici hatalar sonraki cycle'da tekrar denenir
- Uzaktan komutlar: `self_update` ve `restart` job tipleri ile agent uzaktan yonetilebilir

**Mimari Degisiklikler:**
- KDS panelinden HTTP yazdirma cagrisi kaldirildi — sadece DB INSERT
- Dosya tabanli duplicate korumasi kaldirildi — Supabase status filtresi yeterli
- Startup zamani ve 5 dakikalik created_at filtreleri kaldirildi — tum bekleyen isler yasindan bagimsiz islenir
- Veritabani RLS policy'leri genisletildi: agent icin SELECT/UPDATE/INSERT izni, kitchen ve waiter rolleri icin write izni

**Duzeltilen Sorunlar:**
- Agent'in veritabani yazma yetkisi eksikti — yazdirma basarili olsa bile is `printing` durumunda kaliyordu
- Hatali isler tekrar denemiyor, dosyaya yazilip kalici olarak atlaniyordu
- KDS farkli cihazdan acildiginda `localhost` HTTP erisilemiyordu — yazdirma sessizce basarisiz oluyordu
- Garson siparis teslim ettiginde gereksiz fis basiliyordu — fis artik sadece kasa hesap kapanisinda basilir

---

### v1.3.0 (17 Mart 2026)
- Tum akislar anlik — bekleme/gecikme yok
- Guncelleme kontrolu: 10sn aralik, bulunca anlik indirme
- Agent poll interval: 2sn, heartbeat: her poll cycle'da
- Owner panel: Supabase realtime (polling yok)
- Settings: tek butonla otomatik lisans gonder
- Fix: print_jobs payload→content, type→job_type (400 hatasi)
- Fix: lisans sonrasi agent restart

---

### v1.2.9 (16 Mart 2026)
- Uctan uca smoke test gecti (v1.2.8 → v1.2.9 otomatik guncelleme)
- Uzaktan restart komutu (Owner panelden)
- Guncelleme gecmisi loglama (update_history.log)
- Installer guncellendi: tanitim + sozlesme + dizin secimi

### v1.2.8
- SHA256 checksum dogrulama (indirilen EXE butunluk kontrolu)
- Rollback mekanizmasi (CMD batch: basarisiz → .bak'tan geri donus)
- Tekil agent guncelleme (Owner panelde butonu)

### v1.2.5 — v1.2.7
- /diagnostics endpoint, API key bypass
- WinForms progress dialog (indirme ilerlemesi)
- PowerShell → CMD batch script (daha guvenilir)
- HttpClient timeout fix (ayri download client, 15dk timeout)

### v1.2.0 — v1.2.4
- PowerShell ile yerinde guncelleme
- Dashboard guncelleme durumu (progress bar + buton)
- Heartbeat UPSERT fix
- Windows Defender exclusion (kurulumda otomatik)

### v1.1.0
- Configured durum kontrolu (frontend entegrasyonu)

### v1.0.1 — v1.0.3
- Profesyonel fis sablonu, yazici bilgisi panelde
- KDS fis fiyat duzeltme
- Turkce karakter ASCII transliteration
- Otomatik guncelleme (30sn geri sayim)

### v1.0.0 (15 Mart 2026)
- Ilk surum: Splash screen, GUI wizard, AES-256-GCM sifreleme
- ESC/POS yazdir (USB, Seri, Ag), otomatik yazici kesif, coklu yazici
- OKC entegrasyonu (Ingenico, Hugin, Profilo, Beko, Olivetti, Casio, Custom SpA)
- Local HTTP API (localhost:19542), Supabase print_jobs polling
- System tray, sifreli config, otomatik guncelleme

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
