# Ders Notları

İlk dersimizde `+`, `odd?`, `string-append`, `circle` gibi daha önceden tanımlanmış fonksiyonları kullanarak bazı işler yapmaya çalıştık. Ancak henüz kendimiz bir fonksiyon tanımlamadık. Bu dersimizde `lambda` ifadesini kullanarak bir değişken alabilen fonksiyonların nasıl tanımlandığını öğreneceğiz.

<a href="https://www.youtube.com/watch?v=flDaxrP4e2w" target="_blank"><img src="ders2.png"></a>


## Fonksiyonlar (Prosedürler): lambda

Şimdi birlikte elektrik faturamızı hesaplayabilmek için bir formül yazalım. Benim kullandığım elektrik şirketinin fiyatlandırma kuralı şu şekilde:

```racket
Kullanım Bedeli (Arbeitspreis) = 17,45 ct/kWh
Taban Fiyat (Grundpreis)       = 10,16 €/Monat
```

Bu şirket taban fiyat olarak aylık 10,16 Euro alıyor. Bunun üzerine kullandığımız her kWh elektrik için 17.45 Cents ödüyoruz. Matematiksel olarak ifade etmek gerekirse;

```
Aylık Fatura = 10.16 + ((17.45 * KULLANIM-MIKTARI) / 100))
```

Örneğin, aylık 350 kWh elektrik kullanıldığında;

```
17.45 * 350 = 6107.5 Cents
6107.5 Cents / 100 = 61.075 Euro
61.075 + 10.16 = 71.235 Euro
```

Bunu `Racket` dilinde yazalım:

```racket
(+ 10.16 (/ (* 17.45 KULLANIM-MIKTARI) 100))
```

Bu ifadeyi bu çalıştırabilmek için `KULLANIM-MIKTARI` yazan yere gerçek değerler vermemiz gerekecek. Örneğin, yine 350 kWh elektrik kullanıldığını varsayarsak:

```racket
(+ 10.16 (/ (* 17.45 350) 100))
```

... sonucun 71.235 olduğunu görebiliriz.

Peki bu ifadeyi içinde değişken olacak şekilde kullanmak istesek? Yani kullanılan elektrik miktarı değiştikçe hesaplayabilen bir fonksiyon yazmak istesek. `lambda` fonksiyonu tam da bu iş için var:

```racket
(lambda (...) ...)
```

`lambda` birinci parametrede verdiğimiz değişken isimlerini ikinci parametredeki ifadece yerine koyarak bize bir fonksiyon (prosedür) geri döner.

```racket
(lambda (x) (+ x 1))
```

Üretilen bu fonksiyona `define` kullanrak bir isim verebiliriz.

```racket
(define bir-ekle (lambda (x) (+ x 1)))
```

Örneğimize geri dönecek olursak:

```racket
(define fatura-hesapla
  (lambda (x)
    (+ 10.16 (/ (* 17.45 x) 100))))
```

## Örnek Fonksiyonlar

Bir sayı alan ve verilen sayının karesini hesaplayan bir fonksiyon yazalım:

```racket
(define karesi
  (lambda (x)
    (* x x)))
```

Dikdörtgenin yüksekliği ve genişliğini temsil eden 2 sayı alan ve dikdörtgenin alanını hesaplayan bir fonksiyon yazalım.

```racket
(define dikdortgen-alani
  (lambda (genislik yukseklik)
    (* genislik yukseklik)))
```

Roketin x koordinatini temsil eden bir sayi alan ve roketi 200x200 boyutlarında boş bir arka plan üzerine (x, 100) noktasina yerleştiren bir fonksiyon yazalım.

```racket
(define roket (circle 20 "solid" "red"))
(define arka-plan (empty-scene 200 200))

(define giden-roket
  (lambda (x)
     (place-image roket x 100 arka-plan)))
```

`Language > Add Teackpack` menüsünden `universe.ss` isimli eğitim paketini yüklerseniz (`Run` butonuna basmayı unutmayalım), yazdığınız `giden-roket` programını bir anımasyon haline getirebilirsiniz.

```racket
(animate giden-roket)
```