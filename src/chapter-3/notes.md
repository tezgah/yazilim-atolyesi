# Ders Notları

Önceki derslerimizde çok basit işler yapan fonksiyonlar tanımladık. Bazılarını kolayca yazabildik, ancak işler zorlaştıkça hatasız çalışan kodu üretebilmek o kadar da kolay olmayabiliyor. Bir çözüm üretebilmek için öncelikle problemin kendisini analiz edip iyice anlamamız gerekiyor. İşte bu aşamada `Tasarım Reçetesi` diye adlandırdığımız bir yöntem bize yardımcı olacak.

## Tasarım Reçetesi

Tasarım reçetesi, problem tanımından yola çıkarak kendimize sorular sora sora, sorunu çözen bir programa ulaşmak için adım adım ilerleyebileceğimiz bir süreç sunar.

Bu yaklaşımın yeniliği, başlangıç seviyesi programlar için ara ürünlerin oluşturulmasıdır. Öğrenci problemi çözerken zorlandığında, bir uzman veya bir eğitmen mevcut ara ürünleri inceleyerek yardımcı olabilir. Bunun için tasarım sürecindeki genel soruları kullarak öğrencinin problemi daha iyi anlamasına yardımcı olur ve hatasını kendisinin bulmasını sağlamaya çalışır. İşte bu kendi kendini güçlendiren süreç, programlama ve program tasarımı arasındaki temel farktır.

Şimdi de tasarım reçetesini oluşturan ara ürünleri inceleyelim:

### 1. Açıklama

İlk olarak fonksiyonun önüne bu fonksiyonun neyi hesapladığı anlatan kısa ve öz bir açıklama yazarız. Örnek vermek gerekirse;

```
; Bir sayı alır ve bu sayının karesini hesaplar.
```

Dikkatinizi çektiyse açıklama kısmina `;` işareti ile başladık. Yani açıklamayı yorum satırı olarak yazdık. Dolayısı ile bu kısım bilgisayar için değil, biz insanlar için. Bu fonksiyonu kullanmak isteyen biri kodu inceleme gereği duymadan açıklama kısmına okuyarak fonksiyonun ne iş yaptığını anlayabilmeli.


### 2. Sözleşme (İmza)

İkinci olarak, programın okunabilirliğini arttırmak için fonksiyonun parametre olarak ne tür veriler beklediğini ve ne tür bir veri döndüğünü açıklayan `sözleşme` ile destekleriz. Bir fonksiyonun sözleşmesi şu formdadır:

```
(: fonksiyonun-adı (parametre1 . . . parametreN -> çıktı))
```
Sözleşme fonksiyonu (`:`) ilk parametre olarak fonksiyonun adını, ikinci parametre olarak da imzasını alır. Bu imzanın ortasında bir ok `->` vardır ve bu nedenle bir fonksiyonu temsil eder. Okun solunda fonksiyonun giriş olarak kabul ettiği değerler, okun sağında ise çıkan değer bulunur. Örnek olarak:

```
(: karesi (number -> number))
```

Bu sözleşme şu şekilde okunabilir: "Karesi, girdi olarak doğal sayı kabul eden ve çıktı olarak doğal sayı üreten bir fonksiyondur."

### 3. Testler

Fonksiyonun sözleşmesine ve açıklamasına uygun örnekler kullanarak testler yazarak fonksiyonun tüm testleri geçtiğinden emin oluruz. Problemi daha iyi anlayabilmek ve hataları önceden keşfedebilmek için bu adım çok önemlidir. Testler ayrıca, fonksiyonu sonradan kullanmak isteyenlerin fonksiyonun ne yaptığını anlamasına yardımcı olur. Bir fonksiyonu test etmenin yollarından bir tanesi `check-expect` fonksiyonunu kullanmaktır:

```
(check-expect (fonksiyonun-adı parametre1 ... parametreN) sonuç)
```

`check-expect` fonksiyonu (yukarıda da görülebileceği üzere) 2 parametre alır: ifade ve beklenen ifade. Eğer bu iki ifade aynı sonucu üretiyorsa testi geçtik demektir. Aksi takdirde hata üretir.

Örnek vermek gerekirse:


```
(check-expect (karesi 0) 0)
(check-expect (karesi 2) 4)
(check-expect (karesi -3) 9)
```

### 4. Kod

Artık çözülmesi gereken problemi daha iyi anladığımıza göre önce fonksiyonun şablonunu, sonra da kodunu yazabiliriz.

Şablon:

```
(define karesi
   (lambda (x)
     (...)))
```

Kod:

```
(define karesi
   (lambda (x)
     (* x x)))
```

## Örnekler

Tasarım reçetesini kullanmayı öğrenmek için, önceki derslerimizde tanımladığımız fonksiyonları şimdi bir de bu yöntemi kullanarak tanımlayalım. Her bir aşamada tasarım reçetesinin sorularını kendimize soralım ve ara ürünleri oluşturalım. Böylece adım adım çözüme ilerleyeceğiz.


```racket
;; Soru 1: Bu fonksiyon neyi hesaplıyor?
;; Açıklama
;; Verilen sayının karesini hesaplar

;; Soru 2: Bu fonksiyon ne tür veriler tüketiyor ve üretiyor?
;; Sözleşme
(: karesi (integer -> integer))

;; Soru 3: Bir kaç örnek verebilir misin?
;; Testler
(check-expect (karesi 0) 0)
(check-expect (karesi 2) 4)
(check-expect (karesi -3) 9)

;; Soru 4: Son olarak fonksiyonun şablonunu ve kodunu yazabilir misin?
;; Şablon
;; (define karesi
;;   (lambda (x)
;;     (...)))
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
