# Ders Notları

Önceki derslerimizde çok basit işler yapan fonksiyonlar tanımladık. Bazılarını kolayca yazabildik, ancak işler zorlaştıkça hatasız çalışan kodu üretebilmek o kadar da kolay olmayabiliyor. Bir çözüm üretebilmek için öncelikle problemin kendisini analiz edip iyice anlamamız gerekiyor. İşte bu aşamada `Tasarım Reçetesi` diye adlandırdığımız bir yöntem bize yardımcı olacak.

## Tasarım Reçetesi

Tasarım reçetesi, problem tanımından yola çıkarak kendimize sorular sora sora, sorunu çözen bir programa ulaşmak için adım adım ilerleyebileceğimiz bir süreç sunar.

Bu yaklaşımın yeniliği, başlangıç seviyesi programlar için ara ürünlerin oluşturulmasıdır. Bir acemi sıkıştığında, bir uzman veya bir eğitmen mevcut ara ürünleri inceleyebilir. Teftiş muhtemelen tasarım sürecindeki genel soruları kullanır ve böylece acemiyi kendini düzeltmeye yönlendirir. Ve bu kendi kendini güçlendiren süreç, programlama ve program tasarımı arasındaki temel farktır.

### 1. Açıklama

Fonksiyonun neyi hesapladığı sorusuna kısa ve öz bir cevap verin.

### 2. Sözleşme

İstenen fonksiyonun ne tür verileri tükettiğini ve ürettiğini belirtin. 

### 3. Testler

Fonksiyonun sözleşmesine ve açıklamasına uygun örnekler kullanarak testler yazın ve fonksiyonun tüm testleri geçtiğinden emin olun. Problemi daha iyi anlayabilmek ve hataları önceden keşfedebilmek için bu adım çok önemlidir. Testler ayrıca, ihtiyaç duyulduğunda diğerlerinin fonksiyonu anlamasına yardımcı olur.

### 4. Kod

Artık çözülmesi gereken problemi daha iyi anladığımıza göre önce fonksiyonun şablonunu, sonra da kodunu yazabiliriz.

## Örnekler


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
