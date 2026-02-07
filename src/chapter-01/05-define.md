# İfadelerin İsimlendirilmesi: define

Yazdığımız ifadelere bir isim vererek onları daha sonra kullanabiliriz:

```racket
(define pi 3.141592653)
(define fatih-tel "+49 111222333")
(define mavi-daire (circle 100 "solid" "blue"))
(define gün-dakika (* 24 60))
```

`(define ... ...)` özel bir form. Bu form hesaplanan bir şey değil, bir efekti var. İkinci parametredeki ifadeyi birinci parametredeki isme bağlıyor.

```racket
(define isim ifade)
```

> Kural:
> isimler ( ) [ ] { } " , ' ` ; # | \ karakterleri ile başlayamaz.

> Kural: 
> isimler bir sayı ile eşit olamaz

> Kural:
> İsimler space, tab, return karakterleri içeremez.

> Kural:
> daha önce başka bir değere bağlanmış olmamalı.

> Kural: Büyük/küçük karakter farketmiyor.

Aşağıdaki örneğin yukarıdaki kurallara uyup uymadığına inceleyelim:

```racket
(define eu->us$ 1.02)
```