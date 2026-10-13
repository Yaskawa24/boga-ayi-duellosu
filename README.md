# Boğa Ayı Düellosu

Meme coin grafiklerinde **LONG** ya da **SHORT** açıp botları ve arkadaşlarını yendiğin mobil tarayıcı oyunu. Tek bir `index.html` dosyası, harici kütüphane ya da CDN yok, sesler Web Audio ile üretiliyor. PWA olarak çevrimdışı da çalışıyor.

Canlı sürüm: https://yaskawa24.github.io/boga-ayi-duellosu/

## Dosyalar
- `index.html`: oyunun tamamı
- `manifest.json`: PWA bilgileri (ad, ikon, tema rengi, dikey tam ekran)
- `sw.js`: çevrimdışı önbellek. **Her yeni sürümde `VERSION` değerini artır.**
- `icons/`: 192, 512 ve maskable 512 PNG ikonlar

## Play Store'a çıkarma

### Yol 1: Bubblewrap ile TWA (en kolayı)
Oyun zaten bir PWA olduğu için Google'ın Bubblewrap aracı onu Android uygulamasına sarar.

1. Node.js 18+ ve Java 17 (JDK) kur.
2. `npm i -g @bubblewrap/cli`
3. Boş bir klasörde: `bubblewrap init --manifest https://yaskawa24.github.io/boga-ayi-duellosu/manifest.json`
   - Paket adı örneği: `com.yaskawa24.bogaayi`
   - İmza anahtarı (keystore) oluşturmasını kabul et. **Anahtarı ve şifresini kaybetme**, sonraki güncellemeler için gerekli.
4. `bubblewrap build`: `app-release-bundle.aab` dosyası oluşur.
5. Adres çubuğu görünmesin diye Digital Asset Links ekle:
   - `bubblewrap fingerprint list` ile SHA-256 parmak izini al (Play App Signing kullanıyorsan Play Console'daki "Uygulama imzalama" parmak izini kullan).
   - Şu dosyayı yayınla: `https://yaskawa24.github.io/.well-known/assetlinks.json`
   - Not: GitHub Pages'te bu dosya **kök alan adında** olmalı. Bunun için `yaskawa24.github.io` adlı ayrı bir depo gerekir, ya da kendi alan adını kullan.
6. Play Console'da (tek seferlik 25 $ geliştirici ücreti) yeni uygulama oluştur, `.aab` dosyasını yükle, mağaza açıklaması, ekran görüntüleri, gizlilik politikası ve içerik derecelendirmesini doldur.

### Yol 2: Capacitor (dosyalar uygulamanın içinde, internet gerekmez)
1. `npm init -y && npm i @capacitor/core @capacitor/cli @capacitor/android`
2. `npx cap init "Boğa Ayı Düellosu" com.yaskawa24.bogaayi --web-dir www`
3. `index.html`, `manifest.json`, `sw.js` ve `icons/` dosyalarını `www/` içine kopyala.
4. `npx cap add android && npx cap sync`
5. `npx cap open android`: Android Studio açılır. Orada Build > Generate Signed Bundle ile `.aab` oluştur.
6. İkonlar için `@capacitor/assets` paketini kullanabilirsin. Dikey kilit için `AndroidManifest.xml` içinde `android:screenOrientation="portrait"` ekle.

### Mağaza notları
- Oyun gerçek para kullanmıyor ve gerçek alım satım yapmıyor. Mağaza açıklamasında bunu açıkça yaz ("eğitim/eğlence amaçlı simülasyon"). Kripto temalı uygulamalar incelemede dikkatle değerlendirilir.
- Veriler (ayarlar, reyting, skorlar) sadece cihazda `localStorage` içinde tutuluyor. Basit bir gizlilik politikası sayfası yine de gerekli.
- Çevrimiçi sıralama yok, sıralama cihaz içi. Küresel sıralama için ileride bir sunucu (ör. Firebase) gerekir.
