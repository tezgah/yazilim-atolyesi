# Ev Ödevi

`big-bang` kullanarak 200x200 boyutunda bir sahnede yanyana ve aşağı doğru farklı ivmelerde hareket ederek yere çarptıklarında tekrar yukarı doğru geri zıplayan 2 tane top similasyonu yapalım. Toplardan biri mavi diğeri kırmızı olsun. Toplar ilk hareketlerine tepe noktasından (y=0) başlasınlar ve farklı ivmelerde hareket etsinler. En alt noktaya eriştiklerinde (y=200) tekrar geri zıplasınlar.

İlk önce bu iş için bize gerekli olan minimal veri yapısını belirleyelim: Toplam kaç adet top olacak? Bu toplar her iki koordinatta mı yoksa sadece tek bir koordinatta mı hareket ediyorlar? Toplar aynı hızda mı hareket ediyorlar? Bu bilgileri kullanarak `top` için bir veri yapısı tasarlayalım.

Her saat vurduğunda dünyamız nasıl değişecek? Topların bir sonraki hareketi nasıl olacak? Toplardan biri en alt noktaya ulaşırsa geri sekmesini nasıl sağlayacağız? Bu gibi durumları göz önünde bulundurarak bir `tick` fonksiyonu yazalım.

Topların görüntüsünü sahneye nasıl çizeceğiz? `draw` fonksiyonunu yazarak 200x200'luk boş bir sahneye iki topu birden çizelim.

`big-bang` fonksiyonuna dünyamızın başlangıç durumunu, `on-tick` için yazdığımız `tick` fonksiyonunu ve `to-draw` için yazdığımız `draw` fonksiyonunu vererek similasyonumuzu çalıştıralım. 

```
(big-bang
 ...
 (on-tick tick)
 (to-draw draw)
)
```