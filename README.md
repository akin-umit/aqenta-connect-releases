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

**Aqenta Connect**, Aqenta SaaS panelinden gelen yazdır / mali işlem komutlarını yerel donanıma ileten Windows masaüstü uygulamasıdır. Sistem tepsisinde çalışır. Otomatik güncelleme (Supabase `agent_versions` + GitHub Release), SHA256 doğrulama ve rollback ile kesintisiz çalışır.

### Özellikler

| Özellik | Açıklama |
|---------|----------|
| **Termal yazıcı** | ESC/POS (USB, seri, ağ) |
| **ÖKC** | Z/X raporu, satış fişi |
| **Para çekmecesi** | Yazıcı üzerinden tetikleme |
| **Cloud yazdırma** | `print_jobs` tablosu — KDS ve kasa farklı PC’den |
| **Otomatik güncelleme** | Aktif sürüm `agent_versions`; indirme **ZIP** (içinde tek `AqentaConnect.exe`) |
| **SHA256** | **Çıkarılmış EXE** üzerinden doğrulama (`checksum_sha256` = EXE hash, ZIP değil) |
| **Rollback** | Güncelleme başarısız → yedekten dönüş |
| **Uzaktan komut** | `self_update`, `restart` job tipleri |
| **Yerel API** | `http://localhost:19542` |

---

## Kurulum

### İlk kurulum

1. Aşağıdaki **ZIP** veya kurulum dosyasını indirin (`AqentaConnect.zip` içinde tek EXE).
2. ZIP ise: çıkartın → `AqentaConnect.exe` çalıştırın.
3. Aqenta panelinden **Ayarlar → Donanım** ile kurulum anahtarını gönderin.
4. Sonraki güncellemeler çoğu kurulumda **otomatik** (OTA).

### Owner — yeni sürüm (platform)

1. **Owner** hesabı → **Agent sürümleri**.
2. `version` (örn. `1.3.12`), `download_url` = bu repodaki `.../releases/download/vX.Y.Z/AqentaConnect.zip` veya `latest/download/AqentaConnect.zip`.
3. `file_size_bytes` = ZIP boyutu; `checksum_sha256` = **publish edilen `AqentaConnect.exe`** SHA256 (ZIP hash’i **kullanmayın** — OTA “bütünlük” hatası verir).
4. Kayıt aktif olunca tüm agent’lar yeni sürümü görür.

---

## İndirme

<p align="center">
  <a href="https://github.com/akin-umit/aqenta-connect-releases/releases/latest/download/AqentaConnect.zip">
    <img src="https://img.shields.io/badge/%C4%B0ndir-AqentaConnect.zip-00b894?style=for-the-badge&logo=windows" alt="Download ZIP"/>
  </a>
</p>

> **AqentaConnect.zip** — içinde tek **AqentaConnect.exe** (~64–70 MB self-contained). OTA ve panel indirme linkleri bu dosyayı hedefler.

<details>
<summary>Kurulum sihirbazı (varsa)</summary>

Eski veya alternatif dağıtımda **AqentaConnect-Setup.exe** kullanılıyorsa [Releases](https://github.com/akin-umit/aqenta-connect-releases/releases) sayfasından seçin.

</details>

---

## Teknik özeti

| Alan | Değer |
|------|--------|
| **Platform** | Windows 10/11 x64 |
| **Runtime** | .NET 8 self-contained |
| **Yerel API** | `http://localhost:19542` |
| **Veri** | `%LOCALAPPDATA%\AqentaConnect` |
| **Güncelleme kaynağı** | Supabase `agent_versions` + GitHub asset |

### API uçları

```
GET  /health          → Durum
GET  /diagnostics     → Tanı (DB aktif sürüm, yazıcı, güncelleme)
POST /setup           → Kurulum anahtarı
POST /print           → ESC/POS
POST /open-drawer     → Çekmece
POST /fiscal/sale     → ÖKC satış
GET  /fiscal/z-report → Z raporu
GET  /fiscal/x-report → X raporu
```

### Supabase tabloları

| Tablo | Açıklama |
|-------|----------|
| `agent_versions` | Aktif sürüm, `download_url`, **`checksum_sha256` (EXE)**, mandatory |
| `agent_instances` | Heartbeat, sürüm, güncelleme mesajı |
| `print_jobs` | Yazdırma ve uzaktan komut kuyruğu |

---

## Sürüm geçmişi

### v1.3.12 (22 Mart 2026) — Mutfak fişi & OTA düzeltmesi

- **Mutfak fişi (MADDE 18):** sipariş notu (`internal_note`), baskı sırası (`print_sequence`), ürün satır notları; `receipt_role: kitchen_ticket` ile panel uyumu.
- **Dağıtım:** GitHub asset **`AqentaConnect.zip`**; SaaS `printAgentClient.ts` ile aynı URL kalıbı.
- **Owner / `agent_versions`:** `checksum_sha256` alanı ZIP değil, **ZIP’ten çıkan `AqentaConnect.exe`** dosyasının SHA256 değeridir (yanlış hash OTA’da “Dosya bütünlüğü doğrulanamadı” verir).

### v1.3.11 (Mart 2026) — Kararlı cloud taban

- `print_jobs` ile uçtan uca bulut yazdırma; agent 5 sn polling.
- OTA: GitHub **ZIP** + `agent_versions` aktif satır; `self_update` / `restart` uzaktan komutları.
- **v1.3.12**’ye geçiş: mutfak fişi içerik iyileştirmeleri + checksum alanı düzeltme notları.

### v1.3.0 (17 Mart 2026) — Cloud Print Pipeline

Yazdırma mimarisi tamamen bulut kuyruğa taşındı: panel `print_jobs` yazar, agent polling ile işler. KDS ve kasa farklı cihazdayken `localhost` HTTP yerine veritabanı köprüsü kullanılır.

- Cloud-first yazdırma, `agent_instances` ile tanı / heartbeat
- KDS’den doğrudan HTTP yazdırma kaldırıldı (DB INSERT)
- RLS / agent yazma izinleri güncellendi

### v1.2.9 (16 Mart 2026)

- Uçtan uca smoke test; uzaktan restart; `update_history.log`

### v1.2.8

- SHA256 doğrulama, rollback (CMD batch), Owner’dan tekil güncelleme komutu

### v1.2.5 — v1.2.7

- `/diagnostics`, indirme progress, CMD script, HttpClient timeout iyileştirmesi

### v1.2.0 — v1.2.4

- Yerinde güncelleme, dashboard durumu, heartbeat UPSERT

### v1.0.0 (15 Mart 2026)

- İlk sürüm: ESC/POS, ÖKC, yerel HTTP API, `print_jobs` polling, şifreli config, otomatik güncelleme

---

## Güvenlik

- Config **AES-256-GCM** + Windows DPAPI  
- Yerel API **localhost**  
- Güncelleme dosyası **SHA256** ile doğrulanır; başarısızlıkta **rollback**

---

## Destek

- **Web:** [aqenta.com.tr/destek](https://aqenta.com.tr/destek)
- **E-posta:** destek@aqenta.com.tr

---

<p align="center">
  <sub>Copyright © 2024–2026 Aqenta. Tüm hakları saklıdır.</sub>
</p>
