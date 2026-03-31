<p align="center">
  <img src="https://app.aqenta.com.tr/logo.svg" alt="Aqenta" width="200"/>
</p>

<h1 align="center">Aqenta Connect v1.3.12</h1>

<p align="center">
  <strong>Restoran yazıcı & ÖKC entegrasyon programı</strong><br/>
  Windows masaüstü uygulaması — yerel yazıcı, mali cihaz ve para çekmecesi yönetimi
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

**Aqenta Connect**, Aqenta SaaS panelinden gelen yazdır / mali işlem komutlarını yerel donanıma ileten Windows masaüstü uygulamasıdır. Sistem tepsisinde çalışır, kaynak tüketimi minimumda tutar. Otomatik güncelleme, uzaktan yönetim ve rollback mekanizması ile kesintisiz çalışır.

### Özellikler

| Özellik | Açıklama |
|---------|----------|
| **Termal yazıcı** | ESC/POS uyumlu (USB, seri, ağ) — Epson, Bixolon, Star, Xprinter |
| **ÖKC entegrasyonu** | Ödeme kaydedici cihaz — Z/X raporu, fiş kesme |
| **Para çekmecesi** | Yazıcı üzerinden otomatik tetikleme |
| **Otomatik güncelleme** | Periyodik sürüm kontrolü, ilerleme penceresi, CMD batch script |
| **SHA256 checksum** | İndirilen dosya bütünlük doğrulaması |
| **Rollback** | Güncelleme başarısız → otomatik eski sürüme dönüş |
| **Uzaktan yönetim** | Panel üzerinden güncelleme, yeniden başlatma |
| **Güncelleme geçmişi** | `update_history.log` ile sürüm geçişlerini kaydetme |
| **Dashboard** | `http://localhost:19542/health` — durum özeti |
| **Güvenlik** | AES-256-GCM şifreleme, DPAPI, localhost-only API |

---

## Kurulum

### İlk kurulum (tek sefer) — müşteri

1. Aşağıdaki **AqentaConnect-Setup.exe** dosyasını indirin.
2. Çalıştırın → **Program tanıtımı** → **Sözleşme** → **Kurulum klasörü** → Kur.
3. Program otomatik başlar ve sistem tepsisine yerleşir.
4. Aqenta panelinden **Ayarlar → Donanım** sayfasına gidin.
5. **Kurulum anahtarı oluştur** ve **Aqenta Connect’e gönderin**.
6. Hazır; agent yapılandırılır ve çalışır.

### Otomatik güncelleme

İlk kurulumdan sonra **yeniden Setup çalıştırmanız gerekmez**. Program kendini günceller:

- Arka planda yeni sürüm kontrolü
- Yeni sürüm varsa ilerleme penceresi ile indirme
- **SHA256** ile dosya doğrulama
- **CMD batch** ile EXE değişimi ve yeniden başlatma
- Başarısız olursa **otomatik rollback**
- İşlemler **güncelleme geçmişi** dosyasına yazılır

---

## İndirme

### İlk kurulum (önerilen)

<p align="center">
  <a href="https://github.com/akin-umit/aqenta-connect-releases/releases/download/v1.3.19/AqentaConnect-Setup.exe">
    <img src="https://img.shields.io/badge/%C4%B0ndir-AqentaConnect--Setup.exe-00b894?style=for-the-badge&logo=windows" alt="Download Setup"/>
  </a>
</p>

> **AqentaConnect-Setup.exe** — İlk kurulum için Inno Setup kurulum paketi (~40–45 MB, sürüme göre değişebilir). Sonraki sürümler çoğunlukla otomatik gelir.

### Otomatik güncelleme / tek dosya (ZIP)

OTA ve tek EXE dağıtımı için release’lerde **AqentaConnect.zip** yayınlanır (içinde `AqentaConnect.exe`). **Standart müşteri yolu yine Setup’tır.**

<p align="center">
  <a href="https://github.com/akin-umit/aqenta-connect-releases/releases/latest/download/AqentaConnect.zip">
    <img src="https://img.shields.io/badge/AqentaConnect.zip-tek%20EXE-6366f1?style=for-the-badge&logo=windows" alt="ZIP"/>
  </a>
</p>

<details>
<summary>Tek EXE doğrudan indirme (gelişmiş)</summary>

