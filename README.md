# C'up Adisyon

Clouds Up (C'up) için iPad'de çalışan adisyon uygulaması: masa planı, sipariş, ödeme, masa aktarma (basılı tut + sürükle),
gider girişi ve günlük / haftalık / aylık / yıllık gelir-gider raporları.

- Adres: https://papazza1.github.io/cup-adisyon/
- iPad'e kurulum: Safari'de adresi aç → Paylaş → **Ana Ekrana Ekle**. Ana ekrandaki C'up simgesinden tam ekran açılır.
- Veriler iPad'in kendisinde (localStorage) tutulur, sunucuya gönderilmez. **Menü → Yedek** bölümünden düzenli yedek alın.
- İnternet olmadan da açılır (service worker önbelleği).

## Geliştirme notları
- Tek dosya: `index.html` (HTML + CSS + JS, kütüphane yok). Veri katmanı `memoryStore()`; Firestore benzeri `doc/collection` arayüzü.
- `index.html` değişince `sw.js` içindeki `VERSION` değerini artırın, iPad'ler güncellemeyi alsın.
- Veri modeli: `config/menu`, `config/settings`, `tables/<masa>`, `days/<YYYY-MM-DD>` (bills, exp), `summ/<YYYY-MM-DD>` (günlük özet).
