# Sadece Sayılar mı var?

Racket dilinin yapı taşları arasında sayılardan başka `türler` de var. Örneğin:

| Signature | Tür | Örnekler  |
|:---|---|---|
| number | Sayılar | 0, 1983, -42, 3.14, 1/2,  &Sqrt;2&#x305;  | 
| string | Metinler | "Fatih", "x", " " |
| boolean | Mantıksal değerler (doğru, yanlış) | #t, #f  |
| image | Resimler | &#x2730;, &#9633; |

Yukarıdaki tabloda gördüğünüz örneklere programlama dillerinde `Literals` ya da `Constants` denir. Yani yapıtaşları, en basit değerler. Daha fazla basitleştirilemezler.

## string (Metinler)

`racket` programlama dilinde (ve daha bir çok dilde) tırnak işareti (`"`) ile başlayıp biten karakter dizilerine `string` denir. Örneğin:

```racket
"Fatih"
"Merhaba Dünya"
" "
```

`string` türünden değerler kullanarak da hesaplamalar yapabiliriz:

```racket
> (string-append "Merhaba" "Dünya")
"MerhabaDünya"

> (string-length "Selam")
5

> (string? "42")
#t
```
## boolean (Mantıksal ifadeler)

Programlamada sık kullanılan diğer bir tür de `boolean` diye adlandırdığımız mantıksal ifadelerdir. Sadece 2 adet `boolean` değer vardır: `#t` (True) ve `#f` (False).

```racket
#t
#f
```
`boolean` değerler kullanarak yaptığımız hesaplamalara örnek:

```racket
> (and #t #f)
#f

> (or #t #f)
#t

> (> 42 14)
#t
```

## image (Resimler)

`racket`'da bulabileceğiniz bir başka değer ise `image` (Resim). Aşağıdaki ifadeyi çalıştırırsanız size 50 yarıçapında, içi dolu, kırmızı bir daire resmi üretir.

```racket
(circle 50 "solid" "red")
```
<img src="static/circle.png">


> İPUCU: DrRacket etkileşim penceresinde `ESC+P` tus kombinasyonu ile en son yazdığınız satırı geri çağırabilirsiniz.


```
(rectangle 100 50 "outline" "blue")
```

<img src="static/rectangle.png">

`image` türünden değerler kullanarak yapabileceğimiz işlemlere örnek olarak:

```racket
(above (circle 20 "solid" "red") (rectangle 100 50 "solid" "blue"))
(overlay (circle 30 "solid" "red") (rectangle 200 100 "outline" "black"))
```

Bknz: [circle](../misc/documentation.md#circle), [rectangle](../misc/documentation.md#rectangle), [above](../misc/documentation.md#above), [overlay](../misc/documentation.md#overlay), [overlay/xy](../misc/documentation.md#overlayxy)
