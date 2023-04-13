# Ders Notları


## Tasarım Reçetesi

### 1. Açıklama

İstenen işlevin ne tür verileri tükettiğini ve ürettiğini belirtin. Fonksiyonun neyi hesapladığı sorusuna kısa ve öz bir cevap formüle edin. İmzaya uygun bir taslak tanımlayın.


### 2. Sözleşme

### 3. Testler

### 4. Kod


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
