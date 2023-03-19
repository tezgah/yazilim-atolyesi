# Ev Ödevi

1. Racket dili tarafından tanımlanmış [circle](../misc/documentation.md#circle), [rectangle](../misc/documentation.md#rectangle), [star](../misc/documentation.md#star), [overlay](../misc/documentation.md#overlay) ve [overlay/xy](../misc/documentation.md#overlayxy) gibi fonksiyonları kullanarak Türk Bayrağımızı çizebilir misiniz?

2. Derste yaptığımız Alman Bayrağı'nın gerçek oranı 3/5'tır. Yani genişliği 5 birim ise yüksekliği 3 birimdir. Dolayısı ile her bir rengi oluşturan dikdörtgenlerin yükseklikleri 1'er birimdir. Öyle bir Alman Bayrağı yapalım ki `genişlik` değerini kaç verirsek geri kalan ölçüleri ona göre ayarlasın ve böylece istediğimiz boyutta bir bayrak çizebilelim.

Örnek:

```
(define genişlik 500)
```

Eğer genişlik değerini 500 olarak tanımlarsak, bayrağın yüksekliği 300 (genişlik \* 3/5) olur. Renkleri oluşturan dikdörtgenler de 500'e 100 (500 / 5) olur.

```
(define genişlik 600)
```

Eğer genişlik değerini 600 olarak tanımlarsak, bayrağın yüksekliği 200, dikdörtgenlerin boyutları ise 600'e 120 olur.

Bu şekilde genişlik değeri değiştikçe boyutu değişen (ancak oranları sabit olan) bir Alman Bayrağı çizebilir misin?

3. İkinci ödevde yaptığımız gibi aynı şekilde genişlik değiştikçe boyutu değişen, ancak oranları bozulmayan bir Türk bayrağı çizebilir misin? Wikipedia sitesindeki Türk Bayrağı'nun boyutları ile ilgili yeterli bilgiyi bulabilirsin.

https://tr.wikipedia.org/wiki/T%C3%BCrk_bayra%C4%9F%C4%B1
