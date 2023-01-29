# Ders Notları

İlk dersimizde `+`, `odd?`, `string-append`, `circle` gibi daha önceden tanımlanmış fonksiyonları kullanarak bazı işler yapmaya çalıştık. Ancak henüz kendimiz bir fonksiyon tanımlamadık. Bu dersimizde `lambda` ifadesini kullanarak bir değişken alabilen fonksiyonların nasıl tanımlandığını öğreneceğiz.

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
(lambda ... ...)
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


```racket
;; Açıklama
;; Verilen sayının karesini hesaplar

;; Sözleşme
(: karesi (integer -> integer))

;; Testler
(check-expect (karesi 0) 0)
(check-expect (karesi 2) 4)
(check-expect (karesi -3) 9)

;; Kod
(define karesi
  (lambda (x)
    (* x x)))
```

```racket
;; Açıklama:
;; Taban fiyat 10.16 Euro ve kullanım bedeli 17.45 Cents/KWh olmak üzere,
;; verilen kullanım miktarı için aylık faturayı hesaplar

;; Sözleşme:
(: fatura-hesapla (rational -> rational))

;; Testler:
(check-within (fatura-hesapla 0) 10.16 0.01)

;; Kod:
(define fatura-hesapla
  (lambda (x)
    (+ 10.16 (/ (* 17.45 x) 100))))
```

```racket
;; Açıklama:
;; Yüksekliği ve genişliği verilen dikdörtgenin alanını hesaplar

;; Sözleşme
(: dikdortgen-alani (integer integer -> integer))

;; Testler:
(check-expect (dikdortgen-alani 1 1) 1)
(check-expect (dikdortgen-alani 1 2) 2)
(check-expect (dikdortgen-alani 3 5) 15)

;; Kod
(define dikdortgen-alani
  (lambda (genislik yukseklik)
    (* genislik yukseklik)))
```

```racket
(define roket (circle 20 "solid" "red"))
(define arka-plan (rectangle 200 200 "solid" "Medium Cyan"))

;; Açıklama:
;; Verilen x koordinatına göre rocket resmini arka-planda gösterir.
;; y koordnatı arka-planın ortası olarak sabit kalır.

;; Sözleşme
(: giden-roket (integer -> image))

;; Testler:
(check-expect (giden-roket 30) (overlay/xy roket -30 -100 arka-plan))
(check-expect (giden-roket 0) (overlay/xy roket 0 -100 arka-plan))

;; Kod
(define giden-roket
  (lambda (x)
     (overlay/xy roket (* -1 x) -100 arka-plan)))
```

`Language > Add Teackpack` menüsünden `universe.ss` isimli eğitim paketini yüklerseniz (`Run` butonuna basmayı unutmayalım), yazdığınız `giden-roket` programını bir anımasyon haline getirebilirsiniz.

```racket
(animate giden-roket)
```