[AqentaConnect.exe (latest)](https://github.com/akin-umit/aqenta-connect-releases/releases/latest/download/AqentaConnect.exe) — çoğu kullanıcı için gerekmez; güncelleme sistemi veya teknik dağıtım içindir.

</details>

[Tüm sürümler — Releases](https://github.com/akin-umit/aqenta-connect-releases/releases)

---

## Teknik detaylar

| Özellik | Değer |
|---------|-------|
| **Platform** | Windows 10/11 (x64) |
| **Runtime** | .NET 8 (self-contained — ek kurulum gerekmez) |
| **Boyut** | Setup ~40–45 MB; tek EXE ~60–70 MB (sürüme göre) |
| **Yerel API** | `http://localhost:19542` |
| **Veri klasörü** | `%LOCALAPPDATA%\AqentaConnect` |
| **Şifreleme** | AES-256-GCM + Windows DPAPI |
| **Başlangıç** | İsteğe bağlı Windows ile otomatik başlatma |
| **Güncelleme** | Sürüm kaynağı + GitHub asset + CMD batch + rollback |
| **Checksum** | SHA256 doğrulama |

### API uç noktaları

```
GET  /health          → Agent durumu (sürüm, yazıcı, ÖKC, config)
GET  /diagnostics     → Ayrıntılı tanı (bağlantı, config, sürüm)
GET  /update-status   → Güncelleme durumu (status, percent, target)
GET  /trigger-update  → Manuel güncelleme tetikle
POST /setup           → Kurulum anahtarı gönder
POST /print           → ESC/POS fiş yazdır
POST /open-drawer     → Para çekmecesi aç
POST /fiscal/sale     → ÖKC satış fişi
GET  /fiscal/z-report → Z raporu
GET  /fiscal/x-report → X raporu
```

### Mimari (kaynak yapısı)

```
AqentaConnect.exe (.NET 8, self-contained, single-file)
├── Services/
│   ├── AutoUpdater.cs            — Otomatik güncelleme (sürüm kaynağı → GitHub)
│   ├── PrintJobPollingService.cs — print_jobs kuyruğundan iş alma
│   ├── LocalHttpServer.cs        — HTTP API (19542, API key + rate limit)
│   ├── SupabaseService.cs        — Supabase REST istemcisi
│   ├── PrinterService.cs         — ESC/POS yazdırma
│   └── FiscalDeviceService.cs    — ÖKC / mali cihaz
├── ConfigProtector.cs            — Şifreli config yönetimi
└── Program.cs                    — WinForms + arka plan servisleri
```

### Veri tabloları (panel tarafı — özet)

| Tablo | Açıklama |
|-------|----------|
| `agent_versions` | Aktif sürüm, indirme adresi, checksum, zorunlu güncelleme bayrağı |
| `agent_instances` | Kayıtlı agent’lar, sürüm, heartbeat, güncelleme durumu |
| `print_jobs` | Yazdırma ve uzaktan komut kuyruğu (polling) |

---

## Sürüm geçmişi

### v1.3.13 (23 Mart 2026)

- Toplu dağıtım tek seferde tamamlandı: GitHub Release `v1.3.13` + `AqentaConnect.zip` yayımlandı.
- Yazdırma akışında aynı sipariş için çift basım riskine karşı agent tarafında dedupe/idempotency koruması eklendi.
- Kuyruk iş tipleri mutfak ve kapanış fişi için ayrıştırıldı (`kitchen_receipt`, `checkout_receipt`), eski `receipt` uyumu korundu.
- `receipt_role` parse akışı güncellendi; mutfak/kasa fiş semantiği daha tutarlı hale getirildi.

### v1.3.12 (22 Mart 2026)

- Mutfak fişinde sipariş notu, baskı sırası ve ürün satır notlarının daha düzgün basılması (panel ile uyumlu içerik).
- Otomatik güncellemede dosya bütünlüğü doğrulamasının iyileştirilmesi.

### v1.3.11 (17 Mart 2026) — Cloud Print Pipeline

Yazdırma mimarisi tamamen bulut tabanlı pipeline’a taşındı. Agent artık doğrudan HTTP çağrısı almak yerine Supabase `print_jobs` tablosunu polling ile takip eder. KDS ve kasa paneli farklı cihazlardan (tablet, PC) açıldığı için `localhost` HTTP yaklaşımı terk edildi — tüm yazdırma işlemleri veritabanı üzerinden yürütülmektedir.

**Yeni özellikler:**
- Cloud-first yazdırma: Tüm fiş işleri `print_jobs` tablosuna yazılır, agent polling ile alıp yazdırır
- Uzaktan tanı: Agent her polling döngüsünde heartbeat gönderir; `agent_instances.update_message` üzerinden iş durumu ve hatalar izlenebilir
- Otomatik hata yönetimi: Başarısız işler `error` olarak işaretlenir; geçici hatalar sonraki döngüde tekrar denenir
- Uzaktan komutlar: `self_update` ve `restart` job tipleri

**Mimari değişiklikler:**
- KDS panelinden HTTP yazdırma çağrısı kaldırıldı — sadece DB INSERT
- Dosya tabanlı duplicate koruması kaldırıldı — Supabase status filtresi yeterli
- Startup zamanı ve 5 dakikalık `created_at` filtreleri kaldırıldı — bekleyen işler yaşa göre dışlanmaz
- Veritabanı RLS politikaları genişletildi: agent için SELECT/UPDATE/INSERT; mutfak ve garson rolleri için yazma

**Düzeltilen sorunlar:**
- Agent’ın veritabanı yazma yetkisi eksikti — yazdırma başarılı olsa bile iş `printing` durumunda kalıyordu
- Hatalı işler tekrar denenemiyor, dosyaya yazılıp kalıcı olarak atlanıyordu
- KDS farklı cihazdan açıldığında `localhost` HTTP erişilemiyordu — yazdırma sessizce başarısız oluyordu
- Garson sipariş teslim ettiğinde gereksiz fiş basılıyordu — fiş artık sadece kasa hesap kapanışında basılır

### v1.3.0 (17 Mart 2026)

- Tüm akışlar anlık — gereksiz bekleme azaltıldı
- Güncelleme kontrolü sıklaştırıldı; yeni sürüm bulununca indirme başlar
- Agent poll ve heartbeat iyileştirmeleri
- Owner panel: Supabase realtime (polling yok)
- Ayarlar: tek adımda kurulum anahtarı gönderimi
- Düzeltme: `print_jobs` payload→`content`, type→`job_type` (400 hatası)
- Düzeltme: lisans sonrası agent yeniden başlatma

### v1.2.9 (16 Mart 2026)

- Uçtan uca smoke test (v1.2.8 → v1.2.9 otomatik güncelleme)
- Uzaktan restart komutu (Owner panelden)
- Güncelleme geçmişi loglama (`update_history.log`)
- Installer: tanıtım + sözleşme + dizin seçimi

### v1.2.8

- SHA256 checksum doğrulama (indirilen EXE bütünlük kontrolü)
- Rollback mekanizması (CMD batch: başarısız → yedekten dönüş)
- Tekil agent güncelleme (Owner panelde butonu)

### v1.2.5 — v1.2.7

- `/diagnostics` uç noktası, API key bypass
- WinForms ilerleme penceresi (indirme)
- PowerShell → CMD batch script
- HttpClient timeout düzeltmesi (ayrı download client, uzun timeout)

### v1.2.0 — v1.2.4

- PowerShell ile yerinde güncelleme
- Dashboard güncelleme durumu (ilerleme + buton)
- Heartbeat UPSERT düzeltmesi
- Windows Defender istisnası (kurulumda)

### v1.1.0

- Yapılandırılmış durum kontrolü (ön uç entegrasyonu)

### v1.0.1 — v1.0.3

- Profesyonel fiş şablonu, yazıcı bilgisi panelde
- KDS fiş fiyat düzeltmesi
- Türkçe karakter ASCII transliterasyonu
- Otomatik güncelleme (geri sayımlı onay)

### v1.0.0 (15 Mart 2026)

- İlk sürüm: Splash screen, GUI sihirbazı, AES-256-GCM şifreleme
- ESC/POS yazdır (USB, seri, ağ), otomatik yazıcı keşfi, çoklu yazıcı
- ÖKC entegrasyonu (Ingenico, Hugin, Profilo, Beko, Olivetti, Casio, Custom SpA)
- Yerel HTTP API (`localhost:19542`), Supabase `print_jobs` polling
- Sistem tepsisi, şifreli config, otomatik güncelleme

---

## Güvenlik

- Konfigürasyon dosyası **AES-256-GCM** ile şifrelenmiştir
- Anahtar **Windows DPAPI** ile korunur (yalnızca o PC’de çözülebilir)
- Yerel API sadece `localhost` üzerinden dinler — dış ağdan erişilemez
- CORS ile yalnız izin verilen origin’ler kabul edilir
- İndirilen güncellemeler **SHA256 checksum** ile doğrulanır
- Güncelleme başarısız olursa **otomatik rollback** yapılır

---

## Destek

- **Web:** [aqenta.com.tr/destek](https://aqenta.com.tr/destek)
- **E-posta:** destek@aqenta.com.tr

---

<p align="center">
  <sub>Copyright © 2024–2026 Aqenta Teknoloji A.Ş. Tüm hakları saklıdır.</sub>
</p>
