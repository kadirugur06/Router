# Router — Gizlilik Politikası

*Son güncelleme: 1 Eylül 2026*

Router ("uygulama"), sevkiyat/teslimat rotalarını planlamanıza yardımcı
olan bir araçtır. Bu belge, uygulamanın hangi verileri nasıl kullandığını
açıklar.

## Hangi veriler kullanılır?

- **Konum (GPS)**: Depo veya teslimat durağı konumunuzu almak istediğinizde,
  cihazınızın konum servislerini kullanır. Bu izin isteğe bağlıdır —
  konum yerine adres/şehir seçerek veya koordinatı elle girerek de
  uygulamayı kullanabilirsiniz.
- **Girdiğiniz veriler**: Durak listesi, araç/şoför bilgileri, adres
  defteri kayıtları ve ayarlarınız.
- **Abonelik/satın alma durumu**: "Router Pro" aboneliğine sahip olup
  olmadığınızı doğrulamak için Apple App Store'un satın alma kaydı,
  RevenueCat üzerinden okunur (aşağıya bakın).

## Bu veriler nerede saklanır?

Durak listesi, araç/şoför bilgileri, adres defteri ve ayarlarınız
**yalnızca cihazınızda** (yerel depolama / `localStorage`) saklanır.
Router, bu veriler için bir sunucu çalıştırmaz, toplamaz veya bir
veritabanına kaydetmez. Abonelik durumu ise Apple'ın App Store'unda ve
RevenueCat'in sunucularında (aşağıdaki "Üçüncü taraf servisler"
bölümüne bakın) tutulur — bu, satın almanın geçerliliğini doğrulamak
için teknik olarak gereklidir.

## Üçüncü taraf servisler

- **OSRM (rota servisi)**: "Gerçek yol verisi kullan" seçeneği (Router Pro
  özelliği) açıldığında, planladığınız durakların **yalnızca
  koordinatları** — isim, telefon veya başka bir kimliklendirici bilgi
  olmadan — açık kaynaklı [OSRM](https://project-osrm.org/) rota servisine
  (`router.project-osrm.org`) gönderilir; karşılığında mesafe ve güzergâh
  bilgisi alınır. Bu seçenek kapalıyken bu veri hiçbir sunucuya
  gönderilmez.
- **RevenueCat (abonelik yönetimi)**: "Router Pro" aboneliğiniz varsa,
  Apple App Store'un ürettiği anonim bir satın alma/kullanıcı kimliği
  (isim, e-posta veya telefon numaranız **değil**) [RevenueCat](https://www.revenuecat.com/)
  servisine iletilir; bu servis aboneliğinizin aktif olup olmadığını
  doğrular ve uygulamanın kilidini buna göre açar. RevenueCat'in kendi
  gizlilik politikasına [revenuecat.com/privacy](https://www.revenuecat.com/privacy)
  adresinden ulaşabilirsiniz. Ücretsiz sürümü kullanıyorsanız bu veri
  hiç oluşmaz.

## Veri paylaşımı ve takip

Router; verilerinizi reklam veya davranışsal takip amacıyla **kullanmaz,
satmaz veya paylaşmaz**. Uygulama içinde herhangi bir reklam SDK'sı
bulunmamaktadır. RevenueCat, yalnızca "Router Pro" aboneliğinizin
geçerliliğini doğrulamak için kullanılır — reklam veya pazarlama amaçlı
bir profil oluşturmaz.

## Verilerinizi silme

Uygulama içindeki "Listeyi boşalt" ve ilgili silme düğmeleriyle
verilerinizi istediğiniz zaman silebilirsiniz; uygulamayı cihazınızdan
kaldırmak da tüm yerel verileri temizler.

## İletişim

Sorularınız için: kadirugur06@gmail.com
