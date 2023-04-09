# Ders Notları

## animate

Hatırlayacak olursanız `animate` zaman bazlı similasyonlar üretebileceğimiz basit bir animasyon fonksiyonu. Programcının görevi verilen her bir doğal sayı için `image` üretecek fonksiyonu sağlamak. Bu fonksiyonu `animate` fonksiyonuna veridiğimizde bize similasyonu gösteriyor. İmzası şu şekilde:

```
; (: animate ((natural -> image) -> natural))
```

`animate` fonksiyonu bir tuval açar ve saniyede 28 kez işleyen bir saat başlatır. Saatin her vuruşunda DrRacket fonksiyonun çağırımından o zamana kadar geçen vuruş sayısını resmi çizecek olan fonksiyona uygular. Üretilen her bir resmi tuvalde tek tek gösterir. Saniyede 28 kere olan bu işlem çok hızlı olduğundan biz oluşan resimleri sanki bir anımasyon gibi görürüz. Similasyon siz açılan pencereyi kapatana kadar devam eder ve geri dönüş değeri olarak o zamana kadar geçen vuruş sayısını doğal sayı olarak geri döner.

Burada dikkatimizi ilk çeken şu olmalı. `animate` fonksiyonu başka bir fonksiyonu parametre olarak alıyor. Bu daha önce kullandığımız fonksiyonların imzalarına pek benzemiyor. Normalde fonksiyonlar `string`, `natural`, `number`, `boolean` ya da `image` gibi basit veri yapılarını parametre olarak alırlar. Ama burada da gördüğümüz üzere başka fonksiyonları parametre olarak kabul eden fonksiyonlar da var. `animate` fonksiyonunu imzasından da görülebileceği gibi öyle herhangi bir fonksiyonu parametre olarak kabul etmiyor. `natural` alan ve `image` ureten bir fonksiyon olmalı.

İlk olarak `animate` fonksiyonunun nasıl çalıştığını anlamamıza da yardımcı olacak basit bir örnekle başlayalım:

```
(: kirmizi-rakam (natural -> image))

(define kirmizi-rakam
  (lambda (t)
    (place-image 
     (text (number->string t) 100 "red")
     100 100
     (empty-scene 200 200))))
```

`kirmizi-rakam` fonksiyonu `animate` in bizden istediği gibi bir doğal sayı alıyor ve resim üretiyor. Ürettiği resim işe verilen doğal sayının 200x200 boyutlarında boş bir sahneye kırmızı renkte yazılmış hali. `kirmizi-rakam` fonksiyonuna değişik doğal sayı değerleri vererek ürettiği resmi inceleyebilirsiniz.

```
(kirmizi-rakam 42)
```
Şimdi `animate` kullanarak similasyonu başlatabiliriz:

```
(animate kirmizi-rakam)
```

Gördüğünüz gibi `animate` fonksiyonu similasyonu başlatarak doğal sayıları tek tek `kirmizi-rakam` fonksiyonuna veriyor. `kirmizi-rakam` fonksiyonu ise kendisine verilen doğal sayıları kanvas'a çiziyor.

Başka bir örnek ile öğrendiklerimizi pekiştirelim:

```
(define ufo-sahnesi
  (lambda(h)
    (underlay/xy (rectangle 100 100 "solid" "white") 50 h UFO)))
 
(define UFO
  (underlay/align "center"
                  "center"
                  (circle 10 "solid" "green")
                  (rectangle 40 4 "solid" "green")))
 
(animate ufo-sahnesi)
```
`ufo-sahnesi` fonksiyonunu dikkatlice incelerseniz UFO'nun nasıl yavaş yavaş aşağıda indiğini göreceksiniz.

## big-bang

`big-bang` daha karmaşık similasyonlar üretebileceğimiz bir fonksiyon. Bir çok parametre alıyor, ancak biz şimdilik isteğe bağlı olan parametrelerden sadece 2 tanesini kullanacağız. İmzası şu şekilde:


```
;(big-bang
;  <başlangıç-hali>
;  (on-tick ...)
;  (to-draw ... ... ...)
;  ....)
```

`big-bang` ılk parametre olan başlangıç halini kullanarak bir sahne başlatıyor ve saati çalıştırıyor. Saat her vurduğunda `on-tick` fonksiyonuna parametre olarak verdiğimiz fonksiyona sahnenin suanki halini verip bir sonraki sahnede dünyanın nasıl değiştiğini öğreniyor. `to-draw` fonksiyona verdiğimiz fonksiyonu kullarak da dünyamızı sahneye çiziyor.

