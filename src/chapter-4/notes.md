# Ders Notları

```racket
(define-record-procedures ogrenci
  make-ogrenci
  ogrenci?
  (ogrenci-adi
   ogrenci-soyadi))

(define ben (make-ogrenci "fatih" "koksal"))
(define sen (make-ogrenci "ali" "kurnaz"))

;; Aciklama
;; bir ogrenci alir ve onun tam adini doner

;; Sozlesme
(: tam-adi (ogrenci -> string))

;; Testler
(check-expect (tam-adi ben) "fatih koksal")
(check-expect (tam-adi (make-ogrenci "ali" "kurnaz")) "ali kurnaz")

;; Kod
(define tam-adi
  (lambda (ogrenci)
    (string-append (ogrenci-adi ogrenci) " " (ogrenci-soyadi ogrenci))))
```

```racket
(define-record-procedures nokta
  make-nokta
  nokta?
  (nokta-x
   nokta-y))

;; koordinat duzlemi uzerinde 2 nokta alir ve aralarindaki uzakligi doner

(: iki-nokta-arasindaki-uzaklik (nokta nokta -> rational))

(check-expect (iki-nokta-arasindaki-uzaklik (make-nokta 4 0) (make-nokta 0 3)) 5)
(check-expect (iki-nokta-arasindaki-uzaklik (make-nokta 0 0) (make-nokta 3 0)) 3)
(check-expect (iki-nokta-arasindaki-uzaklik (make-nokta 0 0) (make-nokta 0 2)) 2)
(check-within (iki-nokta-arasindaki-uzaklik (make-nokta 5 0) (make-nokta 0 3)) (sqrt 34) 0.0000001)

(define iki-nokta-arasindaki-uzaklik
  (lambda (n1 n2)
    (sqrt (+ (karesi (- (nokta-y n2) (nokta-y n1))) (karesi (- (nokta-x n2) (nokta-x n1)))))))

(define karesi
  (lambda (x)
    (* x x)))
```