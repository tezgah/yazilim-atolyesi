# Ders Notları

Bazen hesaplamaların duruma bağlı olarak farklı şekilde ilerlemesi gerekebilir. Örneğin, zıplayan top animasyonu yaptığımızı düşünelim. Top serbest düşüş yaparken program normal bir şekilde ilerler, ancak top canvas'ın alt kenarına eriştiğinde onun daha fazla aşağı inmesini engellememiz gerekir.


```racket
;; Yaşı verilen bir kişinin ehliyet alıp alamayacağını söyler.
;; Bir sayı alır, verilen sayı 17'ye eşit ya da daha büyükse #t
;; değil ise #f döner

(: ehliyet-alabilir (integer -> boolean))

(check-expect (ehliyet-alabilir 1) #f)
(check-expect (ehliyet-alabilir 16) #f)
(check-expect (ehliyet-alabilir 17) #t)
(check-expect (ehliyet-alabilir 39) #t)

(define ehliyet-alabilir
  (lambda (yaş)
    (cond
      [(< yaş 17) #f]
      [else #t])))
```

```racket
;; Verilen yaş değerine göre hangi okula gidilmesi gerektiğini söyler.
;; < 0 -> "Okul yok"
;; 0 - 5 -> "Kindergarten"
;; 6 - 10 -> "Grundschule"
;; 11 - 19 -> "Gymnasium"
;; 20 -> ... -> "Universitaet"

(check-expect (hangi-okul 5) "Kindergarten")
(check-expect (hangi-okul 6) "Grundschule")
(check-expect (hangi-okul 10) "Grundschule")
(check-expect (hangi-okul 11) "Gymnasium")
(check-expect (hangi-okul 14) "Gymnasium")
(check-expect (hangi-okul 60) "Universitaet")
(check-expect (hangi-okul 19) "Gymnasium")
(check-expect (hangi-okul 20) "Universitaet")

(: hangi-okul (integer -> string))

(define hangi-okul
  (lambda (yaş)
    (cond
      [(<= yaş 0) "Okul yok"]
      [(<= yaş 5) "Kindergarten"]
      [(and (>= yaş 6) (<= yaş 10)) "Grundschule"]
      [(and (>= yaş 11) (<= yaş 19)) "Gymnasium"]
      [else "Universitaet"])))
```

```racket
;; Bir sayı alır ve trafik lambası resmi üretir.
;; Verilen sayı;
;; 10'a eşit ya da daha küçükse kırmızı ışık,
;; 12'ye eşit ya da daha küçükse sarı ışık,
;; 17'ye eşit ya da dağa küçükse yeşik ışık,
;; 19'a eşit ya da daha küçükse sarı ışık,
;; 19'dan daha sonra da kırmızı ışık yanıyormuş gibi gösterir.

(: trafik-lambasi-sn (rational -> image))

(check-expect (trafik-lambasi-sn 1) kirmizi-isik)
(check-expect (trafik-lambasi-sn 9) kirmizi-isik)
(check-expect (trafik-lambasi-sn 10) kirmizi-isik)
(check-expect (trafik-lambasi-sn 11) sari-isik)
(check-expect (trafik-lambasi-sn 12) sari-isik)
(check-expect (trafik-lambasi-sn 13) yesil-isik)
(check-expect (trafik-lambasi-sn 17) yesil-isik)
(check-expect (trafik-lambasi-sn 18) sari-isik)
(check-expect (trafik-lambasi-sn 19) sari-isik)
(check-expect (trafik-lambasi-sn 20) kirmizi-isik)
(check-expect (trafik-lambasi-sn 180) kirmizi-isik)

(define kirmizi (circle 40 "solid" "red"))
(define sari (circle 40 "solid" "yellow"))
(define yesil (circle 40 "solid" "green"))
(define gri (circle 40 "solid" "grey"))
(define lamba (rectangle 100 260 "solid" "black"))
(define kirmizi-isik (overlay (above kirmizi gri gri) lamba))
(define sari-isik (overlay (above gri sari gri) lamba))
(define yesil-isik (overlay (above gri gri yesil) lamba))

(define trafik-lambasi-sn
  (lambda (sn)
    (cond
      ((<= sn 10) kirmizi-isik)
      ((<= sn 12) sari-isik)
      ((<= sn 17) yesil-isik)
      ((<= sn 19) sari-isik)
      (else kirmizi-isik))))


;; Bir sayı alır ve bu sayıyı 28'e bölerek trafik-lambasi-sn fonksiyonunu çağırarak bir resim üretir.
;; animate fonksiyonu her saniyede 28 kare resim gösterdiği için böyle bir yöntem kullanıyoruz.

(: trafik-lambasi (integer -> image))

(check-expect (trafik-lambasi 0) kirmizi-isik)
(check-expect (trafik-lambasi 1) kirmizi-isik)
(check-expect (trafik-lambasi 308) sari-isik)

(define trafik-lambasi
  (lambda (sn)
    (trafik-lambasi-sn (/ sn 28))))
```