Kulağa çok karmaşık gelen bu tanımı anlayabilmek için daha önce `animate` fonksiyonu ile yaptığımız similasyonun aynısını şimdi bir de `big-bang` ile yapalım.

```
(define 1ekle
  (lambda (t)
    (+ 1 t)))

(big-bang
  1
  (on-tick 1ekle)
  (to-draw kirmizi-rakam))
```
Animasyonumuzda değişen tek şey sahnede göstereceğimiz sayı olduğundan, dünyamızı temsilen sadece bir doğal sayı kullanıyoruz. Başlangıç hali için `1` verdik. Saat her vurduğunda bu sayının bir artmasını istedik. Sahneye çizmek için ise daha önce tanımladığımız `kirmizi-rakam` fonksiyonunu kullandık. `big-bang` ile similasyonu çalıştırdığımızda daha önce `animate` ile yaptığımızın aynısını üretmiş olduk.

Şimdi çok daha karmaşık bir animasyon yapalım. İçinde bulunduğu odanın duvarlarından seke seke ilerleyen kırmızı bir top yapacağız. Daha öncekine göre daha karmaşık olan bu animasyonu üretmek için dünyamızı temsilen kullandığımız veri yapısı da daha karmaşık olacak.

```
(define-record-procedures top
  make-top
  top?
  (top-x
   top-y
   top-x-yon
   top-y-yon))
```

`top`'un 4 niteliği var. x-koordinatını temsilen `top-x`, y-koordinatını temsilen `top-y`, topun x-koordunatındaki ivmesini temsilen `top-x-yon` ve y-koordinatındaki ivmesini temsilen `top-y-yon`.

Saat her vurduğunda dünyamızın nasıl değişmesini istediğimizi `tick` fonksiyonunda tanımlayalım.

```
(: tick (top -> top))

(check-expect (tick (make-top 1 50 2 1)) (make-top 3 51 2 1))
(check-expect (tick (make-top 3 51 2 1)) (make-top 5 52 2 1))
(check-expect (tick (make-top 5 52 2 1)) (make-top 7 53 2 1))
(check-expect (tick (make-top 7 53 2 1)) (make-top 9 54 2 1))
(check-expect (tick (make-top 0 53 -1 1)) (make-top 1 53 1 1))
(check-expect (tick (make-top -4 67 -5 2)) (make-top 1 67 5 2))
(check-expect (tick (make-top 200 50 3 2)) (make-top 199 50 -3 2))
(check-expect (tick (make-top 7 0 5 -2)) (make-top 7 1 5 2))
(check-expect (tick (make-top 5 -1 4 -2)) (make-top 5 1 4 2))
(check-expect (tick (make-top 50 200 2 3)) (make-top 50 199 2 -3))

(define tick
  (lambda(t)
    (cond
      ((<= (top-x t) 0) (make-top 1 (top-y t) (* -1 (top-x-yon t)) (top-y-yon t)))
      ((>= (top-x t) 200) (make-top 199 (top-y t) (* -1 (top-x-yon t)) (top-y-yon t)))
      ((<= (top-y t) 0) (make-top (top-x t) 1 (top-x-yon t) (* -1 (top-y-yon t))))
      ((>= (top-y t) 200) (make-top (top-x t) 199 (top-x-yon t) (* -1 (top-y-yon t))))
      (else (make-top (+ (top-x t) (top-x-yon t)) (+ (top-y t) (top-y-yon t)) (top-x-yon t) (top-y-yon t)))))) 
```

Topun duvarlara çarpınca geri sekmesini, çarpmadığı zamanlarda ise x ve y koordunatlarındaki ivmelerine göre hareket etmesini istiyoruz.

Dünyamızı çizmek için ise basitçe topu boş bir sahne üzerinde x ve y koordinatına yerleştirelim.

```
(: draw (top -> image))

(define draw
  (lambda (t)
    (place-image 
     (circle 10 "solid" "red")
     (top-x t) (top-y t)
     (empty-scene 200 200))))
```

Ve similasyonu baslatalim:

```
(big-bang
    (make-top 0 200 8 8)
  (on-tick tick)
  (to-draw draw 200 200))
```