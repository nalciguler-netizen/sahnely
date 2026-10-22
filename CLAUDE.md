# CLAUDE.md – Sahnely

## Amaç
Hakan'ın senaryolarını, kameraya önden bakarak konuşan karakterli YouTube videolarına çevirmek (Shorts 9:16 + derleme 16:9).
İki parça var:
- **Sahnely Play** (`com.sahnely.app`): Google Play'deki uygulama. 2D karakter + ekran görüntüsü/video modu. Ses kullanıcının kendi kaydıdır.
- **HIKAYE_STUDYO** (iç kullanım): 3D/gerçekçi sahne, yüz ağı animasyonu, Kaggle üretimi. Kanal videoları burada yapılır.

## Altyapı (tamamı ücretsiz)
- **GitHub Pages** (hesap `nalciguler-netizen`)
  - Play uygulaması: repo `sahnely` → https://nalciguler-netizen.github.io/sahnely/
  - İç uygulama: repo `HIKAYE_STUDYO`, dosya `hikaye-studyosu.html` + `sw.js` (adı index.html DEĞİL)
  - Ortak `assetlinks.json`: kök repo `nalciguler-netizen.github.io/.well-known/` (diğer uygulamalarla paylaşımlı; yalnızca Sahnely bloğuna dokun)
- **Telefonda çalışan teknolojiler:** MediaPipe FaceLandmarker + WebGL yüz ağı, Whisper (transformers.js) altyazı, MediaRecorder
- **Kaggle** (GPU T4 x2, haftada 30 saat, her zaman "Save & Run All")
  - Ses: Chatterbox
  - Sahne: SDXL / RealVisXL
  - Gerçekçi dudak senkronu: LatentSync 1.5 (`sahnely_dudak_senkronu_v3.py`)
- **Google Drive:** paylaşılan `sahnely` klasöründen senaryo ve ses girdileri
- **Cloudflare:** Sahnely şu an kullanmıyor. Gerekirse yalnızca ücretsiz Workers katmanı.
- **Paketleme:** PWABuilder (TWA). Play hesabı Yurdagül Güler adına; kapalı test (12+ kişi, 14 gün).

## Alınan kararlar
- Kanal oranı: yaklaşık %70 gerçekçi, %20 3D animasyon, %10 2D. Günde 2 Short; haftada 1 derleme.
- **Gerçekçi yüzlerde** ağız uygulamada çizilmez → Kaggle'da LatentSync ile üretilir, uygulamada "Sadece video" + videonun kendi sesi kullanılır.
- **3D animasyonda** uygulamadaki yüz ağı kullanılır: resim sabit kalır, sadece kafa/ağız/göz oynar (14 yaş test ayarları).
- Kaggle çıktısı 1080×1920 olur; resmin tamamı görünür, boşluk bulanık arka planla dolar; uzun ses 20 sn'lik parçalarla işlenir.
- Sahneler önden bakan karakterle üretilir. Dosya adları `NN_yas_cinsiyet` (ör. `23_55_erkek`).
- Play uygulamasında yapay zekâ / TTS sesi YOK.

## Yapılmaması gerekenler
- `signing.keystore`, `signing-key-info.txt`, şifre ve token'lar asla GitHub'a ya da paylaşılan Drive klasörüne konmaz (yalnızca özel "Uygulama anahtarları" klasörü).
- Ücretli servis önerilmez (uygulama para kazanana kadar).
- Bir tarzı düzeltirken diğerini bozma (gerçekçi ↔ 3D animasyon ayrı yollar).
- Paket adı `com.sahnely.app` ve imza anahtarı değiştirilmez.
- Uzun Kaggle işleri etkileşimli oturumda çalıştırılmaz.
- Güncellemede yalnızca değişen dosya/satır verilir; teslim test edilmiş, tek parça olur.
- Dosya adlarında "(1)" eki olmamalı; Kaggle'a yapıştırılan kodun sürüm satırı kontrol edilir.
- Kurulum adımları tek tek ve kısa verilir; kullanıcı istemedikçe ders anlatılmaz.

## Ticari kullanım kuralları
- Yalnızca ticari kullanıma açık lisanslar: MediaPipe (Apache 2.0), LatentSync (Apache 2.0), Chatterbox (MIT), SDXL/RealVisXL (OpenRAIL++). Wav2Lip gibi ticari olmayan lisanslar KULLANILMAZ.
- Kanalda kullanılan aile seslerinin ticari kullanım izni yazılı olmalı.
- Play uygulaması: veriler cihazda kalır, veri toplanmaz, reklam yok. Gizlilik ve kullanım şartları `sahnely/gizlilik.html` ve `kullanim-sartlari.html` dosyalarında. Uygulamada "Uygunsuz içerik bildir" düğmesi bulunur (sahnely.destek@gmail.com).
- Kullanıcının ürettiği videoların hakları kullanıcıya aittir.
- YouTube'da her videoda yapay zekâ/sentetik içerik beyanı açılır. Açıklamaya şu satırlar eklenir: "Gerçek olaylardan esinlenilmiştir, isimler ve karakterler kurgusaldır." ve "Bu video Sahnely ile üretildi."
- Senaryolar Hakan'ındır; içerik "özgün olmayan / seri üretim" görünmemeli (her videoda farklı hikâye, karakter ve açılış).
