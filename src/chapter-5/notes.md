# Ders Notları

```racket
(define-record-procedures ogrenci
  make-ogrenci
  ogrenci?
  (ogrenci-adi
   ogrenci-soyadi))

(define ben (make-ogrenci "fatih" "koksal"))
(define sen (make-ogrenci "ali" "kurnaz"))

;; Açıklama
;; Bir öğrenci alır ve onun tam adını döner

;; Sozlesme
(: tam-adi (ogrenci -> string))

;; Testler
(check-expect (tam-adi ben) "fatih koksal")
(check-expect (tam-adi sen) "ali kurnaz")
(check-expect (tam-adi (make-ogrenci "yusuf islam" "dagdelen")) "yusuf islam dagdelen")

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

;; İki nokta alır ve birbirleri ile toplayarak yeni bir nokta döner
;; İki noktanın toplamı x ve y koordinatlarının toplamına eşittir

(: iki-nokta-toplami (nokta nokta -> nokta))

(check-expect (iki-nokta-toplami (make-nokta 1 1) (make-nokta 2 2)) (make-nokta 3 3))
(check-expect (iki-nokta-toplami (make-nokta 0 4) (make-nokta 1 3)) (make-nokta 1 7))

(define iki-nokta-toplami
  (lambda (n1 n2)
      (make-nokta (+ (nokta-x n1) (nokta-x n2)) (+ (nokta-y n1) (nokta-y n2)))))
```

```racket
(define-record-procedures nokta
  make-nokta
  nokta?
  (nokta-x
   nokta-y))

;; Koordinat duzlemi uzerinde 2 nokta alir ve aralarindaki uzakligi hesaplar

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