<p align="center">
  <img src="https://app.aqenta.com.tr/logo.svg" alt="Aqenta" width="200"/>
</p>

<h1 align="center">Aqenta Connect Agent</h1>

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
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20(x64)-blue?style=for-the-badge" alt="Platform"/>
</p>

---

## Nedir?

**Aqenta Connect**, Aqenta SaaS panelinden gelen yazdir/mali islem komutlarini yerel donanima ileten hafif bir Windows uygulamasidir. Sistem tepsisinde calisir, kaynak tuketimi minimumda tutar.

### Desteklenen Donanim

| Kategori | Detay |
|----------|-------|
| **Termal Yazicilar** | ESC/POS uyumlu (USB, Seri Port, Ag) — Epson, Bixolon, Star, Xprinter, vb. |
| **OKC / Yazar Kasa** | Genel protokol destegi (Seri Port) |
| **Para Cekmecesi** | Yazici uzerinden tetikleme (ESC/POS kick komutu) |

---

## Kurulum

### Ilk Kurulum (Tek Sefer)

1. Asagidaki **AqentaConnect-Setup.exe** dosyasini indirin
2. Calistirin → Ileri → Kur
3. Program otomatik baslar ve sistem tepsisine yerlesir
4. Aqenta panelinden **Ayarlar → Donanim** sayfasina gidin
5. "Kurulum Anahtari Olustur" butonuna basin
6. "Aqenta Connect Lisans Gonder" ile anahtari gonderin
7. Hazir! Agent yapilandirildi ve calisir durumda

### Otomatik Guncelleme

Ilk kurulumdan sonra **tekrar kurulum yapmaniza gerek yoktur**. Program kendini otomatik gunceller:

- Her **2 dakikada bir** yeni surum kontrolu yapar
- Yeni surum varsa arka planda indirir
- Programi yeniden baslatir — **kurulum ekrani gostermez**
- Kullanici mudahalesi gerektirmez

---

## Indirme Linkleri

| Dosya | Aciklama | Link |
|-------|----------|------|
| **AqentaConnect-Setup.exe** | Ilk kurulum icin (Inno Setup installer) | [Indir](https://github.com/akin-umit/aqenta-connect-releases/releases/latest/download/AqentaConnect-Setup.exe) |
| **AqentaConnect.exe** | Otomatik guncelleme icin (dogrudan EXE) | [Indir](https://github.com/akin-umit/aqenta-connect-releases/releases/latest/download/AqentaConnect.exe) |

> **Not:** Normal kullanicilar sadece **AqentaConnect-Setup.exe** indirmelidir. `AqentaConnect.exe` otomatik guncelleme sistemi tarafindan kullanilir.

---

## Teknik Detaylar

| Ozellik | Deger |
|---------|-------|
| **Platform** | Windows 10/11 (x64) |
| **Runtime** | .NET 8 (self-contained — ek kurulum gerekmez) |
| **Yerel API** | `http://localhost:19542` |
| **Veri Klasoru** | `%LOCALAPPDATA%\AqentaConnect` |
| **Sifreleme** | AES-256-GCM + Windows DPAPI |
| **Baslangic** | Windows ile otomatik baslar (HKCU\Run) |
| **Guncelleme** | Supabase `agent_versions` tablosu + PowerShell |

### API Endpointleri

```
GET  /health        → Agent durumu (surum, yazici, OKC, config)
POST /setup         → Lisans anahtari gonder
POST /print         → ESC/POS fis yazdir
POST /open-drawer   → Para cekmecesi ac
POST /fiscal/sale   → OKC satis fisi
GET  /fiscal/z-report → Z raporu
GET  /fiscal/x-report → X raporu
POST /check-update  → Manuel guncelleme kontrolu
```

---

## Surum Gecmisi

### v1.2.0 (16 Mart 2026)
- PowerShell ile yerinde guncelleme (Inno Setup kurulum ekrani yok)
- 2 dakika guncelleme kontrol araligi
- Heartbeat UPSERT fix (duplicate kayit sorunu cozuldu)
- print_jobs status uyumu (queued/pending)
- Windows Defender exclusion (kurulumda otomatik)
- Yapimci bilgileri ve dijital imza altyapisi

### v1.1.0
- Configured durum kontrolu (frontend entegrasyonu)
- Agent running vs configured ayrimi

### v1.0.3
- Turkce karakter ASCII transliteration
- Fis footer duzeltme
- Otomatik guncelleme (30sn geri sayim)
- Owner panel surum yonetimi

### v1.0.2
- KDS fis fiyat duzeltme
- On izleme iyilestirme

### v1.0.1
- Profesyonel fis sablonu
- Yazici bilgisi panelde
- MALi DEGERi YOKTUR basimi
- Barkod numarasi

### v1.0.0
- Ilk surum: Splash screen, GUI wizard, AES-256-GCM sifreleme
- Otomatik yazici kesif, coklu yazici destegi

---

## Guvenlik

- Konfigürasyon dosyasi **AES-256-GCM** ile sifrelenmistir
- Anahtar **Windows DPAPI** ile korunur (sadece o PC'de cozulebilir)
- Yerel API sadece `localhost` dinler — dis agdan erisilemez
- CORS ile sadece izin verilen originler kabul edilir

---

## Destek

- **Web:** [aqenta.com.tr/destek](https://aqenta.com.tr/destek)
- **E-posta:** destek@aqenta.com.tr

---

<p align="center">
  <sub>Copyright &copy; 2024-2026 Aqenta Teknoloji A.S. Tum haklari saklidir.</sub>
</p>
