# Router — Gizlilik Politikası

*Son güncelleme: [tarih girin]*

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

## Bu veriler nerede saklanır?

Tüm veriler **yalnızca cihazınızda** (yerel depolama / `localStorage`)
saklanır. Router, bir sunucu çalıştırmaz ve kullanıcı verilerini
toplamaz, bir veritabanına kaydetmez veya üçüncü taraflarla paylaşmaz.

## Üçüncü taraf servisler

"Gerçek yol verisi kullan" seçeneği açıldığında, planladığınız durakların
**yalnızca koordinatları** — isim, telefon veya başka bir kimliklendirici
bilgi olmadan — açık kaynaklı [OSRM](https://project-osrm.org/) rota
servisine (`router.project-osrm.org`) gönderilir; karşılığında mesafe ve
güzergâh bilgisi alınır. Bu seçenek kapalıyken uygulama tamamen
çevrimdışı çalışır ve hiçbir veri hiçbir sunucuya gönderilmez.

## Veri paylaşımı ve takip

Router; verilerinizi reklam, analitik veya takip amacıyla **kullanmaz,
satmaz veya paylaşmaz**. Uygulama içinde herhangi bir üçüncü taraf
analitik veya reklam SDK'sı bulunmamaktadır.

## Verilerinizi silme

Uygulama içindeki "Listeyi boşalt" ve ilgili silme düğmeleriyle
verilerinizi istediğiniz zaman silebilirsiniz; uygulamayı cihazınızdan
kaldırmak da tüm yerel verileri temizler.

## İletişim

Sorularınız için: [e-posta adresinizi girin]
