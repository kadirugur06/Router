# Router'ı iOS uygulaması olarak derleyip App Store'a yükleme

Bu depo artık bir [Capacitor](https://capacitorjs.com/) projesi olarak
organize edildi: `www/index.html` web uygulamasının kendisi, geri kalan
dosyalar (`package.json`, `capacitor.config.json`) onu native bir iOS
(ve istersen Android) kabuğuna sarmak için gerekli yapılandırma.

Aşağıdaki adımların tamamı **bir Mac üzerinde, Xcode kurulu olarak**
yapılmalı — iOS derlemesi/imzalama/App Store'a yükleme Linux veya
Windows'ta yapılamıyor.

## 0) Ön koşullar

- macOS + [Xcode](https://apps.apple.com/app/xcode/id497799835) (App Store'dan) + Xcode Command Line Tools
- [CocoaPods](https://cocoapods.org/): `sudo gem install cocoapods` (veya `brew install cocoapods`)
- [Node.js](https://nodejs.org/) (18+) ve npm
- Aktif bir **Apple Developer Program** üyeliği (yıllık 99$) — App Store'a
  yüklemek ve gerçek cihazda test etmek için gerekli

## 1) Bağımlılıkları kur

```bash
git clone <bu repo> router
cd router
npm install
```

## 2) iOS platformunu ekle

```bash
npx cap add ios
npx cap sync ios
```

Bu komut `ios/` klasörünü oluşturur (Xcode projesi + CocoaPods bağımlılıkları).
`ios/` klasörünü commit etmek isteğe bağlıdır ama tavsiye edilir (ekip
içinde tutarlı bir Xcode projesi için).

## 3) Uygulama kimliği (Bundle ID) ve adını ayarla

`capacitor.config.json` içindeki `appId` (`com.kadirugur.router`) **App
Store Connect'te kaydettiğin Bundle ID ile birebir aynı olmalı**. Kendi
Apple Developer hesabındaki takım/organizasyon adına göre değiştir, örn.
`com.senintakimin.router`. Değiştirdikten sonra tekrar senkronize et:

```bash
npx cap sync ios
```

`appName` alanı ("Router") uygulamanın görünen adı — istersen değiştir.

## 4) İkon ve açılış ekranı (splash screen)

1024×1024 boyutunda tek bir `icon.png` ve bir `splash.png` (2732×2732
önerilir) dosyasını `resources/` klasörüne koy, sonra:

```bash
npx @capacitor/assets generate --ios
```

Bu, gerekli tüm iOS ikon/splash boyutlarını otomatik üretip Xcode
projesine yerleştirir.

## 5) Konum izni metni (Info.plist)

Uygulama GPS kullanıyor (depo/durak konumu, "konumu öğret"), bu yüzden
Apple kullanıcıya gösterilecek bir açıklama metni istiyor. Xcode'da
`ios/App/App/Info.plist` dosyasına şu anahtarı ekle (Xcode arayüzünden
Info sekmesinden de eklenebilir):

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>Router, depo ve teslimat durağı konumunuzu rota planlamak için kullanır.</string>
```

## 6) Xcode'da aç, imzala, test et

```bash
npx cap open ios
```

Xcode açılınca:

1. Sol üstte **App** hedefini seç → **Signing & Capabilities** sekmesi →
   **Team** olarak kendi Apple Developer hesabını seç (otomatik imzalama
   açık kalabilir).
2. Bundle Identifier'ın `capacitor.config.json`'daki `appId` ile aynı
   olduğunu doğrula.
3. Üstte bir simülatör seçip **▶ (Run)** ile çalıştır; her kod
   değişikliğinden sonra `npx cap sync ios` çalıştırmayı unutma (web
   dosyalarını native projeye kopyalar).
4. Gerçek cihazda test etmek istersen cihazını bağlayıp onu hedef seç.

## 7) App Store Connect'e yükleme

1. [appstoreconnect.apple.com](https://appstoreconnect.apple.com) →
   **My Apps** → **+** → **New App**. Bundle ID olarak yukarıdaki `appId`'yi
   seç (önce [developer.apple.com](https://developer.apple.com) → Certificates,
   Identifiers & Profiles'ta bu Bundle ID'yi bir kere kaydetmen gerekebilir).
2. Xcode'da **Product → Archive** ile bir arşiv oluştur.
3. Organizer penceresinde **Distribute App → App Store Connect → Upload**.
4. Yükleme işlendikten sonra App Store Connect'te build'i TestFlight'a
   veya doğrudan bir sürüme bağla.

### Gizlilik bilgi formu (App Privacy)

App Store Connect uygulama sayfasında **App Privacy** bölümünü doldurman
gerekiyor. Router konum verisi topluyor, bu yüzdenkonum veri türünü
işaretle:

- **Veri türü**: Precise Location (Kesin Konum)
- **Kullanım amacı**: App Functionality (uygulama işlevselliği — rota
  hesaplama)
- **Kullanıcıyla ilişkilendirilmiyor**, **takip amaçlı kullanılmıyor**
  (uygulama konumu hiçbir sunucuya/analitik servisine göndermiyor;
  yalnızca cihazda hesaplama için kullanılıyor — "Gerçek yol verisi"
  açıksa yalnızca koordinatlar, kimlikle ilişkilendirilmeden, herkese açık
  OSRM rota servisine gönderiliyor)

### Ekran görüntüleri, açıklama, kategori

En az 6.7" (iPhone 15 Pro Max vb.) ekran görüntüleri gerekiyor. Uygulamayı
simülatörde çalıştırıp `Cmd+S` ile ekran görüntüsü alabilirsin, ya da bu
oturumda paylaştığım PNG'leri referans alabilirsin. Kategori olarak
"Business" veya "Utilities" uygun olur.

## 8) İnceleme (App Review) notları

- Apple, yalnızca bir web sitesini pencereye saran uygulamaları reddeder
  (Guideline 4.2 — Minimum Functionality). Bu proje artık native GPS izin
  akışı ve cihazda kalıcı depolama kullandığı için bu riski azaltıyor;
  yine de incelemeyi güçlendirmek istersen native paylaşım (Share sheet),
  push bildirim veya widget gibi ek native özellikler eklemeyi
  düşünebilirsin.
- "Gerçek yol verisi" özelliği genel bir internet servisine (OSRM demo
  sunucusu) bağlanıyor — App Review notlarına bunun ne olduğunu (açık
  kaynak, ücretsiz rota servisi) kısaca yazmak faydalı olur.

## Sorun giderme

- `pod install` hatası alırsan: `cd ios/App && pod install --repo-update`
- Kod değişikliği Xcode'da görünmüyorsa: `npx cap sync ios` çalıştırmayı
  unutmuş olabilirsin.
- `xcrun: error` gibi hatalar genelde Command Line Tools eksikliğinden
  kaynaklanır: `xcode-select --install`
