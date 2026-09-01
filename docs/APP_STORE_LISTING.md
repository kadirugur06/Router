# App Store Connect metinleri (taslak)

Bu dosya App Store Connect'teki "App Information" ve sürüm (version)
sayfalarına kopyala-yapıştır için hazırlanmış taslak metinleri içerir.
Karakter sınırlarını Apple'ın kuralları belirliyor — aşağıda her alan için
sınır not edildi.

## Uygulama adı (30 karakter)

```
Router: Rota Planlayıcı
```

## Alt başlık / Subtitle (30 karakter)

```
Sevkiyat Rota Optimizasyonu
```

## Tanıtım metni / Promotional Text (170 karakter — sürüm yayınlamadan güncellenebilir)

```
Duraklarınızı ekleyin, Router en kısa rotayı saniyeler içinde bulsun.
Çoklu araç desteği, gerçek yol verisi ve anlık yakıt tasarrufu hesabı.
```

## Açıklama / Description (4000 karakter)

```
Router, dağıtım ve sevkiyat rotalarınızı planlayan basit ama güçlü bir
araçtır. Teslimat noktalarınızı ekleyin, Router en verimli sırayı
hesaplasın — elle sıralamaya kıyasla kazandığınız yol, süre ve yakıtı
anında görün.

ÖZELLİKLER

• Akıllı rota optimizasyonu
Router, en yakın komşu ve 2-opt algoritmalarını kullanarak durakları en
kısa sırayla dizer. Elle sıraladığınız tur ile karşılaştırıp kaç km, kaç
saat ve kaç TL yakıt tasarrufu sağladığınızı gösterir.

• Şehirler arası veya şehir içi
81 il arasında şehirler arası dağıtım planlaması yapın, ya da GPS/adres
tabanlı şehir içi teslimatlarınızı optimize edin.

• Çoklu araç ve filo desteği
Birden fazla araç/şoför tanımlayın; duraklar kapasiteye duyarlı şekilde
otomatik bölüştürülür. İsterseniz bir durağı elle başka bir araca
taşıyın. Her araç için ayrı optimize rota, haritada farklı renk ve kendi
sevkiyat listesi.

• Gerçek yol verisi (opsiyonel)
Açık kaynaklı OSRM servisiyle kuş uçuşu tahmini yerine gerçek karayolu
mesafesi ve güzergâhı kullanın.

• Adres defteri
Şehir içi teslimatlarda "konumu öğret" ile bir alıcının gerçek GPS
konumunu bir kere kaydedin, sonraki seferlerde sadece adını yazın.

• Verileriniz cihazınızda kalır
Durak listeniz, araçlarınız ve ayarlarınız yalnızca cihazınızda saklanır;
hiçbir sunucuya gönderilmez.

Router; kurye, lojistik, toptan dağıtım ve saha satış ekipleri için
günlük rota planlamasını basitleştirmek üzere tasarlandı.
```

## Anahtar kelimeler / Keywords (100 karakter, virgülle ayrılmış, boşluksuz)

```
lojistik,teslimat,kamyon,filo,rota,navigasyon,optimizasyon,dağıtım,şoför,vrp,kurye,sevkiyat
```

*(100 karakteri geçmeyecek şekilde kısaltmanız gerekebilir — App Store
Connect otomatik uyarır.)*

## Kategori önerisi

- Birincil: **Business** (İş)
- İkincil (opsiyonel): **Navigation** (Navigasyon)

## Yaş derecelendirmesi

Uygulama şiddet, yetişkin içerik vb. barındırmıyor — App Store Connect'teki
içerik anketinde tüm soruları "Yok/None" olarak işaretlemek 4+ derecesini
verecektir.

## What's New (ilk sürüm için)

```
İlk sürüm: rota optimizasyonu, çoklu araç desteği, gerçek yol verisi
(OSRM) ve adres defteri ile sevkiyat planlaması.
```

## Destek URL'si ve Gizlilik Politikası URL'si (zorunlu alanlar)

Apple, App Store Connect'te hem bir **Support URL** hem de bir **Privacy
Policy URL** istiyor. Bu depoda bir gizlilik politikası taslağı
`docs/PRIVACY_POLICY.md` olarak hazır — GitHub Pages ile ücretsiz
yayınlayabilirsiniz:

1. GitHub'da bu reponun **Settings → Pages** bölümüne gidin.
2. Source olarak `main` branch + `/docs` klasörünü seçin.
3. Birkaç dakika sonra `https://<kullanıcı-adınız>.github.io/Router/PRIVACY_POLICY.html`
   gibi bir adres aktif olur (Markdown yerine `.html` istiyorsanız
   `docs/PRIVACY_POLICY.md` dosyasını `.html`'e çevirebiliriz — isterseniz
   bunu da hazırlarım).
4. Bu adresi App Store Connect'teki **Privacy Policy URL** alanına yapıştırın.
5. **Support URL** için aynı sayfayı ya da bir e-posta adresi/iletişim
   sayfası kullanabilirsiniz.
