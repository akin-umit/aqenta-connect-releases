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

**Aqenta Connect**, Aqenta işletme paneli ile birlikte çalışan Windows uygulamasıdır. Fiş yazdırma, para çekmecesi ve uyumlu ÖKC işlemlerini bilgisayarınızdaki donanıma iletir. Sistem tepsisinde çalışır; yeni sürümler **otomatik güncelleme** ile gelir (indirme doğrulanır, sorun olursa güvenli geri dönüş).

### Özellikler

| Özellik | Açıklama |
|---------|----------|
| **Termal yazıcı** | ESC/POS (USB, seri, ağ) |
| **ÖKC** | Z/X raporu, satış fişi (desteklenen markalar) |
| **Para çekmecesi** | Yazıcı üzerinden tetikleme |
| **Bulut yazdırma** | Panelden gelen yazdırma işleri sırayla işlenir (mutfak / kasa farklı bilgisayarda olabilir) |
| **İlk kurulum** | **AqentaConnect-Setup.exe** — müşteri önce bunu kurar |
| **Sonraki sürümler** | Otomatik güncelleme; tekrar Setup şart değildir |
| **Yerel API** | `http://localhost:19542` (sadece bu bilgisayar) |

---

## Kurulum (ilk kez)

1. Aşağıdan **AqentaConnect-Setup.exe** indirin ve çalıştırın.
2. Kurulum sihirbazını izleyin (tanıtım → lisans → klasör → Kur).
3. Kurulum bitince program açılır; tepsi ikonundan erişebilirsiniz.
4. Aqenta panelinizde **Ayarlar → Donanım** bölümünden kurulum anahtarını oluşturup uygulamaya gönderin.
5. Bundan sonra yeni sürümler **otomatik** iner ve uygulanır.

---

## İndirme

### Müşteri — ilk kurulum (önerilen)

<p align="center">
  <a href="https://github.com/akin-umit/aqenta-connect-releases/releases/latest/download/AqentaConnect-Setup.exe">
    <img src="https://img.shields.io/badge/%C4%B0ndir-AqentaConnect--Setup.exe-00b894?style=for-the-badge&logo=windows" alt="İndir Setup"/>
  </a>
</p>

> **AqentaConnect-Setup.exe** — Inno Setup kurulum paketi. Restoran ve işletmeler ilk kurulumda **bunu** kullanır.

### Gelişmiş / teknik (ZIP)

Otomatik güncelleme veya tek dosya dağıtımı için release’lerde **AqentaConnect.zip** de yayınlanır; içinde **AqentaConnect.exe** bulunur. İlk kurulum için öncelik yine **Setup**’tır.

<p align="center">
  <a href="https://github.com/akin-umit/aqenta-connect-releases/releases/latest/download/AqentaConnect.zip">
    <img src="https://img.shields.io/badge/AqentaConnect.zip-tek%20EXE-6366f1?style=for-the-badge&logo=windows" alt="ZIP"/>
  </a>
</p>

Tüm dosyalar: [Releases](https://github.com/akin-umit/aqenta-connect-releases/releases)

---

## Teknik özet

| Alan | Değer |
|------|--------|
| **Platform** | Windows 10/11 (x64) |
| **Runtime** | .NET 8 (self-contained) |
| **Yerel API** | `http://localhost:19542` |
| **Veri klasörü** | `%LOCALAPPDATA%\AqentaConnect` |

### Yerel API uçları

```
GET  /health          → Durum
GET  /diagnostics     → Tanı bilgisi
POST /setup           → Kurulum anahtarı
POST /print           → Fiş yazdır
POST /open-drawer     → Çekmece
POST /fiscal/sale     → ÖKC satış
GET  /fiscal/z-report → Z raporu
GET  /fiscal/x-report → X raporu
```

---

## Sürüm geçmişi

### v1.3.12 (22 Mart 2026)

- Mutfak fişinde sipariş notu, baskı sırası ve ürün satır notlarının daha düzgün basılması.
- Otomatik güncelleme sırasında dosya doğrulama iyileştirmesi (kullanıcı tarafında ek işlem gerekmez).

### v1.3.11 (Mart 2026)

- Bulut tabanlı yazdırma akışında kararlılık; mutfak ve kasa farklı PC’deyken yazdırmanın sorunsuz işlemesi.
- Otomatik güncelleme paketleri (ZIP) ile dağıtım.

### v1.3.0 (17 Mart 2026) — Cloud Print Pipeline

- Yazdırma işleri panel üzerinden kuyruğa alınır; Connect sırayla işler.
- Mutfak ve kasa ekranlarının ayrı cihazlarda kullanımına uyum.

### v1.2.9 (16 Mart 2026)

- Uçtan uca testler; uzaktan yeniden başlatma komutu; güncelleme geçmişi kaydı.

### v1.2.8

- İndirilen güncelleme için bütünlük doğrulama; sorun olursa otomatik geri dönüş.

### v1.2.5 — v1.2.7

- Tanı (`/diagnostics`) ve indirme deneyimi iyileştirmeleri.

### v1.2.0 — v1.2.4

- Yerinde güncelleme ve durum göstergeleri.

### v1.0.0 (15 Mart 2026)

- İlk sürüm: ESC/POS, ÖKC, yerel HTTP API, şifreli ayarlar, otomatik güncelleme.

---

## Güvenlik

- Ayarlar dosyası şifrelenir; anahtar Windows ile korunur.
- Yerel servis yalnızca bu bilgisayardan erişilebilir.
- Güncelleme paketi doğrulanır; başarısız güncellemede geri dönüş.

---

## Destek

- **Web:** [aqenta.com.tr/destek](https://aqenta.com.tr/destek)
- **E-posta:** destek@aqenta.com.tr

---

<p align="center">
  <sub>Copyright © 2024–2026 Aqenta. Tüm hakları saklıdır.</sub>
</p>
