# MAKİNA · Pro — Web Sürümü (`makina.html`)

MAKİNA masaüstü platformunun **Pro paketi** özelliklerini tek dosyalık, kurulumsuz bir
web uygulaması (PWA) olarak sunar. Tarayıcıda `makina.html` açılır; harici bir sunucu
gerekmez. AI özellikleri — MAKİNA'nın kendisi gibi — **senin bilgisayarındaki yerel
Ollama / Qwen** (`localhost:11434`) üzerinden çalışır; veri buluta gitmez.

> Bu, MAKİNA'nın masaüstü kaynak kodunu değiştirmez (o kaynak bu depoda değil, Drive'daki
> `MAKINA.zip` içinde ve bu ortamdan indirilemiyor). Bunun yerine aynı fikri, aynı yerel-AI
> mimarisiyle bağımsız bir web sürümü olarak yeniden kurar. Pilates uygulaması (`index.html`)
> hiç değişmedi.

## Çalıştırma

- **En basit:** `makina.html` dosyasını tarayıcıda aç.
- **AI'ın çalışması için** (MAKİNA ile aynı): Ollama'yı tarayıcı erişimine izin verecek
  şekilde başlat ve modeli indir:
  ```bash
  OLLAMA_ORIGINS=* ollama serve
  ollama pull qwen2.5:7b
  ```
  Uygulamayı `http://localhost` üzerinden (ya da `file://`) açarsan tarayıcı yerel AI'a
  bağlanabilir. HTTPS bir sayfadan `http://localhost`'a istek "karışık içerik" nedeniyle
  engellenebilir — Ayarlar'dan uç noktayı değiştirebilirsin.
- Model değiştirmek için: Ayarlar ⚙️ → *Model* (örn. `qwen2.5:3b`, `qwen3:8b`). Koda dokunmadan.

## Özellik durumu

**Canlı (tarayıcıdan doğrudan, anahtarsız, gerçek veri):**
- 🌍 Depremler — USGS canlı feed (büyüklüğe göre işaret, M5+ halka efekti, otomatik uyarı)
- ✈️ Uçuşlar — OpenSky (yönüne dönük işaret, tık → rota/irtifa/hız/ülke detayı)
- 🛰️ Uydular — Celestrak TLE + `satellite.js` ile canlı yörünge propagasyonu
- 🌋🔥🌀 Volkan / Yangın / Fırtına — NASA EONET canlı afet olayları
- 🌌 Aurora / Uzay havası — NOAA Kp
- 🪐 Gezegen Keşif — NASA APOD (günün görüntüsü + AI Türkçe çeviri), Perseverance/Curiosity
  son fotoğrafları, canlı ay evresi, 9 gök cismi bilgi kartı
- 🌍 3D Dünya Küresi — globe.gl; olaylar + M5+ halkaları küre üzerinde
- ⏪ Zamanda Geri Git — 1–24 saat kaydırma
- 🗂️ Ülke Paneli — canlı olaylardan hesaplanan risk skoru + AI brifing
- 📥 CSV dışa aktarma · 🔔 kritik olay bildirimleri · 🗺️ sağ tık bölge brifingi

**AI özellikleri (yerel Qwen ile canlı):**
- 🧠 AI İstihbarat Analisti — görünen olayları örüntü/küme olarak yorumlar, doğal dil soru-cevap
- 🔎 Kendi Verimize Soru Sor — yanıtı SADECE uygulamanın topladığı kayıtlardan üretir; yetersizse söyler
- 🎯 İsabet Karnesi — AI öngörülerini sonraki gerçek depremlerle otomatik eşleştirir
- 👁 AI Agent İzleme · 📰 AI Sabah Brifingi (🔊 sesli + 📄 PDF) · 📍 Yakınımdaki Riskler (300 km)
- 🚨 Kriz Merkezi (500 km) · doğal dille haritayı yapılandırma

**Veri köprüsü gerektirenler** (katman menüsünde `veri bağla` rozetiyle işaretli — tarayıcıdan
doğrudan CORS/anahtar engeli var; MAKİNA masaüstünde `osiris/` backend'i sağlar):
- 🚢 Gemiler / ⛔ Yaptırımlı gemi (OFAC×AIS) / 📡 AIS Karartma — AIS akış anahtarı
- ☢️ Radyasyon (Safecast) · 🧲 Manyetik anomali (EMAG2) · 🌿 NDVI (MODIS) · 📡 Schumann
- 📷 AI Kamera Analizi — görüntü-yetenekli model + kamera karesi
- 🔌 Deniz altı kablo / fay / nükleer katmanları — örnek düğümlerle gösterilir; tam GeoJSON
  URL'si aynı `GEO_DATA` deseniyle bağlanır.

Her köprü katmanına tıklayınca hangi veri kaynağının gerektiği ekranda yazar.

## Genişletme

- Yeni canlı katman: `LAYERS`'a giriş ekle + bir `loadX()` fonksiyonu yaz (`loadQuakes` desenini kopyala) → `LOADERS`'a bağla.
- Backend köprüsü: Ayarlar'daki uç nokta mantığıyla `osiris/` API'sine `fetch` at, dönen olayları `setEvents('ship', [...])` ile besle.
- Kod tek dosyada, derleme adımı yok. Kütüphaneler (Leaflet, satellite.js, globe.gl) CDN'den, tarayıcıda yüklenir.
