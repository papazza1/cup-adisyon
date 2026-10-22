# C'up Adisyon

Clouds Up (C'up) için iPad'de çalışan adisyon uygulaması: masa planı, sipariş, ödeme, masa aktarma (basılı tut + sürükle),
gider girişi ve günlük / haftalık / aylık / yıllık gelir-gider raporları.

- Adres: https://papazza1.github.io/cup-adisyon/
- iPad'e kurulum: Safari'de adresi aç → Paylaş → **Ana Ekrana Ekle**. Ana ekrandaki C'up simgesinden tam ekran açılır.
- Veriler iPad'in kendisinde (IndexedDB, `cup-adisyon/docs`; eski localStorage verisi ilk açılışta taşınır) tutulur, sunucuya gönderilmez. **Menü → Yedek** bölümünden düzenli yedek alın.
- İnternet olmadan da açılır (service worker önbelleği).

## Geliştirme notları
- Tek dosya: `index.html` (HTML + CSS + JS, kütüphane yok). Veri katmanı `memoryStore()`; Firestore benzeri `doc/collection` arayüzü.
- `index.html` değişince `sw.js` içindeki `VERSION` değerini artırın, iPad'ler güncellemeyi alsın.
- Veri modeli: `config/menu`, `config/settings`, `tables/<masa>`, `days/<YYYY-MM-DD>` (bills, exp), `summ/<YYYY-MM-DD>` (günlük özet).

## Stok
- `stock/items` → `{seeded, items:[{id,name,cat,unit(cl|ml|g|adet),pack,packName,qty,full,alert,alerted}]}`; `stock/log` → son 400 hareket.
- Menü ürünlerinde `use:[{s:stokId,q:miktar}]` reçetesi. Hesap kapanınca (ayrı ödemede her parçada) düşülür, adisyonda `stk` olarak saklanır; iptalde geri eklenir.
- Yüzde = qty / full. `full` her girişte yeni miktara eşitlenir. Eşik (varsayılan %20) geçilince bildirim + başlıkta "Stok: n azalan".

## Rapor şifresi ve Sahne
- Raporlar ve başlıktaki "Bugün" cirosu ayrı 6 haneli şifreyle korunur (`config/replock`). İlk açılışta belirlenir; sekmeden çıkınca veya 5 dk boşta kalınca kilitlenir.
- `SAHNE` masası satış değildir: "Gidere aktar" ile kapanır, ciroya girmez; ürünler stoktan düşer ve `days/<tarih>.exp` içine `cat:"Sahne"` gideri yazılır (tutar düzenlenebilir, varsayılan menü fiyatı). Gider silinirse stok geri eklenir.
 

## Güvenlik notları
- Masayı **Boşalt** ve adisyon **iptal** yönetici şifresi ister; bu şifre girildikten sonra Menü/Gider/Stok kendiliğinden açık kalmaz.
- Sayımı yapılmamış stok kalemi satışta eksiye düşürülmez.
- Sıfırlama yok. Menü → Yedek: **Tam yedek** (her şey) ve **Yıllık yedek** (seçilen yılın satış/giderleri + menü/ayarlar, şifresiz). Yıllık yedek geri yüklenince mevcut veriye eklenir, hiçbir şeyi silmez.
