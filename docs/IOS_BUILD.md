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

`resources/icon.png` (1024×1024) ve `resources/splash.png` (2732×2732)
depoda hazır olarak geliyor — beğenmezsen aynı isim/boyutla kendi
dosyalarınla değiştirebilirsin. Sonra:

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

1284×2778 px boyutunda ekran görüntüleri gerekiyor (App Store Connect'in
6.5"/6.7" iPhone sekmesinin kabul ettiği kesin ölçü). `store/screenshots/`
klasöründe doğrudan App Store Connect'e yüklenebilecek 4 hazır görüntü
var — 3'ü temel akışı, 4.'sü (`store-4-router-pro.png`) Router Pro
abonelik/paywall ekranını gösteriyor. **Not:** bu 4. görüntüdeki
"₺99,99/ay" fiyatı bir yer tutucudur — App Store Connect'te gerçek
aboneliğin fiyatını belirledikten sonra bu görseli o fiyatla yeniden
üretip güncellemeniz gerekir (aksi halde Apple, ekran görüntüsü ile
gerçek fiyat arasındaki tutarsızlık nedeniyle reddedebilir). İstersen
simülatörde `Cmd+S` ile kendi görüntülerini de alabilirsin. Uygulama adı, alt başlık, açıklama,
anahtar kelimeler ve kategori önerisi için **`docs/APP_STORE_LISTING.md`**
dosyasına bakın — kopyala-yapıştıra hazır. Gizlilik politikası taslağı
için **`docs/PRIVACY_POLICY.md`** dosyasına ve `APP_STORE_LISTING.md`
içindeki GitHub Pages ile yayınlama adımlarına bakın (App Store Connect
bir Privacy Policy URL'si zorunlu kılıyor).

## 8) Router Pro aboneliği (RevenueCat + App Store Connect)

Router artık bir **ücretsiz sürüm + aylık "Router Pro" aboneliği** modeliyle
çalışıyor:

- **Ücretsiz**: günlük en fazla `FREE_STOP_LIMIT` (varsayılan **5**) durak,
  tek araç, kuş uçuşu mesafe hesabı.
- **Router Pro**: sınırsız durak, çoklu araç/şoför desteği, gerçek yol
  verisi (OSRM).

Bu kısıtlamalar `www/index.html` içinde uygulama tarafında (client-side)
uygulanıyor; satın alma/abonelik durumunu ise [RevenueCat](https://www.revenuecat.com/)
üzerinden App Store'un StoreKit altyapısına bağlıyoruz — makbuz doğrulama,
abonelik yenileme/iptal takibi gibi karmaşık kısımları RevenueCat hallediyor.
Ücretsiz planı aylık ~2.500$ gelire kadar kendisi de ücretsiz.

### 8.1) App Store Connect'te abonelik ürününü oluştur

1. App Store Connect'te uygulamana git → soldaki menüden **Monetization →
   Subscriptions** (ya da "Features" altında, ASC sürümüne göre değişebilir).
2. **+** ile yeni bir **Subscription Group** oluştur, örn. adı: `Router Pro`.
3. Grup içinde **+** ile yeni bir abonelik ürünü ekle:
   - **Reference Name**: `Router Pro Aylık` (sadece senin göreceğin etiket)
   - **Product ID**: `com.kadirugur.router.pro.monthly` (bunu değiştirirsen
     RevenueCat tarafında da aynısını kullan)
   - **Subscription Duration**: 1 Month
   - **Price**: kendi belirleyeceğin fiyatı seç (App Store Connect fiyat
     tablosundan — otomatik olarak tüm ülke/para birimlerine çevrilir)
   - **Localizations**: en az Türkçe için görünen ad + açıklama gir
     (örn. "Router Pro" / "Sınırsız durak, çoklu araç ve gerçek yol verisi")
   - **Review Screenshot**: App Review'a abonelik ekranının nasıl göründüğünü
     gösteren bir ekran görüntüsü yükle — uygulamadaki paywall penceresinin
     (Router Pro rozetine tıklayınca açılan pencere) bir görüntüsünü kullan.
4. Kaydet.

### 8.2) RevenueCat hesabı kur

1. [app.revenuecat.com](https://app.revenuecat.com) üzerinden ücretsiz bir
   hesap aç, yeni bir **Project** oluştur (örn. "Router").
2. **Apps** → **+ New** → **App Store** seç, Bundle ID'yi
   (`com.kadirugur.router`) gir.
3. App Store Connect ile bağlantı için bir **App-Specific Shared Secret**
   gerekiyor: App Store Connect'te uygulamana git → **App Information** →
   **App-Specific Shared Secret** → **Manage** → oluştur, kopyala, RevenueCat'e
   yapıştır.
4. **Products** sekmesinde **+ New** ile 8.1'de oluşturduğun Product ID'yi
   (`com.kadirugur.router.pro.monthly`) ekle.
5. **Entitlements** sekmesinde `pro` adında bir yetki (entitlement) oluştur
   (kod bu ismi bekliyor — `RC_ENTITLEMENT_ID` sabiti) ve az önce eklediğin
   ürünü buna bağla.
6. **Offerings** sekmesinde bir "Current" offering oluştur, içine bir Package
   ekleyip (Package type: Monthly) ürünü ona bağla.
7. **API Keys** sekmesinde **Public app-specific API key**'i (iOS) kopyala.

### 8.3) API anahtarını uygulamaya ekle

`www/index.html` içinde şu satırı bul:

```js
const RC_API_KEY_IOS="";
```

Çift tırnak arasına RevenueCat'ten kopyaladığın Public API Key'i yapıştır,
kaydet, sonra:

```bash
npm install
npx cap sync ios
```

(`npm install`, `@revenuecat/purchases-capacitor` paketini indirir;
`npx cap sync ios` onu Xcode projesine native tarafta bağlar.)

`RC_API_KEY_IOS` boş bırakıldığı sürece uygulama satın alma denemez, sadece
ücretsiz sürüm olarak çalışır — yani bu adımı atlarsan uygulama yine de
bozulmaz, sadece Pro'ya geçiş devre dışı kalır.

### 8.4) Sandbox'ta test et

1. App Store Connect → **Users and Access → Sandbox → Testers**'da bir test
   Apple ID'si oluştur (gerçek e-postan olmasına gerek yok, sahte bir tane
   yeter).
2. Test cihazında (simülatörde de çalışır) **Ayarlar → App Store → Sandbox
   Hesabı**'na bu test kullanıcısıyla giriş yap.
3. Xcode'dan uygulamayı çalıştır, sağ üstteki **Router Pro** rozetine tıkla,
   abone ol butonuna bas — fiyatın yanında "[Environment: Sandbox]" gibi bir
   not göreceksin, gerçek para çekilmez.
4. Satın alma sonrası uygulamanın kilidi açılmalı (durak sınırı kalkar,
   ikinci araç eklenebilir, "Gerçek yol verisi" işaretlenebilir). Uygulamayı
   silip tekrar kurup **Satın almaları geri yükle** butonunu da test et.

### 8.5) App Review notları (abonelik)

- **Restore Purchases** butonu zaten paywall ekranında var — Apple bunu
  abonelik içeren her uygulamada zorunlu kılıyor.
- Paywall ekranı satın almadan önce fiyatı, süreyi (aylık) ve neyin açıldığını
  (özellik listesi) gösteriyor — Guideline 3.1.2 bunu istiyor.
- **App Information → License Agreement** alanında "Apple's Standard License
  Agreement" seçili kalmalı — bu, otomatik yenilenen abonelikler için
  gereken standart şartları zaten içeriyor, ayrı bir EULA yazmana gerek yok.
- **App Privacy** anketine, aboneliği RevenueCat üzerinden işlediğin için
  **Purchase History** (Satın Alma Geçmişi) veri türünü de eklemen gerekiyor
  — App Store Connect → App Privacy → Data Types → Edit'ten ekleyip
  kullanım amacı olarak "App Functionality" işaretle, kullanıcı kimliğiyle
  ilişkilendirilip ilişkilendirilmediğini RevenueCat'in sana ilettiği
  müşteri kimliği (anonim bir ID, e-posta/isim değil) üzerinden "Hayır" (No)
  olarak işaretleyebilirsin.
- `docs/PRIVACY_POLICY.md` ve `docs/privacy-policy.html` dosyaları
  RevenueCat'in bir üçüncü taraf servis olarak abonelik/satın alma verisini
  işlediğini zaten belirtiyor — fiyatı değiştirirsen ayrıca bir şey
  güncellemen gerekmiyor.

## 9) İnceleme (App Review) notları

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
