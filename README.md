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
- **Harita**: Türkiye sınırı veya şehir içi ızgara üzerinde duraklar,
  planlanan rota ve (varsa) depoya dönüş çizgisi SVG olarak çizilir.
- **Adres defteri**: Şehir içi modda "konumu öğret" ile bir alıcının gerçek
  GPS konumu kaydedilir, bir sonraki seferde adı yazmak yeterli olur.
- **Sevkiyat listesi**: Sıra, etap/kümülatif km, tahmini varış saati, teslim
  durumu ve araç/yakıt maliyeti özetini içeren manifest tablosu.

## Kullanım

Depoyu klonlayıp `index.html` dosyasını bir tarayıcıda açmak yeterli;
herhangi bir derleme adımı veya bağımlılık yok. Tüm veriler yalnızca o
oturumda (tarayıcı belleğinde) tutulur, sunucuya kaydedilmez.
