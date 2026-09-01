# Router

Sevkiyat rota planlayıcı — durakları en kısa/en verimli sırayla dizen, elle
sıralanan tur ile karşılaştırıp yol/süre/yakıt tasarrufunu gösteren, tek
dosyalık bir web uygulaması.

## Özellikler

- **İki mod**: Şehirler arası (81 il arasında) ve şehir içi (GPS/koordinat
  tabanlı) dağıtım planlaması.
- **Rota motoru**: En yakın komşu (nearest neighbour) + 2-opt iyileştirmesi
  ile durakları sıralar; elle girilen sırayla karşılaştırıp kazanılan
  km/süre/₺ yakıtı gösterir.
- **Gerçek yol verisi (OSRM)**: "Gerçek yol verisi kullan" açıldığında
  mesafe ve güzergâh, kuş uçuşu tahmini yerine ücretsiz [OSRM](https://project-osrm.org/)
  demo servisinden (`router.project-osrm.org`) alınır — internet bağlantısı
  gerekir; bağlantı yoksa veya servise erişilemezse otomatik olarak kuş
  uçuşu hesaba geri döner. *Not: bu genel demo servisi test/deneme amaçlıdır;
  yoğun/üretim kullanımı için kendi OSRM sunucunuzu veya ücretli bir
  sağlayıcıyı düşünün.*
- **Çoklu araç/şoför desteği**: Birden fazla araç tanımlanabilir (isim +
  kapasite kg). Duraklar; elle atama yapılmadıysa depoya göre açısal
  sıralama (sweep) ile ve araç kapasitesine duyarlı şekilde otomatik
  bölüştürülür. Her araç için ayrı optimize rota, harita üzerinde farklı
  renkte çizgi ve kendi sevkiyat tablosu üretilir; kapasiteye sığmayan
  duraklar uyarı olarak listelenir.
- **Veri kalıcılığı**: Durak listesi, araçlar, adres defteri ve tüm
  ayarlar tarayıcının `localStorage`'ında saklanır — sayfa yenilense veya
  kapatılıp açılsa bile kaybolmaz. Veriler yalnızca o tarayıcıda kalır,
  hiçbir sunucuya gönderilmez.
- **Harita**: Türkiye sınırı veya şehir içi ızgara üzerinde duraklar,
  planlanan rota(lar) ve (varsa) depoya dönüş çizgisi SVG olarak çizilir;
  gerçek yol verisi açıkken güzergâh gerçek karayolu şekliyle çizilir.
- **Adres defteri**: Şehir içi modda "konumu öğret" ile bir alıcının gerçek
  GPS konumu kaydedilir, bir sonraki seferde adı yazmak yeterli olur.
- **Sevkiyat listesi**: Araç başına sıra, etap/kümülatif km, tahmini varış
  saati, teslim durumu ve maliyet özeti; birden fazla araç varsa toplam
  (agregat) özet de gösterilir.

## Proje yapısı

```
www/index.html      — uygulamanın kendisi (HTML/CSS/JS, tek dosya)
capacitor.config.json — Capacitor (native sarmalama) yapılandırması
package.json         — Capacitor bağımlılıkları ve yardımcı script'ler
docs/IOS_BUILD.md     — iOS uygulaması olarak derleyip App Store'a
                        yükleme adımları
```

## Kullanım (web)

Depoyu klonlayıp `www/index.html` dosyasını bir tarayıcıda açmak yeterli;
herhangi bir derleme adımı veya bağımlılık yok. "Gerçek yol verisi kullan"
seçeneği kapalıyken tamamen çevrimdışı çalışır. Yerel bir sunucuyla
denemek için:

```bash
npm install
npm run serve   # http://localhost:8080
```

## iOS uygulaması olarak derleme

Uygulama [Capacitor](https://capacitorjs.com/) ile native bir iOS kabuğuna
sarılabilir (GPS izinleri native akışa bağlanır, App Store'a yüklenebilir
hale gelir). Adım adım talimatlar için **[docs/IOS_BUILD.md](docs/IOS_BUILD.md)**
dosyasına bakın — bu adımlar bir Mac + Xcode + Apple Developer Program
üyeliği gerektirir.
